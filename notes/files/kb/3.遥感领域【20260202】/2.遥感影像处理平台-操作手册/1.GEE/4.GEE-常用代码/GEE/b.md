GEE中的六边形分析-以统计中国区域NDVI值为例
原创 mu 在小岛学gis的穆 2023-06-26 12:19 发表于澳大利亚
在地学中，我们经常需要将研究区域划分为六边形统计，好处包括但不限于：

空间中六边形可以降低采样偏差（因为六边形是最能形成均匀分布网络的“圆形多边形”）

网格的等距性质有利于空间数据分析

如果研究区是大面积的，由于地球的曲率，六边形网格形状扭曲较少。

当分析包括连接性或移动路径的方面时，六边形更为合适。

六边形最适合邻近分析，六边形的每一边都直接与其他六边形接触，这一特性使得六边形能够更好地描述和模拟物体之间的空间关系。



俺分享一下如何在gee中如何创建六边形格网以及如果进行区域统计

首先加载geemap以及矢量


import ee
import geemap
import math
Map = geemap.Map(center=(30, 120), zoom=4)
#加载全中国矢量
roi = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017').filter("country_co == 'CH'").union().first().geometry()
Map.addLayer(roi, {}, "china!")
Map
定义生成六边形格网的函数，hexGrid 函数接收两个参数，proj 是要用于生成网格的投影，diameter 是每个六边形从边缘到边缘的大小。然后，计算出从六边形中心到顶点的距离 size。然后，获取像素坐标，通过一系列数学表达式，计算出每个像素所在的六边形的i和j坐标。最后，将i和j的坐标转换成一个唯一的“ID”数字，这是通过对i的坐标进行左移32位然后加上j的坐标来实现的。这个唯一的ID就是每个六边形的标识。

#proj:投影，diameter:六边形直径(m)
def hexGrid(proj, diameter):
  #从中心到顶点的距离
  size = ee.Number(diameter).divide(math.sqrt(3))
  #获取像素坐标
  coords = ee.Image.pixelCoordinates(proj)
  #准备用于计算的值
  vals = {
  #交换x和y的位置，以得到平顶六边形而不是尖顶六边形
    "x": coords.select("x"),
    "u": coords.select("x").divide(diameter),  
    "v": coords.select("y").divide(size),  
    "r": ee.Number(diameter).divide(2),
  }
  #计算出每个像素所在的六边形的i和j坐标
  i = ee.Image().expression("floor((floor(u - v) + floor(x / r))/3)", vals)
  j = ee.Image().expression("floor((floor(u + v) + floor(v - u))/3)", vals)
  #将六边形坐标转换为单个的“ID”数字
  cells = i.long().leftShift(32).add(j.long()).rename("hexgrid")
  return cells
使用函数生成格网并创建了一个图像，将指定区域内的像素设置为1，其余像素设置为0。接下来，将这个图像添加到六边形网格图像中，然后将网格图像中的连通组件进行归约，结果是每个连通组件中的最大值。最后，使用这个归约后的图像来更新六边形网格图像的掩膜。

#生成六边形网格，并将不接触区域的单元遮盖掉
grid = hexGrid(ee.Projection('EPSG:3857'), 50000)
regionImg = ee.Image(0).byte().paint(roi, 1)
mask = grid.addBands(regionImg).reduceConnectedComponents(ee.Reducer.max(), "hexgrid", 128)
grid = grid.updateMask(mask)
Map.addLayer(grid.randomVisualizer())
Map

图片

通过 reduceConnectedComponents 方法，计算每个六边形网格内的NDVI值的平均值。这个方法的工作原理是，首先找到图像中的连通区域，连通区域是指相邻像素具有相同值（在这里就是相同的六边形ID）的区域，然后对每个连通区域内的像素值进行归约。这里使用的归约器是 ee.Reducer.mean()，所以是计算每个连通区域内的平均值。最后，结果被保存在新的图像 meanNdvi 中，这个图像的每个像素值是其所在的六边形内的NDVI值的平均值。

ndvi = ee.ImageCollection('MODIS/006/MOD13A2').filterDate('2020-04-01', '2020-05-01').mean().select('NDVI').addBands(grid)
meanNdvi = ndvi.reduceConnectedComponents(ee.Reducer.mean(), "hexgrid", 256)
vis = {
  'min': 0,
  'max': 9000,
  'palette': [
    'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901',
    '66A000', '529400', '3E8601', '207401', '056201', '004C00', '023B01',
    '012E01', '011D01', '011301'
  ]
  }
Map.add_colorbar(vis, label="meanndvi")
Map.addLayer(meanNdvi,vis,'meanndvi')
Map


图片

图片

gee代码

https://code.earthengine.google.com/5b2ebcec6cab710668a0c311667df109
ps:如果想要六边形矢量的话。。建议本地做好再上传

reference

https://pro.arcgis.com/en/pro-app/latest/tool-reference/spatial-statistics/h-whyhexagons.htm
https://gorelick.medium.com/more-buffered-samples-with-hex-cells-b9a9bd36120d

参考链接：https://mp.weixin.qq.com/s/HhQePqqQFpIO3p6oWY6cvQ