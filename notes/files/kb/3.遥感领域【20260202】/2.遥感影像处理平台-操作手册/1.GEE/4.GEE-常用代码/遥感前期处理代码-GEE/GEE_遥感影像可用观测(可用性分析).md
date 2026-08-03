GEE | 遥感影像可用观测（可用性分析）

# 前言

随着遥感技术的发展，各类卫星影像已成为城市规划、生态监测、灾害评估等领域的重要数据源。对于遥感从业人员或研究者来说，明确特定地区在指定时间范围内的卫星数据可用性至关重要。然而，由于不同卫星的重访周期、轨道特性以及天气因素的影响，实际数据的获取情况会存在明显差异。因此为直观展示研究区域的常用的Sentinel-1、Sentinel-2、Landsat-8、Landsat-9卫星影像的可用性，本文分享通过GEE平台进行卫星影像可用性的可视化分析方法，清晰呈现不同卫星在研究区的影像覆盖情况。

# 1 实现方法

不同卫星的重访周期决定了它们在特定区域的数据可用性。其中，Sentinel-1的重访周期较短，约为6天；Sentinel-2的重访周期约为5天；而Landsat系列（Landsat-8和Landsat-9）卫星的重访周期较长，单颗卫星为16天，两颗卫星交错运行能实现8天左右的观测频率。本文所用方法的是基于卫星影像的元数据获取日期信息，并结合目标区域（AOI），分析一定时间窗口内不同卫星影像的空间覆盖情况和数据有效获取次数。具体而言，利用GEE平台对每个影像集合（ImageCollection）进行时空过滤，并提取每个日期的有效影像数量，通过统计分析和可视化手段呈现。


# 2 实现代码

```js


// 指定AOI和日期
var aoi = ee.FeatureCollection("users/geestudy2/Beijing");
var startDate = '2022-01-01';
var endDate = '2022-01-31';

// 卫星影像集合定义及颜色
var satellites = [  
    {name:'Sentinel-1', col: ee.ImageCollection("COPERNICUS/S1_GRD"), color: 'red'},  
    {name:'Sentinel-2', col: ee.ImageCollection("COPERNICUS/S2"), color: 'yellow'},  
    {name:'Landsat-8', col: ee.ImageCollection("LANDSAT/LC08/C02/T1_L2"), color: 'blue'},  
    {name:'Landsat-9', col: ee.ImageCollection("LANDSAT/LC09/C02/T1_L2"), color: 'green'}];

// 提取日期
function getDates(col){  
    return ee.List(col.reduceColumns(ee.Reducer.toList(), ["system:time_start"]).values().get(0))    
    .map(function(n){return ee.Date(n).format("YYYY-MM-dd")});
}

// 获取所有日期
 var all_dates = ee.List([]);
 satellites.forEach(function(sat){  
    sat.col = sat.col.filterDate(startDate, endDate).filterBounds(aoi);  
    sat.dates = getDates(sat.col);  all_dates = all_dates.cat(sat.dates);
 
// 绘制影像框（无填充，仅边界）  
 var bounds = sat.col.geometry().bounds();  
 var boundsOutline = ee.Image().byte().paint({    
 featureCollection: bounds,    
 color: 1,    width: 2  
 });  Map.addLayer(boundsOutline, {palette: sat.color}, sat.name + ' bounds');});
all_dates = all_dates.distinct().sort();
// 日期统计
var merged_collection = ee.FeatureCollection(all_dates.map(function(date){  
    date = ee.String(date);  
    var props = {date: date};  
    satellites.forEach(function(sat){    
        props[sat.name] = sat.dates.filter(ee.Filter.eq('item', date)).size();  
    });  
    return ee.Feature(null, props);
}));
// 柱状图
var columnChart = ui.Chart.feature.byFeature({    
    features: merged_collection,    
    xProperty: 'date',    
    yProperties: satellites.map(function(sat){return sat.name;})  
    })  
    .setChartType('ColumnChart')  
    .setOptions({    
        width: 600,    
        height: 400,    
        title: '卫星影像可用性（柱状图)',    
        hAxis: {title: '日期'},    
        vAxis: {title: '影像数量'},    
        colors: ['red', 'yellow', 'blue', 'green'],    
        isStacked: 'absolute',    
        legend: {position: 'top'}  
    });
// 折线图
var lineChart = ui.Chart.feature.byFeature({    
    features: merged_collection,    
    xProperty: 'date',    
    yProperties: satellites.map(function(sat){return sat.name;})  
    })  
    .setChartType('LineChart')  
    .setOptions({    
        width: 600,    
        height: 400,    
        title: '卫星影像可用性（折线图）',    
        hAxis: {title: '日期'},    
        vAxis: {title: '影像数量'},    
        colors: ['red', 'yellow', 'blue', 'green'],    
        legend: {position: 'top'},    
        pointSize: 5 
        });
print(columnChart);
print(lineChart);
// 在地图上显示AOI
Map.centerObject(aoi, 12);
Map.addLayer(aoi, {color: 'black'}, 'AOI');
```