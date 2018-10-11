<template>
  <div class="container" @click="clickHandle('test click', $event)">
    <map class="map" id="map" :longitude="longitude" :latitude="latitude" scale="14"  show-location="true" :markers="markers" @markertap="makertap($event)" subkey="EX5BZ-QDRWU-LHZV7-2PSQA-LUSMH-GABYI" style="width: 100%; height: 50vh"></map>
    <view class="place_info">
      <text>{{placeData.title}}</text>
      <text>{{placeData.address}}</text>
      <text>{{placeData.telephone}}</text>
    </view>
  </div>
</template>

<script>
import card from '@/components/card'
import bmap from '@/utils/baidu/bmap-wx.js';
export default {
  data () {
    return {
      motto: 'Hello World',
      userInfo: {},
      markers: [],
      latitude: '',
      longitude: '',
      placeData: {}
    }
  },

  components: {
    card
  },

  methods: {
    makertap (event){
      var that = this;
      var id = event.mp.markerId;
      that.showSearchInfo(that.wxMarkerData, id);
      that.changeMarkerColor(that.wxMarkerData, id);
    },
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    },
    baiduFun () {
      var that = this;
      console.log('333')
      var BMap = new bmap.BMapWX({
        ak: 'ZM9tyL0QwFKDzHUf6vucbOk587jnSNu5'
      });
      var fail = function(data) {
        console.log(data)
      };
      var success = function(data) {
        that.wxMarkerData = data.wxMarkerData;
        that.markers = data.wxMarkerData;
        that.latitude = data.wxMarkerData[0].latitude;
        that.longitude = data.wxMarkerData[0].longitude;
      }
      BMap.search({
        "query": '美食',
        fail: fail,
        success: success
      });
    },
    showSearchInfo: function(data, i) {
      var that = this;
      console.log(data,  i)
      that.placeData = {
        title: '名称：' + data[i].title + '\n',
        address: '地址：' + data[i].address + '\n',
        telephone: '电话：' + data[i].telephone
      };
    },
    changeMarkerColor: function(data, id) {
      var that = this;
      var markersTemp = {};
      for (var i = 0; i < data.length; i++) {
        if (i === id) {
          data[i].iconPath = "../../../assets/img/marker_yellow.png";
        } else {
          data[i].iconPath = "../../../assets/img/marker_red.png";
        }
        markersTemp[i] = data[i];
      }
      that.markers = markersTemp
    }
  },

  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo();
    this.baiduFun();
  },
  mounted () {

  }
}
</script>

<style scoped>

</style>
