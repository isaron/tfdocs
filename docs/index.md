# TensorFlow 1.0 官方文档中文翻译

首先申明，本文档并非官方文档，仅为个人兴趣而建立的。如需查询TensorFlow官方文档，请至https://www.tensorflow.org/，或者到TensorFlow中文社区（http://www.tensorfly.cn/）查询完整的中文文档。

本文档基于[TensorFlow官方文档](https://www.tensorflow.org/) [1.0 版本]()进行翻译，尽量与官方网站的目录保持一致。

文档翻译过程中克隆了[官方文档库](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/g3doc)（尽量保持跟随最新更新），同时严重参考了[TensorFlow中文社区](http://www.tensorfly.cn/)的文档（该社区的文档仍为0.5版，这也是促使本文档项目的一个原因）。

本文档使用MkDocs文档系统[mkdocs.org](http://mkdocs.org)构建，界面主题为[Read the Docs](https://readthedocs.org/)。


## 文档目录

### 1.安装

* [安装综述](install/install-index.md)
* 在Ubuntu上安装TensorFlow
* 在Mac OS X上安装TensorFlow
* 在Windows上安装TensorFlow
* 源代码安装TensorFlow
* 迁移到TensorFlow 1.0

### 2.开发

#### 2.1新手入门

* TensorFlow快速入门
* MNIST入门
* MNIST进阶
* TensorFlow 运作方式 101
* tf.contrib.learn: 快速开始
* tf.contrib.learn: 创建输入函数
* TensorBoard: 可视化学习
* TensorBoard: 内嵌可视化
* TensorBoard: 图表可视化
* tf.contrib.learn: 日志和监控基础
* TensorFlow版本

#### 2.2程序员手册

* 读取数据
* 线程和队列
* 共享参数
* TensorFlow的版本语义
* TensorFlow数据版本化: GraphDefs and Checkpoints
* Supervisor: Training Helper for Days-Long Trainings.
* TensorFlow Debugger (tfdbg) 命令行指南: MNIST
* TensorFlow Debugger (tfdbg) 与 tf.contrib.learn 结合使用
* 导入导出MetaGraph
* 常见问题
* Tensor Ranks, Shapes, and Types
* Variables: Creation, Initialization, Saving, and Loading

#### 2.3教程

* Mandelbrot Set
* Partial Differential Equations
* Convolutional Neural Networks
* Image Recognition
* How to Retrain Inception’s Final Layer for New Categories
* Vector Representations of Words
* Recurrent Neural Networks
* Sequence-to-Sequence Models
* A Guide to TF Layers: Building a Convolutional Neural Network
* Large-scale Linear Models with TensorFlow
* TensorFlow Linear Model Tutorial
* TensorFlow Wide & Deep Learning Tutorial
* Using GPUs

#### 2.4性能优化

* Performance
* XLA Overview
* Broadcasting semantics
* Developing a new backend for XLA
* Using JIT Compilation
* Operation Semantics
* Shapes and Layout
* Using AOT compilation
* How to Quantize Neural Networks with TensorFlow

### 3.API r1.0

* Overview r1.0
* Python API r1.0
* C++ API r1.0
* Java API r1.0
* Go API

### 4.部署

* How to run TensorFlow on Hadoop
* Distributed TensorFlow
* TensorFlow Serving

### 5.扩展

* TensorFlow Architecture
* Adding a New Op
* Adding a Custom Filesystem Plugin
* TensorFlow in other languages
* Custom Data Readers
* Creating Estimators in tf.contrib.learn
* A Tool Developer’s Guide to TensorFlow Model Files

### 6.资源

#### 6.1社区

* Writing TensorFlow Documentation
* TensorFlow Style Guide

#### 6.2关于

* Additional Resources
* TensorFlow White Papers
* TensorFlow Uses

### 7.版本

* TensorFlow Versions
* master
* r1.0
* r0.12
* r0.11
* r0.10
