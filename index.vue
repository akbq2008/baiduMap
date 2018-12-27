<template>
  <div>
    <div
      class="banner ct"
      @click="playVideo"
    >
      <video
        src="/static/img/bases.mp4"
        controls="controls"
        id="video"
        poster="/static/img/ic_address copy 5.png"
      ></video>
    </div>
    <div class="map ct">
      <h2 class="fw ft_48">千红蜜蜂基地</h2>
      <p class="fw ft_24 title_info">健康生活 甜蜜家园</p>
      <div
        class="map_container"
        id="container"
      ></div>
    </div>
    <div class="address_info">
      <div class="tabbar ct">
        <div class="tab_content ct">
          <div
            class="content wt"
            v-for="(item,index) in addressData"
            :key="index"
            @click="turnTo(item)"
          >
            <div class="product_img">
              <img
                src="/static/img/product.png"
                alt=""
              >
            </div>
            <div class="product_content">
              <div class="fx_sp">
                <span class="ft_20 addressName">{{item.name}}</span>
              </div>
              <div class="cl grey info fx_sp">
                <div class="left"> {{item.time}}</div>
                <div class="right1"><img
                    src="/static/img/location.png"
                    alt=""
                  >{{item.address}}</div>

              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      start: "",
      mapA: "",
      __this: "",
      opts: {},
      addressInfo: [], //地址信息
      searchResult: [], //搜索后的实例
      content: [], //windowInfo信息html内容
      markerInstance: [], //maker实例
      addressData: [
        {
          name: "福建武夷山养蜂基地",
          address: "福建",
          time: "成立于2012年4月"
        },
        {
          name: "山西省安泽县有机蜜源基地",
          address: "山西",
          time: "成立于2012年4月"
        },
        {
          name: "黑龙江漠河蜂业基地",
          address: "黑龙江",
          time: "成立于2012年4月"
        },
        {
          name: "吉林安图蜂业基地",
          address: "吉林",
          time: "成立于2012年4月"
        },
        {
          name: "内蒙古呼伦贝尔蜂业基地",
          address: "内蒙古",
          time: "成立于2012年4月"
        },
        {
          name: "青海门源蜂业基地",
          address: "青海",
          time: "成立于2012年4月"
        },
        {
          name: "山西沁水蜂业基地",
          address: "山西",
          time: "成立于2012年4月"
        },
        {
          name: "江山江郎山土蜂基地",
          address: "江西",
          time: "成立于2012年4月"
        },
        {
          name: "陕西延安洋槐基地",
          address: "陕西",
          time: "成立于2012年4月"
        }
      ]
    };
  },
  mounted() {
    this.__this = this;
    var map = new BMap.Map("container");
    this.mapA = map;
    // 创建地图实例
    var point = new BMap.Point(118.014211, 27.75102);
    // 创建点坐标
    // map.enableScrollWheelZoom(true); //允许鼠标滚轮
    map.centerAndZoom("陕西", 6);
    map.addControl(new BMap.NavigationControl()); //控件缩放
    map.addControl(new BMap.ScaleControl()); //缩放控制
    map.addControl(new BMap.OverviewMapControl()); //缩略图
    map.addControl(new BMap.MapTypeControl()); //地图类型
    map.enableDragging(); //允许拖拽
    // 百度地图API功能
    this.init(map);
  },

  methods: {
    init(map) {
      var data_info = [
        [
          118.014211,
          27.75102,
          "福建武夷山基地.jpg",
          "福建武夷山养蜂基地",
          "福建武夷山"
        ],
        [
          112.243965,
          36.189409,
          "山西省安泽县有机蜜源基地.jpg",
          "山西省安泽县有机蜜源基地",
          "山西省安泽县"
        ],
        [
          122.513438,
          52.994953,
          "黑龙江漠河蜂业基地.jpg",
          "黑龙江漠河蜂业基地",
          "黑龙江漠河"
        ],
        [
          128.909472,
          43.125919,
          "吉林安图蜂业基地.jpg",
          "吉林安图蜂业基地",
          "吉林安图"
        ],
        [
          119.837907,
          49.263995,
          "内蒙古呼伦贝尔蜂业基地.jpg",
          "内蒙古呼伦贝尔蜂业基地",
          "内蒙古呼伦贝尔"
        ],
        [
          101.532426,
          37.433088,
          "青海门源蜂业基地.jpg",
          "青海门源蜂业基地",
          "青海门源"
        ],
        [
          112.496407,
          35.500078,
          "山西沁水蜂业基地.jpg",
          "山西沁水蜂业基地",
          "山西沁水"
        ],
        [
          118.651885,
          28.785517,
          "江山江郎山土蜂基地.jpg",
          "江山江郎山土蜂基地",
          "江山江郎山"
        ],
        [
          109.489105,
          36.561434,
          "陕西延安洋槐基地.jpg",
          "陕西延安洋槐基地",
          "陕西延安"
        ]
      ];
      this.addressInfo = data_info;
      //@params  精度  维度  图片地址  标题  副标题
      var myIcon = new BMap.Icon(
        "/static/img/location.png",
        new BMap.Size(48, 63)
      ); //标注的图标
      this.opts = {
        width: 370, // 信息窗口宽度
        height: 377, // 信息窗口高度
        enableAutoPan: true
      };
      for (var i = 0; i < data_info.length; i++) {
        var pt = new BMap.Point(data_info[i][0]);
        var marker = new BMap.Marker(
          new BMap.Point(data_info[i][0], data_info[i][1]),
          { icon: myIcon }
        ); // 创建标注
        this.markerInstance[i] = marker;
        this.content[i] =
          "<img src='/static/img/" +
          data_info[i][2] +
          "' style='width:400px;max-height:290px;' ><p class='ft_20'>" +
          data_info[i][3] +
          "</p><p class='grey'>" +
          data_info[i][4] +
          "</p> <div class='cr'>起点：<input  class='start' style='height:33px'/> <button class='mapBtn cf ' >前往</button></div>";
        map.addOverlay(marker); // 将标注添加到地图中
        this.addClickHandler(
          this.content[i],
          this.markerInstance[i],
          this,
          data_info[i][4],
          map,
          data_info[i][0],
          data_info[i][1]
        );
      }
    },
    /**
     * @params  conten:内容,marker:实例 ,_vue:vue实例,map:地图实例,lang:经度,lt:维度
     */
    addClickHandler(content, marker, _vue, address, map, lang, lt) {
      marker.addEventListener("click", function(e) {
        var icon = new BMap.Icon(
          "/static/img/ic_address copy 18.png",
          new BMap.Size(48, 63)
        );
        this.setIcon(icon);
        _vue.openInfo(content, this, _vue, address, lang, lt);
      });
    },
    /**
     * @params  conten:内容,e:event对象,_this:infoWindow实例 ,_vue:vue实例,,lang:经度,lt:维度
     */
    openInfo(content, _this, _vue, end, lang, lt) {
      // 传this,是因为2个函数内部的this指向不同
      var point = new BMap.Point(lang, lt); //窗口所在点
      var infoWindow = new BMap.InfoWindow(content, _vue.opts); // 创建信息窗口对象
      _vue.mapA.openInfoWindow(infoWindow, point); //开启信息窗口
      _vue.$nextTick(() => {
        setTimeout(() => {
          let mapBtn = document.querySelector(".mapBtn");
          if (mapBtn) {
            mapBtn.addEventListener("click", function() {
              let start = document.querySelector(".start").value;
              var driving = new BMap.DrivingRoute(_vue.mapA, {
                renderOptions: {
                  map: _vue.mapA,
                  panel: "allmap-result",
                  autoViewport: true
                },
                policy: "BMAP_DRIVING_POLICY_LEAST_DISTANCE"
                // 路线设置策略，最短距离
              });
              _vue.searchResult.push(driving);
              let startPoint = "";
              let endPoint = "";
              var myGeo = new BMap.Geocoder();
              myGeo.getPoint(start, point => {
                if (point) {
                  startPoint = new BMap.Point(point.lng, point.lat); //起始点坐标
                } else {
                  _vue.$message.error("请输入正确的位置");
                }
                endPoint = new BMap.Point(lang, lt); //结束点位置坐标
                _vue.searchResult.forEach(v => {
                  if (v.aa != driving.aa) {
                    v.clearResults(); //清除上一次的路线,实例不一样
                  }
                });
                driving.search(startPoint, endPoint);
              });
            });
          }
        }, 20);
      });
      infoWindow.addEventListener("close", function(e) {
        //监听信息框的关闭事件
        if (!infoWindow.isOpen()) {
          var icon = new BMap.Icon(
            "/static/img/location.png",
            new BMap.Size(48, 63)
          );
          _this.setIcon(icon);
        }
      });
    },
    turnTo(item, e) {
      this.addressInfo.forEach((v, i) => {
        if (v[3] === item.name) {
          this.addClickHandler(
            this.content[i],
            this.markerInstance[i],
            this,
            this.addressInfo[i][4],
            this.mapA,
            this.addressInfo[i][0],
            this.addressInfo[i][1]
          );
          // 开启窗口
          this.openInfo(
            this.content[i],
            this.markerInstance[i],
            this,
            this.addressInfo[i][4],
            this.addressInfo[i][0],
            this.addressInfo[i][1]
          );
        }
      });
    },
    playVideo() {
      let video = document.getElementById("video");
      video.paused ? video.play() : video.pause();
    }
  }
};
</script>

