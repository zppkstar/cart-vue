<template>
  <div class="container">
    <div class="row">
      <table class="table table-hover table-bordered">
        <caption class="h2 text-warning text-center">购物车</caption>
        <thead>
          <tr>
            <th>全选
              <input type="checkbox" v-model="checkAll" @change="change()">
            </th>
            <th>商品</th>
            <th>单价</th>
            <th>数量</th>
            <th>小计</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product,index) in products" :key="index">
            <td>
              <input type="checkbox" v-model="product.isSelected" @change="checkOne()">
            </td>
            <td>
              <img :src="product.productCover" alt>
              {{product.productName}}
            </td>
            <td>{{product.productPrice | toFixed(2)}}</td>
            <td>
              <input type="number" v-model.number="product.productCount" min="1">
            </td>
            <td>{{product.productPrice*product.productCount |toFixed(2)}}</td>
            <td>
              <button class="btn btn-danger" @click="remove(product)">删除</button>
            </td>
          </tr>
          <tr>
            <td colspan="6">订单总额：{{sum() |toFixed(2)}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import carts from "@/data/carts.js";
export default {
  data() {
    return {
      checkAll: false,
      products: []
    };
  },
  created() {
    //钩子函数
    this.getData();
  },
  filters: {
    toFixed(input, param1) {
      return input.toFixed(param1); //input代表的是管道符前面的数字，param1带包的是toFixed里传的参数
    }
  },
  methods: {
    sum() {
      return this.products.reduce((prev, next) => {
        if (!next.isSelected) return prev;
        return prev + next.productPrice * next.productCount;
      }, 0);
    },
    checkOne() {
      //根据下面点击的结果，控制上面全选的状态
      this.checkAll = this.products.every(item => item.isSelected);
    },
    change() {
      //根据当前自己的状态设置其他人的状态，实现全选和反选
      this.products.forEach(item => {
        item.isSelected = this.checkAll;
      });
    },
    remove(p) {
      this.products = this.products.filter(item => item !== p);
    },
    getData() {
      this.products = carts;
      this.checkOne();
      // axios.get('./carts.json').then(res=>{
      // this.products=res.data;
      // this.checkOne();//数据获取完成后，给checkAll赋予默认值
      // },err=>{
      //     console.log("err");
      // });
    }
  }
};
</script>
