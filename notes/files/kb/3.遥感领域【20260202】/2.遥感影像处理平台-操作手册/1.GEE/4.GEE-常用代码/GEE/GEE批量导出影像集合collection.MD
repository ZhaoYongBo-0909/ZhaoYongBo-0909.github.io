0.背景
遇到有几次朋友问我，GEE该如何批量导出一个数据集。


图片



因此，我把自己常用的导出collection方法写出来，可能有人用得到。

1.数据筛选
首先，在GEE中导入自己需要的矢量边界ROI，这个ROI可以上传shp或者自己在gee中勾画。随后选择卫星数据类型、时间。本文选择了2021年之后，覆盖了研究区域的所有Landsat8影像。

Map.addLayer(ROI)
Map.centerObject(ROI,9)
var roi_collection =landsat.filter(ee.Filter.date('2021-01-01','2021-12-31'))
                            .filter(ee.Filter.bounds(ROI))
2.引入批量导出函数
var batch = require('users/fitoprincipe/geetools:batch')
这个函数来自于github的一名大神，他针对gee批量导出做了一个整理，并且做成了API，我们可以直接调用。

图片
该批量导出的具体代码如下所示:

Download.ImageCollection.toDrive = function(collection, folder, options) {
    
    var defaults = {
      scale: 1000,
      maxPixels: 1e13,
      type: 'float',
      region: null,
      name: '{id}',
      crs: null,
      dateFormat: 'yyyy-MM-dd',
      async: false
    }
    var opt = tools.get_options(defaults, options)
    var colList = collection.toList(collection.size());
    
    var wrap = function(n, img) {
      var geom = opt.region || img.geometry()
      var region = getRegion(geom)
      var imtype = IMAGE_TYPES(img, opt.type)
      var description = helpers.string.formatTask(n)
      var params = {
        image: imtype,
        description: description,
        folder: folder,
        fileNamePrefix: n,
        region: region,
        scale: opt.scale,
        maxPixels: opt.maxPixels
      }
      if (opt.crs) {
        params.crs = opt.crs
      }
      Export.image.toDrive(params)
    }
    
    var i = 0
    while (i >= 0) {
      try {
        var img = ee.Image(colList.get(i));
        var n = tools.image.makeName(img, opt.name, opt.dateFormat).getInfo()
        wrap(n, img)
        i++
      } catch (err) {
        var msg = err.message
        if (msg.slice(0, 36) === 'List.get: List index must be between') {
          break
        } else {
          print(msg)
          break
        }
      }
    }
  }
3.设置导出函数
主要是设置影像分辨率以及导出区域。

//导出影像
batch.Download.ImageCollection.toDrive( roi_collection, 'Landsat_image', {
  scale: 30,
  region: ROI
})
4.下载
依次点击Run(也可以用CSDN有人分享的一次性点击脚本)，直接下载影像。


图片

需要注意，下载的影像是覆盖ROI区域的所有原始影像,并没有按研究区进行裁剪。

参考资料：https://mp.weixin.qq.com/s/PeBT2AY5hAaPlxKEREd-UQ