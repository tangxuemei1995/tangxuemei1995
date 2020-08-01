+++
author = "Xuemei"
title = "tensorflow.python.framework.errors_impl.InvalidArgumentError: Incompatible shapes: [40,12] vs. [40,1]"
date = "2020-07-30"
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

tensorflow.python.framework.errors_impl.InvalidArgumentError: Incompatible shapes: [40,12] vs. [40,14]

今天在LSTM前面加了一层CNN以后，发现出现了以上错误。

然后将CNN中，padding 从‘VALID'改成了'SAME'，就能正常运行了。

....................
原来代码是这样的

conv = tf.nn.conv1d(self.word_embeddings,
conv_weights,
stride=1,
padding='VALID',
name='conv')


修改之后的代码：

conv = tf.nn.conv1d(self.word_embeddings,
conv_weights,
stride=1,
padding='SAME',
name='conv') 

·················

参数含义：
'VALID' = without padding
'SAME' = with zero padding



