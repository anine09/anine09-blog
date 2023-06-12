---
title: 机器学习基础
tags: LikelihoodLab
mathjax: true
mathjax_autoNumber: true
---
1. 训练数据从哪里来，中间有那些状态（类型、shape、存储格式等）
2. 训练过程中数据顺序的随机性如何保证？
3. 模型参数存在哪里，如何查看所有参数的类型、shape
4. 训练过程中梯度在哪里存，哪个函数修改了模型参数
5. model.train() / model.eval() 具体做了什么，对程序有什么影响

# 训练数据从哪里来，中间有那些状态（类型、shape、存储格式等）
数据来自 [THE MNIST DATABASE](http://yann.lecun.com/exdb/mnist/)，原始数据是二进制文件，主流框架都对其进行了封装，例如 [paddle.vision.datasets.MNIST](https://www.paddlepaddle.org.cn/documentation/docs/zh/api/paddle/vision/datasets/MNIST_cn.html) 和 [torchvision.datasets.MNIST](https://pytorch.org/vision/stable/generated/torchvision.datasets.MNIST.html)，特别 `Pytorch` 有一个 `mnist` 系列的小工具，可以直接下载并读取 MNIST 的图像与标签文件。

根据 MNIST 官方网站给出的[文件格式规则](http://yann.lecun.com/exdb/mnist/#:~:text=FILE%20FORMATS%20FOR%20THE%20MNIST%20DATABASE)，实现了一个读取原始 MNIST 文件的函数：


```python
def read_mnist_raw_file(path: str, type: str) -> dict:
    if type not in ["image", "label"]:
        raise ValueError("type`s value is only can be 'image' or 'label'.")

    with open(path, "rb") as f:
        raw_content = f.read()
        hex_content = binascii.hexlify(raw_content)

    if type == "label":
        magic_number = b"00000801"
    elif type == "image":
        magic_number = b"00000803"
    else:
        raise ValueError("It can not solve this problem.")
    
    if hex_content[:8] != magic_number:
        raise ValueError("This file is not SN3 format.")
    data_len = int(hex_content[8:16], 16)

    if type == "label":
        data_list = list()
        for i in range(16, data_len*2+16, 2):
            data_list.append(int(hex_content[i:i+2], 16))

    elif type == "image":
        data_list = list(list())
        ...
    
    return {"data_len": data_len, "data_list": data_list}
```
以 `train-labels-idx1-ubyte` 文件为例，其格式约定如下：
![][1]



# 训练过程中数据顺序的随机性如何保证？
# 模型参数存在哪里，如何查看所有参数的类型、shape
# 训练过程中梯度在哪里存，哪个函数修改了模型参数
# model.train() / model.eval() 具体做了什么，对程序有什么影响


[1]:../assets/images/2023-06-09-Likelihood-Lab-2023-AIGC-Q1/Training_Set_Label_File.png