<style scoped lang="scss">
.banner {
  cursor: pointer;
  background-size: cover;
  height: 960px;
}
#video {
  width: 100%;
  height: 960px;
}
.map {
  padding-top: 120px;
  .title_info {
    margin: 16px 0 64px 0;
  }
  .map_container {
    height: 868px;
  }
}
.address_info {
  padding: 40px 0 120px 0;
}
.tabbar {
  margin-top: 40px;
  .tab_selected {
    border-bottom: 4px solid #f44336;
  }
  .tab_content {
    max-width: 1440px;
    margin: 0 auto;
    text-align: left;
    .content:hover {
      box-shadow: 0px 34px 23px 0px rgba(0, 0, 0, 0.1);
    }
    .content {
      cursor: pointer;
      box-shadow: 0px 8px 24px 0px rgba(0, 0, 0, 0.1);
      display: inline-block;
      width: 320px;
      margin: 20px;
      .product_img {
        height: 180px;
        img {
          width: 100%;
        }
      }
      .product_content {
        padding: 24px;
        .info {
          margin: 8px 0;
        }
        .shadow {
          margin-left: -24px;
          width: 320px;
          height: 40px;
          background-image: url("/static/img/box-shadow.png");
        }
        img {
          width: 12px;
          height: 16px;
          margin-right: 4px;
        }
        img,
        span {
          vertical-align: middle;
        }
      }
      .btn {
        width: 120px;
        height: 40px;
        margin-bottom: 24px;
        box-sizing: border-box;
      }
    }
  }
}
.addressName {
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  max-width: 272px;
}
</style>