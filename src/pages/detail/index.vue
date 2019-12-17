<template>
  <div class="wrap">
    <div class="header">
      <img class="logoImg" src="/static/images/left.png" alt @click="back" />
      <p>详情</p>
      <img class="logoImg" src="/static/images/sandian.png" alt />
    </div>
    <div class="main">
      <img class="imgBox" :src="infoArr.pic" mode="widthFix" />
      <div class="info">
        <div class="zyname">
          <span class="ziying">当当自营</span>
          <span class="name">{{infoArr.title}}</span>
        </div>
        <p class="desc">{{infoArr.description}}</p>
        <div>
          <div class="jiage">
            <span class="price">￥{{infoArr.price}}</span>
            <span class="Discount">(9.48折)</span>
            <span class="reduce">
              <img class="jiangjia" src="/static/images/降价.png" />降价通知
            </span>
          </div>
          <div class="dingjia">
            定价
            <span class="old-price">￥{{infoArr.fee}}</span>
          </div>
        </div>
      </div>
      <div class="br"></div>
      <div class="bottom">
        <div class="left">
          <div class="iconbox"><img class="bottomIcon" src="/static/images/店铺.png" alt=""><p>店铺</p></div>
          <div class="iconbox"><img class="bottomIcon" src="/static/images/cart.png" alt=""><p>购物车</p></div>
        </div>
        <div class="right">
          <p class="addCart" @click="addCart(infoArr._id)">加入购物车</p>
          <p class="buy">立即购买</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      infoArr: {}
    };
  },

  methods: {
    addCart(id){
      let products = wx.getStorageSync('cart') || []
      let index = products.findIndex((item)=>{
        return item.id == id
      })
      if(index != -1){
        products[index].count+=1
      }else{
        this.infoArr.count=1
        this.infoArr.cheched=true
        products.push(this.infoArr)
      }
      wx.setStorageSync('cart',products)
      wx.reLaunch({
        url: '/pages/cart/main',
        success: (res)=>{
          
        }
      })
      // console.log(index)
    },
    back() {
      wx.navigateBack({
        delta: 1,
        success: res => {
          console.log(res);
        },
        fail: err => {
          console.log(err);
        }
      });
    }
  },
  onLoad(e) {
    console.log(e);
    this.id = e.id;
    wx.request({
      url: "http://localhost:3000/bookList",
      success: res => {
        // console.log(res)
        let infoArr = res.data.find(item => {
          return item._id == e.id;
        });
        this.infoArr = infoArr;
        console.log(infoArr);
      },
      fail: () => {
        // fail
      }
    });
  },
  
  created() {}
};
</script>
<style scoped>
.addCart,.buy{
  border-radius: 0;
  padding: 0;
  line-height: 100rpx;
  text-align: center;
  font-size: 26rpx;
}
.addCart{
  background: #f7c01f;
  color: #ffffff;
  width: 50%;
}
.buy{
  background: #f3554a;
  color: #ffffff;
  width: 50%;
}
.iconbox{
  width: 50%;
  text-align: center;
}
.bottomIcon{
  width: 40rpx;
  height: 40rpx;
}
.bottom{
  width: 750rpx;
  height: 100rpx;
  display: flex;
}
.left{
  width: 50%;
  font-size: 24rpx;
  display: flex;
  align-items: center;
}
.right{
  width: 50%;
  font-size: 24rpx;
  display: flex
}
.br{
  width: 750rpx;
  height: 20rpx;
  background-color:#f5f5f3;
}
.header {
  box-sizing: border-box;
  padding: 0 20rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1rpx solid #d0d0d0;
  line-height: 90rpx;
}
.logoImg {
  width: 38rpx;
  height: 38rpx;
}
.imgBox {
  width: 750rpx;
  /* height: 750rpx; */
}
.info {
  padding: 30rpx 24rpx;
  border-top: 1rpx solid #d0d0d0;
}
.zyname {
  margin-bottom: 20rpx;
}
.ziying {
  background-color: #f12f38;
  border-radius: 18rpx;
  padding: 0 10rpx;
  line-height: 22rpx;
  color: #ffffff;
  font-size: 22rpx;
}
.name {
  color: #454060;
  margin-left: 10rpx;
  font-size: 24rpx;
}
.desc {
  color: #949da6;
  font-size: 24rpx;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
}
.price {
  line-height: 32rpx;
  font-size: 32rpx;
  color: #f12f38;
}
.Discount {
  font-size: 26rpx;
  margin: 0 20rpx;
  color: #949da6;
}
.reduce {
  width: 162rpx;
  /* text-align: center; */
  display: inline-block;
  border: 1rpx solid #000000;
  border-radius: 25rpx;
  height: 50rpx;
  /* line-height: 30rpx; */
  font-size: 22rpx;
}
.jiage{
  margin: 28rpx 0 20rpx 0;
}
.jiangjia {
  width: 36rpx;
  height: 36rpx;
  position: relative;
  top: 7rpx;
  left: 14rpx;
  margin-right: 20rpx;
}
.dingjia {
  color: #949da6;
  font-size: 26rpx;
}
.old-price {
  text-decoration: line-through;
  color: #949da6;
  font-size: 26rpx;
}
</style>