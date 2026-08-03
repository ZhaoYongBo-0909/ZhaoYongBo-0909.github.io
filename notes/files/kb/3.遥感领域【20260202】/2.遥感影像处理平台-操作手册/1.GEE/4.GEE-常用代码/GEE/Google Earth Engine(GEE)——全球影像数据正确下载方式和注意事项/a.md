Google earth engine——初学者容易犯错的地方
原创 此星光明 生态云计算 2021-10-15 17:19


一下要介绍的主要是一些代码的练习，以此来最大的话让你能成果完成更为复杂的GEE计算，说白了就是让你避免入坑环节的一些案例介绍。

图片

避免将客户端函数和对象与服务器函数和对象混合



Earth Engine 服务器对象是具有以ee （例如ee.Image，ee.Reducer）开头的构造函数的对象，并且此类对象上的任何方法都是服务器功能。任何不是以这种方式构造的对象都是客户端对象。客户端对象可能来自代码编辑器（例如Map、Chart）或 JavaScript 语言（例如Date、Math、[]、 {}）。



为避免意外行为，请勿在脚本中混合使用客户端和服务器功能，如此处、 此处和此处讨论的那样。有关 地球引擎中客户端与服务器的深入解释，请参阅此页面和/或本教程。以下示例说明了混合客户端和服务器功能的危险：

var table = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017');
 
// Won't work.
for(var i=0; i<table.size(); i++) {
  print('No!');
}
C你能发现错误吗？请注意，这table.size()是服务器对象上的服务器方法，不能与客户端功能（如<条件）一起使用。



您可能希望使用 for 循环的一种情况是 UI 设置，因为代码编辑器ui对象和方法是客户端。(Learn more about creating user interfaces in Earth Engine). For example:



Good — Use client functions for UI setup.

var panel = ui.Panel();
for(var i=1; i<8; i++) {
  panel.widgets().set(i, ui.Button('button ' + i))
}
print(panel);
如果以上两端代码没看懂的话，不重要，告诉你，最好别用FOR循环，谢谢！

Conversely, map() is a server function and client functionality won't work inside the function passed to map(). For example:



Error — This code doesn't work!ss

var table = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017');
 
// Error:
var foobar = table.map(function(f) {
  print(f); // Can't use a client function here.这里一般只要涉及function 都会涉及到return否则就会报错
  // Can't Export, either.
});
相反，map()是一个服务器功能，客户端功能在传递给map().例如：

Good — Use map() set().

var table = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017');
//仅仅加载第一景影像
print(table.first());
 
// Do something to every element of a collection.用了一个map来遍历函数F
var withMoreProperties = table.map(function(f) {
  // Set a property.加入 一个属性，括号内是属性名称和属性面积
  return f.set('area_sq_meters', f.area())
});
print(withMoreProperties.first());
您还可以filter()基于计算或现有属性和print()结果的集合。请注意，您无法打印包含超过 5000 个元素的集合。如果您收到“累积超过 5000 个元素后集合查询中止”错误，filter()或limit()打印前集合。这种情况比较多，特别是在研究大区域的影像集合，你要一下输出的话，那么比较卡顿，而且同时会提示你超过5000个无法显示，所以，你可以通过筛选和限制多少个数来查看。