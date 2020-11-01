---
title: Open3D
author: hero576
tags:
  - robotic
categories:
  - programme
date: 2020-10-27 01:11:04
---
> 

<!-- more -->

# 简介
- [官网](http://www.open3d.org/)
## 安装
- [安装教程](http://www.open3d.org/docs/release/compilation.html)


### conda安装
- 查找open3d的安装信息，要依据自己的系统，选择合适的包

```bash
anaconda search -t conda open3d
```

- 显示open3d的安装信息
```bash
anaconda show open3d-admin/open3d
```

- 依据提示，运行
```bash
conda install --channel https://conda.anaconda.org/open3d-admin open3d
```


### linux

# 示例[^1]
[^1]: [链接](https://blog.csdn.net/suyunzzz/article/details/105183824)

## 可视化点云
```py
# 使用open3d可视化点云
# 使用open3d可视化点云,返回pointcloud类型示例
from open3d.open3d.utility import Vector3dVector
from open3d.open3d.geometry import PointCloud
from open3d.open3d.visualization import draw_geometries
def visualize(pointcloud):
    # from open3d_study import *
    # points = np.random.rand(10000, 3)
    point_cloud = PointCloud()
    point_cloud.points = Vector3dVector(pointcloud[:,0:3].reshape(-1,3))
    draw_geometries([point_cloud],width=800,height=600)
    return point_cloud
```

##使用knn搜索点云，并指定颜色
```py
#创建点云对象
pcd=o3d.geometry.PointCloud()
pcd.points=o3d.utility.Vector3dVector(pt[:,0:3])
pcd.paint_uniform_color([0.5, 0.5, 0.5])  # 给全部点云上色，灰色
pcd_tree=o3d.geometry.KDTreeFlann(pcd)  # 创建一个实例 pcd_tree以使用KDTree
[k, index, _] = pcd_tree.search_knn_vector_3d(pcd.points[K], 50)
np.asarray(pcd.colors)[index[1:], :] = [0, 1, 0]   # 给50以内的点设置颜色为green
```

- ps:colors和points是PointCloud中的两个矩阵

## 可视化点云+label
- 可视化点云，并根据标签赋值颜色
```py
def vis(data,label):
    '''
    :param data: n*3的矩阵
    :param label: n*1的矩阵
    :return: 可视化
    '''
    data=data[:,:3]
    labels=np.asarray(label)
    max_label=labels.max()
    # 颜色
    colors = plt.get_cmap("tab20")(labels / (max_label if max_label > 0 else 1))
    pt1 = o3d.geometry.PointCloud()
    pt1.points = o3d.utility.Vector3dVector(data.reshape(-1, 3))
    pt1.colors=o3d.utility.Vector3dVector(colors[:, :3])
    o3d.visualization.draw_geometries([pt1],'part of cloud',width=500,height=500)
```

## 可视化两个点云
- 可视化两个点云并赋予不同的颜色
```py
def vis_cloud(a,b):
	# a:n*3的矩阵
	# b:n*3的矩阵
    pt1=o3d.geometry.PointCloud()
    pt1.points=o3d.utility.Vector3dVector(a.reshape(-1,3))
    pt1.paint_uniform_color([1,0,0])
    pt2=o3d.geometry.PointCloud()
    pt2.points=o3d.utility.Vector3dVector(b.reshape(-1,3))
    pt2.paint_uniform_color([0,1,0])
    o3d.visualization.draw_geometries([pt1,pt2],window_name='cloud[0] and corr',width=800,height=600)
```

## 保存与读取view point
- 参考
```py
# Open3D: www.open3d.org
# The MIT License (MIT)
# See license file or visit www.open3d.org for details
# examples/python/visualization/load_save_viewpoint.py
import numpy as np
import open3d as o3d
def save_view_point(pcd, filename):
    vis = o3d.visualization.Visualizer()
    vis.create_window()
    vis.add_geometry(pcd)
    vis.run()  # user changes the view and press "q" to terminate
    param = vis.get_view_control().convert_to_pinhole_camera_parameters()
    o3d.io.write_pinhole_camera_parameters(filename, param)
    vis.destroy_window()
def load_view_point(pcd, filename):
    vis = o3d.visualization.Visualizer()
    vis.create_window()
    ctr = vis.get_view_control()
    param = o3d.io.read_pinhole_camera_parameters(filename)
    vis.add_geometry(pcd)
    ctr.convert_from_pinhole_camera_parameters(param)
    vis.run()
    vis.destroy_window()
if __name__ == "__main__":
    pcd = o3d.io.read_point_cloud("../../test_data/fragment.pcd")
    save_view_point(pcd, "viewpoint.json")
    load_view_point(pcd, "viewpoint.json")
```
