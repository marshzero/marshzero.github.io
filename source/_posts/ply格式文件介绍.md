---
title: ply格式文件介绍
date: 2020-10-27 09:51:24
categories: 
- 点云
tags: 
- 点云
- 数据
---
## ply文件介绍
PLY是一种三维mesh模型数据格式，全名为多边形档案（Polygon File Format）或 斯坦福三角形档案（Stanford Triangle Format）。 

该格式主要用以储存立体扫描结果的三维数值，透过多边形片面的集合描述三维物体，与其他格式相较之下这是较为简单的方法。它可以储存的资讯包含颜色、透明度、表面法向量、材质座标与资料可信度，并能对多边形的正反两面设定不同的属性。

在档案内容的储存上PLY有两种版本，分别是纯文字（ASCII）版本与二元码（binary）版本，其差异在储存时是否以ASCII编码表示元素资讯。  
ply文件的基本格式如下
```
This is the structure of a typical PLY file:

  Header
  Vertex List
  Face List
  (lists of other elements)
```
从ply到end_header是头部，element可以有多个property，例如vertex可以有xyz三个property。ply示例如下：
```
ply
format ascii 1.0           { ascii/binary, 编码类型 }
comment made by Greg Turk  { comments keyword specified, like all lines }
comment this file is a cube
element vertex 8           { 定义 "vertex" 元素, 8 代表这个文件有八个顶点信息 }
property float x           { vertex contains float "x" coordinate }
property float y           { y coordinate is also a vertex property }
property float z           { z coordinate, too }
element face 6             { 定义 "face" 元素, 6 代表这个文件有六个面信息 }
property list uchar int vertex_index { "vertex_indices" is a list of ints }
end_header                 { delimits the end of the header }
0 0 0                      { start of vertex list }
0 0 1
0 1 1
0 1 0
1 0 0
1 0 1
1 1 1
1 1 0
4 0 1 2 3                  { start of face list }
4 7 6 5 4
4 0 4 5 1
4 1 5 6 2
4 2 6 7 3
4 3 7 4 0
```

这里是一个对ply文件的解释 
http://paulbourke.net/dataformats/ply/  
这里有一个ply文件的示例
http://paulbourke.net/dataformats/ply/example1.ply
## 代码示例
```python
from plyfile import PlyData
import os
import numpy as np
```
使用plydata.read读取ply文件，可以看到ply_data是头部信息
```python
ply_data = PlyData.read('../repo/bun_zipper.ply')
print(ply_data)
```
    # 输出
    ply
    format ascii 1.0
    comment zipper output
    element vertex 35947
    property float x
    property float y
    property float z
    property float confidence
    property float intensity
    element face 69451
    property list uchar int vertex_indices
    end_header
    
读取顶点信息

```python
vertex = ply_data['vertex']
print(vertex)
```
可以看到输出的是顶点vertex及它的property的信息  

    # 输出
    element vertex 35947
    property float x
    property float y
    property float z
    property float confidence
    property float intensity
    
读取面的信息

```python
face = ply_data['face']
print(face)
```
    # 输出
    element face 69451
    property list uchar int vertex_indices
    
将三个坐标合在一起并输出

```python
x = np.asarray(vertex['x'])
y = np.asarray(vertex['y'])
z = np.asarray(vertex['z'])
points = np.stack([x, y, z], axis=1)
print(x.shape)
print(points.shape)
```
    #输出
    (35947,)
    (35947, 3)
    
