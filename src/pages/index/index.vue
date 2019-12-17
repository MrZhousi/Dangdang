<template>
    <div class="wrap">
      <header>
        <div class="logo">
          <img src="/static/images/dangdang.png" class="logoImg" />
          <div class="searchbox"><input class="search" type="text" placeholder="Lily超品日，三折封顶"><img class="logoImg serchicon" src="/static/images/search.png" alt=""></div>
          <img src="/static/images/菜单管理.png" class="logoImg">
        </div>
      </header>
      <div>
        <swiper indicator-dots="flag" autoplay="flag" circular="flag">
          <swiper-item v-for="(item,index) in imgList"  wx:key="index">
            <img :src="item" class="swiperImg"/>
          </swiper-item>
        </swiper>
      </div>
      <div class="item-list">
        <img class="itemImg" v-for="(item,index) in typeList" :src="item.pic" alt="" :key="index" mode="widthFix">
      </div>
      <div class="br"></div>
      <img class="banner" src="/static/images/banner.jpg" alt="">

      <div class="bookList">
        <div class="bookItem" v-for="(item,index) in bookList" :key="index" @click="goCart(item._id)">
          <div class="imgbox"><img :src="item.pic" class="bookImg" mode="aspectFill"></div>
          <p class="title">{{item.title}}:{{item.description}}</p>
          <div class="little"><span class="ziying">当当自营</span><span class="manjian">满减</span></div>
          <p class="price">￥{{item.price}}</p>
        </div>
      </div>
    </div>
</template>
<script>
export default {
  data(){
    return{
      imgList:[
        "/static/images/1.jpg",
        "/static/images/2.jpg",
        "/static/images/3.jpg",
        "/static/images/4.jpg",
        "/static/images/5.jpg"
      ],
      typeList:[
        "/static/images/1.png",
        "/static/images/2.png",
        "/static/images/3.png",
        "/static/images/4.png",
        "/static/images/5.png",
        "/static/images/6.jpg",
        "/static/images/7.png",
        "/static/images/8.png",
        "/static/images/9.png",
        "/static/images/10.jpg"
      ],
      bookList:[],
      flag:true
    }
  },
   
  methods:{
    goCart(id){
      console.log(111)
      wx.navigateTo({
        url:`/pages/detail/main?id=${id}`,
        success:res=>{
          console.log(res)
        }
      })
    }
  },
  mounted() {
    wx.request({
      url: 'http://localhost:3000/bookList',
      success: (res)=>{
        console.log(res)
        this.bookList = res.data
      },
      fail: (err)=>{
        console.log(err)
      }
    })
    wx.request({
      url: 'http://localhost:3000/typeList',
      success: (res)=>{
        console.log(res)
        this.typeList = res.data
      },
      fail: (err)=>{
        console.log(err)
      }
    })
  },
  created(){
   
  }
}
</script>
<style scoped>
.imgbox{
  width: 360rpx;
  height: 360rpx;
}
.wrap{
  box-sizing: border-box;
}
.logo{
  margin: 10rpx 0;
  width: 750rpx;
  height: 60rpx;
  display: flex;
  justify-content: space-around;
  /* padding: 0 20rpx; */
}
.logoImg{
  width: 60rpx;
  height: 60rpx
}
.searchbox{
  position: relative;
}
.search{
  width: 520rpx;
  height: 60rpx;
  border-radius: 30rpx;
  background-color: #e8ebf0;
  padding-left:30px;
}
.serchicon{
  position: absolute;
  top: 0;
  left: 0;
}
swiper{
  height: 228rpx;
}
.swiperImg{
  width: 750rpx;
  height: 228rpx;
}

.itemImg{
  width: 20%;
}
.br{
  width: 750rpx;
  height: 20rpx;
  background-color:#f5f5f3;
} 
.banner{
  width: 750rpx;
  height: 200rpx
}
.bookList{
  width: 750rpx;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  background-color: #eef3f9;
}
.bookItem{
  padding-left:10rpx; 
  width: 48%;
  height: 530rpx;
  font-size: 26rpx;
  background-color: #fff;
  margin-top: 10rpx
}
.bookImg{
  width: 100%;
  height: 100%
}
.little{
  margin: 10rpx 0 ;
}
.title{
  height: 68rpx;
  display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
	overflow: hidden;
}
.ziying{
  border: 1rpx solid #f12f38;
  color: #f12f38;
  border-radius: 3rpx;
  margin-right:10rpx;
}
.manjian{
  background: #f12f38;
  color: #ffffff;
  border-radius: 3rpx;
}
.price{
  color: #f12f38;
}
</style>