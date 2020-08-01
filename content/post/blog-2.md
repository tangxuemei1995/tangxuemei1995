+++
author = "Xuemei"
title = "tensorflow 一维卷积 tf.nn.conv1d 的使用"
date = "2020-08-01"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "python",
    "css",
    "html",
    "themes",
]
categories = [
    "themes",
    "syntax",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++
tendorflow里面怎么用tf.nn.conv1d

基本格式：

tf.nn.conv1d（value,

filters,

stride,

padding,

use_cudnn_on_gpu=None,

data_format=None,

name=None )

我的代码：
        
        conv_weights = tf.get_variable(
            "conv_weights", [self.filter_size, hidden_size, self.filter_num],
            initializer=tf.truncated_normal_initializer(stddev=0.02))
        conv_bias = tf.get_variable(
            "conv_bias", [self.filter_num], initializer=tf.zeros_initializer())
        conv = tf.nn.conv1d(self.word_embeddings,
                            conv_weights,
                            stride=1,
                            padding='SAME',
                            name='conv') #bug [40,12]vs[40,14]
        self.cnn_output_layer = tf.nn.relu(tf.nn.bias_add(conv, conv_bias), name='relu')
    



我的cnn层之前是word_embedding,维度是[batch_size,seq_lenth,embedding_dim]；[40,120,300]

用tf.nn.conv1d的时候

1. value就是我这里的word_embedding。value的格式为：[batch, in_width, in_channels]，batch为样本维，表示多少个样本，in_width为宽度维，表示样本的宽度，in_channels维通道维，表示样本有多少个通道。value其实就是和我的word_embedding是对应的维度。

2. filters：filters的格式为：[filter_width, in_channels, out_channels]。按照value的第二种看法，filter_width可以看作每次与value进行卷积的行数，可以看作是卷积核的维度，in_channels表示value一共有多少列（与value中的in_channels相对应）。out_channels表示输出通道，可以理解为一共有多少个卷积核，即卷积核的数目。这里定义一个conv_weights，就相当于filters。 维度是 [3,hidden_size,filter_num]，第一维就是卷积核的维度；第二维是前面传进来的value的列数，取value的最后一维的维度即可: hidden_size = self.word_embeddings.shape[-1].value；第三维就是卷积核数量。

3. stride:  整数类型，表示步长，卷积核每次向右边移动几个位置。我这里定为1。

4. padding：'SAME' 'VALID'，这两个是有区别的，用不好会报错。我开始用的VALID 一直报错。

5. name：名称。可省略。我这里的名称是"conv"。



结束，撒花！