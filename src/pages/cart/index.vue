<template>
  <div class="wrap">
    <div class="main">
      <div class="cartItem" v-for="(item,index) in cartArr" :key="index">
        <div class="top">
          <div class="imgbox">
            <input type="radio" :checked="item.cheched" @click="check(item)" />
            <img class="itemImg" :src="item.pic" mode="widthFix" />
          </div>
          <div class="iteminfo">
            <div class="name">{{item.title}}<span @click="del(index)">X</span> </div>
            <div class="pri">
              <div class="price">
                <span class="new-price">￥{{item.price}}</span>
                <span class="old-price">￥{{item.fee}}</span>
              </div>
              <div class="count">
                <span class="btns" @click="sub(item)">-</span>
                {{item.count}}
                <span class="btns" @click="add(item)">+</span>
              </div>
            </div>
          </div>
        </div>
        <div class="bottom">
          <div class="buy">
            <span class="jiajia">加价购</span>
            <span>购买一件，即可享受换购优惠</span>
          </div>
          <div class="choose">
            <span class="gochoose">去选择</span>
            <img class="righticon" src="/static/images/right.png" alt />
          </div>
        </div>
      </div>
    </div>
    <div class="footer">
      <div class="allPrice">
        <input type="radio" :checked="checkAlls" @click="allCheck" />
        <span>全选</span>
        <span>合计：</span>
        <span>￥{{sum}}</span>
      </div>
      <div class="balance">
        <span class="gobalance">去结算</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      cartArr: [],
      checkAlls: true,
      sum: 0
    };
  },
  computed: {},
  methods: {
      del(index){
          this.cartArr.splice(index,1)
          wx.setStorageSync('cart',this.cartArr)
          this.allPrice()
      },
      //点击全选按钮
    allCheck() {
      this.checkAlls = !this.checkAlls;
      this.cartArr.forEach(item => {
        item.cheched = this.checkAlls;
      });
      wx.setStorageSync('cart', this.cartArr)
      this.allPrice()
    },
    //总价
    allPrice() {
      let allItem = wx.getStorageSync("cart");
      let newArr = allItem.filter(item => {
        return item.cheched;
      });
      let sum = 0;
      newArr.forEach(item => {
        sum += item.price * item.count;
      });
      this.sum = sum;
      console.log(newArr);
    },
    // 点击每一项的radio按钮
    check(ite) {
      // console.log(ite.cheched)
      let itemArr = wx.getStorageSync("cart");
      let itemObj = itemArr.find(item => {
        return item.id == ite.id;
      });
      itemObj.cheched = !itemObj.cheched;
      wx.setStorageSync("cart", itemArr);
      console.log(itemObj.cheched);
      ite.cheched = itemObj.cheched;
      this.checkAll();
      this.allPrice();
    },
    sub(ite) {
      let itemArr = wx.getStorageSync("cart");
      let itemObj = itemArr.find(item => {
        return item.id == ite.id;
      });
      console.log(itemObj);
      if (itemObj.count == 1) {
        itemObj.count = 1;
      } else {
        itemObj.count -= 1;
      }
      wx.setStorageSync("cart", itemArr);
      if (ite.count == 1) {
        ite.count = 1;
      } else {
        ite.count--;
      }
      this.allPrice();
    },
    add(ite) {
      let itemArr = wx.getStorageSync("cart");
      let itemObj = itemArr.find(item => {
        return item.id == ite.id;
      });
      console.log(itemObj);
      itemObj.count += 1;
      wx.setStorageSync("cart", itemArr);
      ite.count++;
      this.allPrice();
    },
    // 如果每一项radio 都选中
    checkAll() {
      let allItem = wx.getStorageSync("cart");
      let bool = allItem.every(item => {
        return item.cheched == true;
      });
      this.checkAlls = bool;
      //   return bool;
    }
  },
  onLoad() {
    let cartArr = wx.getStorageSync("cart");
    console.log(cartArr);
    this.cartArr = cartArr;
    this.allPrice();
  },
  created() {}
};
</script>
<style scoped>
.name{
    display: flex;
    justify-content: space-between;
}
.gobalance {
  display: block;
  width: 238rpx;
  height: 90rpx;
  background-color: #f12617;
  color: #ffffff;
  text-align: center;
  line-height: 90rpx;
  border-radius: 45rpx;
}
.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #ffffff;
  line-height: 110rpx;
  padding-left: 36rpx;
  padding-right: 20rpx;
}
.jiajia {
  border: 1rpx solid #ec315c;
  color: #ec315c;
  border-radius: 6rpx;
  padding: 3rpx 6rpx;
  margin-right: 10rpx;
}
.choose {
  font-size: 26rpx;
  line-height: 26rpx;
}
.buy {
  font-size: 24rpx;
  line-height: 24rpx;
  color: #919191;
}
.bottom {
  display: flex;
  padding: 40rpx 30rpx 40rpx 60rpx;
  justify-content: space-between;
  align-items: center;
}
.choose {
  display: flex;
  align-items: center;
}
.righticon {
  width: 30rpx;
  height: 30rpx;
}
.btns {
  display: inline-block;
  width: 50rpx;
  height: 50rpx;
  line-height: 50rpx;
  text-align: center;
  border-radius: 50%;
  background-color: #f4f4f4;
}
.old-price {
  color: #bdbdbd;
  font-size: 20rpx;
  text-decoration: line-through;
}
.new-price {
  font-size: 28rpx;
  color: #fd1813;
  margin-right: 14rpx;
}
.price {
  margin-right: 50rpx;
}
.pri {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.iteminfo {
  width: 500rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 30rpx 30rpx 0 20rpx;
}
.itemImg {
  width: 130rpx;
}
.imgbox {
  display: flex;
  align-items: center;
  height: 230rpx;
  padding-left: 20rpx;
}
.top {
  /* width: 750rpx; */
  height: 230rpx;
  display: flex;
}
.wrap {
  background-color: #f3f3f3;
}
.main {
  width: 750rpx;
}
.cartItem {
  margin: 20rpx auto;
  width: 710rpx;
  height: 330rpx;
  background-color: #fff;
  border-radius: 20rpx;
}
.header {
  background-color: #fff;
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
</style>