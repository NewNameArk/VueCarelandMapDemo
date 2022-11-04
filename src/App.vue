<template>
  <div id="app">
    <div id="map"></div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  mounted() {
    //凯立德地图API功能
    var point = new Careland.Point(419364916, 143908009); //创建坐标点(天安门)
    var map = new Careland.Map("map", point, 15); //实例化地图对象
    map.enableAutoResize(); //启用自动适应容器尺寸变化
    map.load(); //加载地图

    var layer = new Careland.Layer("point", "layer"); //创建点图层
    //创建样式
    var style = new Careland.PointStyle({
      offsetX: -11,
      offsetY: -30,
      textOffsetX: -5,
      textOffsetY: -30,
      src: require("./assets/poiMarker.png"),
      fontColor: "#000"
    });
    layer.setStyle(style); //设置点图层的表达样式
    map.addLayer(layer); //将图层添加到地图上

    var mapInfoWin = new Careland.InfoWindow();
    mapInfoWin.setOffset(new Careland.Size(0, -22));

    map.addEventListener("click", function (point) {
      //为点击地图添加事件，point为点击点坐标
      layer.clear();
      //创建文本标注点
      var marker = new Careland.Marker("image");
      marker.setPoint(point); //设置标注点位置
      layer.add(marker); //将标注点添加到图层上
      mapInfoWin.setContent("当前点坐标为：" + point.x + "," + point.y);
      mapInfoWin.redraw();
      marker.openInfoWindow(mapInfoWin);
    });
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 500px;
  width: 500px;
}
#map{width:100%;height:100%;overflow:hidden;margin:0;padding:0px;}
</style>
