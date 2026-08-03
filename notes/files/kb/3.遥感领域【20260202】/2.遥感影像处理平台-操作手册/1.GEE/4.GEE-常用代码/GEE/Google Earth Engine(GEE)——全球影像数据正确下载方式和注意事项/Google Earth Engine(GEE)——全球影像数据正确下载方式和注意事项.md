Google Earth Engine(GEE)——全球影像数据正确下载方式和注意事项
原创 此星光明 生态云计算 2022-02-09 17:58

今天看到有人担心全球影像能不能下载的问题？

有时候我们会用到全球的数据，但是如何下载全球的数据呢？

首先如果没有全球的矢量边界我们会不会无法下载全球的数据，答案当然是可以的，因为我们在边界筛选的过程中filterBounds(),就是为了筛选边界，当我们不进行边界筛选的时候，自然而然默认的边界范围就是全球的，但是紧接着会面临其他两个问题：1如何下载？2如何在不缩小分辨率的情况下下载成功？

对于第一个问题我们如何下载？还是按照正常的逻辑利用export直接倒出到Google硬盘当中，

代码：

    //这里进行影像的加载然后不用边界筛选，直接将影像合成后下载
  var scol= ee.ImageCollection("MODIS/006/MOD17A2H")
          //.filterBounds(hh)
          .filterDate(start,stop)
          .select("Gpp");print("scol",scol)
  var ndvi_before =scol.qualityMosaic('Gpp');//.clip(hh);

  Export.image.toDrive({
image: ndvi_before.select("Gpp"),
    //region:hh,
    scale:10000,
    description: "world_GPP_MOD17A2H_006_10000m_8day",
    folder: 'MOD17A2H_006_MOD13Q1',
  })
结果：我已经成功将数据下载下来了，这里为了实现全球的快速下载，分辨率直接使用了10000米，所以很快就下载下来，首先我们下载全球的只要代码没问题，那么下载都是正常的。



图片



添加图片注释，不超过 140 字（可选）
当然在下载分辨率更为精细的500m甚至30M的数据的时候，会出现第二个问题，数据超限的问题，所以大家要注意，之前写过关于超限的两篇文章，这里给大家提供链接即可：

Google Earth Engine——gee文件导出到drive：影像等大文件导出100000000超限解决办法_此星光明的博客-CSDN博客



地球引擎保姆级教程——高程影像数据的面积的计算和scale超限问题的解决_此星光明的博客-CSDN博客

此外，我们还要注意的是在下载一个区域的影像的时候，有时候数据过大，会出现每一年活着每一期的影像自动分区下载，所以下载的文件不是一个而是很多歌，这点需要大家注意。

此外，如果你要下载30M分辨率的数据，Google drive 的免费使用额度是15G，所以很少情况下不会超限，所以这个问题需要你缩小分辨率活着一个洲一个洲下载，这样可以不用购买会员。

获取更多资源：

此星光明的博客_CSDN博客-GEE数据集专栏,Google Earth Engine,GEE教程训练领域博主
参考资料：https://mp.weixin.qq.com/s/St6poQMiHyqjSFmhz6tCjA