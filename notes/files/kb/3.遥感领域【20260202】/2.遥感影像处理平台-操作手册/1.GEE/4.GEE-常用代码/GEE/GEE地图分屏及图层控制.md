

在之前的推送中，我讲了<GEE成像方式汇总>和<GEE图例添加>，今天我们来讲另一个JavaScript版GEE的实用的组件：地图分屏与自定义图层操控。

    这个功能非常直观地呈现算法前后地图的对比，其运行效果如下：


图片

自定义图层操控组件图片

地图分屏拖动



完整版代码 GEE Link：

https://code.earthengine.google.com/87e411c1f6c4e152e8524ca3f2a9878c



下面我们来进行代码讲解，本次我们要实现的功能有两个：

（1）地图分屏，将地图分为左图和右图，且左右图联动操作

（2）自定义图层操控组件，使用悬浮的panel来控制图层








地图分屏基础设置


其核心为设置地图分屏组件SplitPanel，然后设置地图联动组件ui.Map.Linker。讲解见注释。



你可以选择分屏模式为'horizontal'水平或'vertical'垂直分屏，也可以选择是否具有“擦除过渡”效果。


图片水平放置，擦除。orientation: 'horizontal'，wipe: true

图片水平放置，不擦除。orientation: 'horizontal'，wipe: false

图片垂直放置，擦除。orientation: 'vertical''，wipe: true

代码：

var leftMap = ui.Map();
var rightMap = ui.Map();

// Configure our map with a minimal set of controls.
leftMap.setControlVisibility(true);
leftMap.style().set({cursor: 'crosshair'});// 设置鼠标形式
leftMap.setOptions('TERRAIN');// 【地形图+地名】
leftMap.centerObject(ee.Geometry.Point([0,30]),3);

rightMap.setControlVisibility(true);
rightMap.style().set({cursor: 'crosshair'});
rightMap.setOptions('TERRAIN');//【默认的卫星影像，但无地名】

// 设置地图分屏组件 Split map:
var splitPanel = ui.SplitPanel({
  firstPanel: leftMap,// 第一个地图
  secondPanel: rightMap, /// 第二个地图
  orientation: 'horizontal',//'vertical',水平或垂直放置
  wipe: true ,//是否设置“擦除过渡”效果, 如果设置为false，则为地理上不连接的2个地图
  style: {stretch: 'both'}// CSS styles 
});

// 设置两个地图的联动。Link the two maps:
var linker = ui.Map.Linker([leftMap, rightMap]);
ui.root.widgets().reset([splitPanel]);





添加地图Panel控件



   我们选择地图模式为“水平放置，擦除”，这种设置有利于图层间的对比。设置地图后，会发现左边的地图的图层layer被遮挡，所以我们需要设置自定义图层操控组件，来操控各地图图层。

    注意set后面的那个数字，该数字是操控图层的句柄。

本次举例为：

左图：图层1：地表覆盖数据；图层2：MODIS地表反射率数据；图层3：大洲边界数据

右图：图层1：积雪覆盖度FSC数据；图层2：大洲边界数据；图层3：大洲边界数据

讲解见注释。
图片



代码：

// 例：左图：图层1：地表覆盖数据；图层2：MODIS地表反射率数据；图层3：大洲边界数据
leftMap.layers().set(0, ui.Map.Layer(landcover,{bands: ['Map']}, 'ESA WorldCover',false));//landcover
leftMap.layers().set(1, ui.Map.Layer(_MOD09GA, visualization_MOD, 'MODIS Surface Reflectance'));//MODIS Surface Reflectance
leftMap.layers().set(2, ui.Map.Layer(continent_roi, {palette: "red"}, 'Continent Boundary'));//continent boundary
  
// 例：右图：图层1：FSC数据；图层2：大洲边界数据；图层3：中国边界数据
rightMap.layers().set(0, ui.Map.Layer(_MOD_FSC, palette_FSC, 'Fractional Snow Cover'));//FSC
rightMap.layers().set(1, ui.Map.Layer(continent_roi, {palette: "red"}, 'Continent Boundary'));//Continent Boundary
rightMap.layers().set(2, ui.Map.Layer(china_roi, {palette: "yellow"}, 'China Boundary'));//China Boundary

