<template>
  <div class="bottom-menu">
    <check-button 
      class="select-all" 
      :class="{ 'checked': isSelectAll }"
      @click.native="selectAllClick" />
    <span @click="selectAllClick">全选</span>
    <span class="total-price">合计: ¥{{totalPrice}}</span>
    <span class="buy-product" @click="calcClick">去计算({{checkLength}})</span>
  </div>
</template>

<script>
import CheckButton from './CheckButton'

import { mapGetters } from 'vuex'

export default {
  name: "BottomBar",
  components: {
    CheckButton
  },
  computed: {
    ...mapGetters(['cartList']),
    totalPrice() {
      return this.cartList
        .filter(item => {
          return item.check === true
        })
        .reduce((prevValue, item) => {
          return prevValue + item.newPrice * item.count
        }, 0)
        .toFixed(2)
    },
    checkLength() {
      return this.cartList.filter(item => {
        return item.check
      }).length
    },
    isSelectAll() {
      if (this.cartList.length === 0) return false
      return this.cartList.every(item => item.check)
    }
  },
  methods: {
    selectAllClick() {
      this.$store.commit('selectAllClick',this.isSelectAll)
    },
    calcClick() {
      if (!this.checkLength) {
        this.$toast.show('请选择购买的商品')
      }
    }
  },
}
</script>

<style scoped>
.bottom-menu {
  width: 100vw;
  height: 40px;
  background-color: #eee;
  position: fixed;
  bottom: 50px;
  left: 0;
  box-shadow: 0 -2px 3px rgba(0, 0, 0, 0.2);
  font-size: 14px;
  color: #888;
  line-height: 40px;
  padding-left: 35px;
  box-sizing: border-box;
}

.bottom-menu .select-all {
  position: absolute;
  line-height: 0;
  left: 12px;
  top: 11px;
}

.bottom-menu .total-price {
  margin-left: 15px;
  font-size: 16px;
  color: #666;
}

.bottom-menu .buy-product {
  background-color: orangered;
  color: #fff;
  width: 100px;
  height: 44px;
  text-align: center;
  line-height: 44px;
  float: right;
}

.checked {
  background-color: #ff0000;
  border-color: #ff0000;
}
</style>
