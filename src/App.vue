
<template>
<!--  选择客户-->
  当前客户{{checkedPersonName}}
  <person-list :persons="this.Persons" :defaultPerson="checkedPersonName" @changePerson="changeChecked"></person-list>
<!--  客户可选单子-->
  <order-list :order-list="this.OrderList" :ordered-list="this.checkedPerson.orders" @orderCheck="orderCheck"></order-list>
<!--客户一点单子价格-->
  <div class="total-price">{{checkedPerson.totalPrice}}</div>
</template>

<script>
  import PersonList from "@/components/PersonList.vue";
  import OrderList from "@/components/OrderList.vue";
  import {isProxy} from "vue";

  export default {
    components: {PersonList,OrderList},
    mounted() {
      this.changeChecked()
    },
    data() {
      return {
        Persons: [
          {
            name: '张三',
            orders: [],
            totalPrice: 0
          },{
            name: '李四',
            orders: [],
            totalPrice: 0
          }
        ],
        OrderList: [{
          name: '单点',
          list: [
              {
            name: '牛肉面',
            sizes: {
              big: 18,
              middle: 16,
              small: 14
            }
          }, {
            name: '肥肠面',
            sizes: {
              big: 20,
              middle: 18,
              small: 16
            }
          }]
        },{
          name: '小食',
          list: [
              {
            name: '牛肉饼',
            price: 10
          }]
        },{
          name: '饮品',
          list: [{
            name: '奶茶',
            price: 12
          }]
        },{
          name: '套餐',
          list: [
              {
            name: '套餐1',
            goods: [{
              name: '奶茶',
              num: 1
            },{
              name: '牛肉面',
              size: 'big',
              num: 1
            },{
              name: '牛肉饼',
              num: 1
            }],
            price: 38
          }, {
            name: '套餐2',
            goods: [{
              name: '奶茶',
              num: 1
            },{
              name: '肥肠面',
              size: 'big',
              num: 1
            },{
              name: '牛肉饼',
              num: 1
            }],
            price: 40
          }]
        }],
        checkedPerson: {},
        checkedPersonName: ''
      }
    },
    methods: {
      changeChecked(val){
        this.checkedPerson = val ? this.Persons.find(p => p.name === val) : this.Persons[0]
        this.checkedPersonName = this.checkedPerson.name;
      },
      orderCheck(chosen){
        if(isProxy(chosen)) return true
        let order = this.checkedPerson.orders.find(o => this.comparePerson(o,chosen))
        if(order){
          order.number += chosen.number
          if(order.number <= 0){
            let orders = this.checkedPerson.orders.filter(o => !this.comparePerson(o,chosen))
            this.checkedPerson.orders = orders
          }
        }else if(chosen.number > 0){
          this.checkedPerson.orders.push(chosen)
        }
        this.calculatePrice()
      },
      calculatePrice(){
        let totalPrice = (this.checkedPerson.orders && this.checkedPerson.orders.length) ? this.checkedPerson.orders.map(o =>
            (o.price || 0) * (o.number || 0)
        ).reduce((previousValue, currentValue) => previousValue + currentValue) : 0
        this.checkedPerson.totalPrice = totalPrice
      },
      comparePerson(o,a){
        return o.name == a.name && (a.size ? o.size == a.size : true)
      }
    }
  }
</script>

<style scoped>
header {
  line-height: 1.5;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