// 构建图层操作控件
var layerPanel_left = ui.Panel();
layerPanel_left.style().set({
  width: '200px',
  position: 'bottom-left',
  backgroundColor: 'rgba(255, 255, 255, 0.8)'
});
// 插入标题
var layerboxTitle = ui.Label('Left Map');
  layerboxTitle.style().set('fontWeight', 'bold');
  layerboxTitle.style().set({fontSize:'1.5vw',padding:'0px',color:'red', textAlign:'left',backgroundColor:'rgba(255,255,255,0)'});
layerPanel_left.widgets().set(0,layerboxTitle);

leftMap.add(layerPanel_left);// 将该插件插入地图





设置checkbox复选框

    我们可以根据复选框的值显示或隐藏图层。


代码：

var checkbox0_left = ui.Checkbox('ESA WorldCover', false);//false为默认关闭
checkbox0_left.style().set({fontSize: '1vw',color:'blue',backgroundColor: 'rgba(255, 255, 255, 0)'});
checkbox0_left.onChange(function(checked) {
  // 根据复选框的值显示或隐藏图层。
  leftMap.layers().get(0).setShown(checked);
});

var checkbox1_left = ui.Checkbox('MODIS Surface Reflectance', true);//该复选框为默认勾选状态
checkbox1_left.style().set({fontSize: '1vw',color:'blue',backgroundColor: 'rgba(255, 255, 255, 0)'});
checkbox1_left.onChange(function(checked) {
  leftMap.layers().get(1).setShown(checked);
});

var checkbox2_left = ui.Checkbox('Continent Boundary', true);
checkbox2_left.style().set({fontSize: '1vw',color:'blue',backgroundColor: 'rgba(255, 255, 255, 0)'});
checkbox2_left.onChange(function(checked) {
  leftMap.layers().get(2).setShown(checked);
});

var subPanelStyle = {backgroundColor:'rgba(255,255,255,0)'};

layerPanel_left.widgets().set(1,ui.Panel({
  widgets:[checkbox0_left,checkbox1_left,checkbox2_left],style: subPanelStyle}));
 





结果图：



图片



图片



图片



# 完整代码
完整版代码 GEE Link：https://code.earthengine.google.com/87e411c1f6c4e152e8524ca3f2a9878c

