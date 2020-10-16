---
title: OpenGL
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-05 16:54:13
---
> 

<!-- more -->
# 简介
## 介绍
- [官方网址](https://www.opengl.org)

## 环境安装

- [下载glew](https://www.opengl.org/sdk/libs/GLEW)
- [GLM](http://glm.g-truc.net)
- [SDL](https://www.libsdl.org/release/)

# OpenGL渲染管道
## 整体流程
- byte data->Vertex Shader->Rasterization->Fragment shalder->output image->I/O

## 顶点着色器Vertex Shader
- 生成空间点

## 光栅化Rasterization
- 对点进行三角化并填充

## 片元着色器Fragment shalder
- 为三角形每个顶点生成颜色信息

## output image
- 结合顶点信息和颜色信息输出最终图像

# 例子
- [代码链接](https://github.com/hero576/gl_test)