```javascript
// 公众号：丢丢学GIS
// 20231025

// ================================= 数据准备 ================================= 
// GEE出图见我前期推送 <GEE成像方法汇总>

var continent_roi = ee.Image().float().paint(ee.FeatureCollection("projects/ee-snow-retrival-2/assets/ROI/Continent_Land"),0,1.5);//大陆边框
var china_roi = ee.Image().float().paint(ee.FeatureCollection("users/TP_Shp/shp/china_poly"),0,2);//中国国界边框

var landcover = ee.ImageCollection("ESA/WorldCover/v100").first();// ESA WorldCover landcover
// Map.addLayer(landcover,{bands: ['Map']}, 'ESA WorldCover v100',false)
var landcover_water_mask = landcover.eq(80).not()

//调用我自建的去云函数库，几乎包括全部的光学传感器卫星的去云方案
var cloudMask = require('users/wanghuahua/cryosphere_wang:我的函数库/1-0 去云');
var dateRange = ee.DateRange(ee.Date('2022-12-01'),ee.Date('2023-02-01'))
var _MOD09GA = ee.ImageCollection('MODIS/061/MOD09GA')
          .filterDate(dateRange)
          .map(cloudMask.MODISCloud)//去云
          .mean().unitScale(0,10000)//归一化
          
var _MOD_FSC = ee.ImageCollection('MODIS/061/MOD10A1').filterDate(dateRange).select('NDSI_Snow_Cover').mean() //积雪覆盖度

var colormap_re = require('users/wanghuahua/cryosphere_wang:basicTool/0-1 积雪覆盖度颜色图');
var colormap_FSC = colormap_re.defaultSnowColor100;//积雪覆盖度颜色调用
                  
// 影像真彩色出图设置
var visualization_MOD = {
  bands: ['sur_refl_b01', 'sur_refl_b04', 'sur_refl_b03'],//选定RGB的波段
  min: 0,
  max: 0.7,
}; 

// 积雪覆盖度FSC出图设置
var palette_FSC = {
  min: 5,
  max: 100,
  palette: colormap_FSC.palette
};

// Map.addLayer(_MOD_FSC, palette_FSC,'FSC');
// Map.centerObject(ee.Geometry.Point([0,30]),3);

// ================================= 左右地图基础设置 ========================================
var leftMap = ui.Map();
var rightMap = ui.Map();

// Configure our map with a minimal set of controls.
leftMap.setControlVisibility(true);
leftMap.style().set({cursor: 'crosshair'});// 设置鼠标形式
leftMap.setOptions('TERRAIN');// 【地形图+地名】
leftMap.centerObject(ee.Geometry.Point([0,30]),3);

rightMap.setControlVisibility(true);
rightMap.style().set({cursor: 'crosshair'});
rightMap.setOptions('TERRAIN');//【默认的卫星影像，但无地名】

// 设置地图分屏组件 Split map:
var splitPanel = ui.SplitPanel({
  firstPanel: leftMap,// 第一个地图
  secondPanel: rightMap, /// 第二个地图
  orientation: 'horizontal',//'vertical',水平或垂直放置
  wipe: true ,//是否设置“擦除过渡”效果, 如果设置为false，则为地理上不连接的2个地图
  style: {stretch: 'both'}// CSS styles 
});

// 设置两个地图的联动。Link the two maps:
var linker = ui.Map.Linker([leftMap, rightMap]);
ui.root.widgets().reset([splitPanel]);

// ======================================= 添加地图Panel控件 =========================================
// 设置地图后，会发现左边的地图的图层layer被遮挡，所以我们需要设置panel，在操控各地图图层
// 注意set后面的那个数字，该数字是操控图层的句柄
//  ui.Map.Layer的用法，和Map.addLayer是一样的

// 例：左图：图层1：地表覆盖数据； 图层2：MODIS地表反射率数据；图层3：大洲边界数据
leftMap.layers().set(0, ui.Map.Layer(landcover,{bands: ['Map']}, 'ESA WorldCover',false));//landcover
leftMap.layers().set(1, ui.Map.Layer(_MOD09GA, visualization_MOD, 'MODIS Surface Reflectance'));//MODIS Surface Reflectance
leftMap.layers().set(2, ui.Map.Layer(continent_roi, {palette: "red"}, 'Continent Boundary'));//continent boundary
  
// 例：右图：图层1：FSC数据； 图层2：大洲边界数据；图层3：中国边界数据
rightMap.layers().set(0, ui.Map.Layer(_MOD_FSC, palette_FSC, 'Fractional Snow Cover'));//FSC
rightMap.layers().set(1, ui.Map.Layer(continent_roi, {palette: "red"}, 'Continent Boundary'));//Continent Boundary
rightMap.layers().set(2, ui.Map.Layer(china_roi, {palette: "yellow"}, 'China Boundary'));//China Boundary

// 构建图层操作控件
var layerPanel_left = ui.Panel();
layerPanel_left.style().set({
  width: '200px',
  position: 'bottom-left',
  backgroundColor: 'rgba(255, 255, 255, 0.8)'
});
// 插入标题
var layerboxTitle = ui.Label('Left Map');
  layerboxTitle.style().set('fontWeight', 'bold');
  layerboxTitle.style().set({fontSize:'1.5vw',padding:'0px',color:'red', textAlign:'left',backgroundColor:'rgba(255,255,255,0)'});
layerPanel_left.widgets().set(0,layerboxTitle);

leftMap.add(layerPanel_left);// 将该插件插入地图

// ======================================= 设置checkbox选择框 =========================================
// 我们可以根据复选框的值显示或隐藏图层。
var checkbox0_left = ui.Checkbox('ESA WorldCover', false);//false为默认关闭
checkbox0_left.style().set({fontSize: '1vw',color:'blue',backgroundColor: 'rgba(255, 255, 255, 0)'});
checkbox0_left.onChange(function(checked) {
  // 根据复选框的值显示或隐藏图层。
  leftMap.layers().get(0).setShown(checked);
});

var checkbox1_left = ui.Checkbox('MODIS Surface Reflectance', true);//该复选框为默认勾选状态
checkbox1_left.style().set({fontSize: '1vw',color:'blue',backgroundColor: 'rgba(255, 255, 255, 0)'});
checkbox1_left.onChange(function(checked) {
  leftMap.layers().get(1).setShown(checked);
});

var checkbox2_left = ui.Checkbox('Continent Boundary', true);
checkbox2_left.style().set({fontSize: '1vw',color:'blue',backgroundColor: 'rgba(255, 255, 255, 0)'});
checkbox2_left.onChange(function(checked) {
  leftMap.layers().get(2).setShown(checked);
});

var subPanelStyle = {backgroundColor:'rgba(255,255,255,0)'};

layerPanel_left.widgets().set(1,ui.Panel({
  widgets:[checkbox0_left,checkbox1_left,checkbox2_left],style: subPanelStyle}));
  
// ==================== 右图图例 ==========================
// 详见上一期推送<如何在GEE上添加图例，或者。。对联？>
var legend_FSC = ui.Panel({
    style: {
        position: 'bottom-right',
        width: '25%',
        backgroundColor: 'rgba(255, 255, 255, 0.7)'//背景值，0.7为透明度
    }
});

// 生成图例
makeLegend(palette_FSC,'Fractional Snow Cover (%)',legend_FSC);

rightMap.add(legend_FSC);

// function of construct the legend widgets. 
function makeLegend(vis,desc,legend) {
  var desc_label = ui.Label({
                value: desc,
                style: {
                    fontSize: '14px',
                    textAlign: 'left',
                    padding: '0px 0px 4px 0px',
                    margin: '8px 0px',
                    fontWeight: 'bold',
                    backgroundColor: 'rgba(255, 255, 255, 0)'
                },})
  
  // Create the color bar for the legend.
  var colorBar = ui.Thumbnail({
        // Image to use for color bar.
            image: ee.Image.pixelLonLat().select(0),
            // Parameters for color bar.
            params: makeColorBarParams(vis.palette),
            style: {
                // Stretch color bar horizontally.
                stretch: 'horizontal',
                // No margin for color bar.
                margin: '0px 0px',
                // Max height of color bar.
                maxHeight: '10%',
                // Width of color bar.
                width: '100%',
                backgroundColor: 'rgba(255, 255, 255, 0)'
            },
        });

  // Create a panel with three numbers for the legend.
  var legendLabels = ui.Panel({
            widgets: [
                ui.Label(vis.min, {
                    margin: '0px 0px',
                    backgroundColor: 'rgba(255, 255, 255, 0)'
                }),
                ui.Label((vis.min + vis.max)/2, {
                    margin: '0px 0px',
                    textAlign: 'center',
                    stretch: 'horizontal',
                    backgroundColor: 'rgba(255, 255, 255, 0)'
                }),
                ui.Label(vis.max, {
                    margin: '0px 0px',
                    backgroundColor: 'rgba(255, 255, 255, 0)'
                }),
            ],
            layout: ui.Panel.Layout.flow('horizontal')
        });

  legendLabels.style().set({
    backgroundColor: 'rgba(255, 255, 255, 0)'
  })
  // Add label to legend.
  legend.add(desc_label);
          
  // Add colorbar to legend.
  legend.add(colorBar);

  // Add labels to legend.
  legend.add(legendLabels);
}

function makeColorBarParams(palette) {
            return { 
                bbox: [0, 0, 1, 0.1],
                // Dimensions of color bar.
                dimensions: '100x10',
                // Format of color bar.
                format: 'png',
                // Min value for color bar.
                min: 0,
                // Max value for color bar.
                max: 1,
                // Color palette for color bar.
                palette: palette
            };
        }
```






























# 参考资料
1.GEE地图分屏及图层控制（附完整代码）_丢丢学GIS：https://mp.weixin.qq.com/s/x_9YI3pyDVc0tFgikVoIFw