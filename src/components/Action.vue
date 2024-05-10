<template>
  <div class="action">
    <div class="plus" @click="pushOrder(1)">+</div>
    <input type="number" class="small-input" :value="num" readonly min="0" />
    <div class="remove" @click="pushOrder(-1)">-</div>
  </div>
</template>

<script>
export default {
  name: 'Action',
  props: {
    item: {
      type: Object,
      default: {},
      required: false
    },
    size: {
      type: String,
      required: false
    },
    num: {
      type: String,
      required: false,
      default: 0
    }
  },
  methods:{
    valid(e){
      if(isNaN(e.target.value) || parseInt(e.target.value) < 0){
        e.target.value = this.item.number || 0
      }
    },
    pushOrder(num){
      let chosen = {
        name: this.item.name,
        size: this.size,
        price: this.item.sizes ? this.querySize(this.item,this.size) : this.item.price,
        number: isNaN(num) ? (parseInt(num.srcElement.value) || 0) : num,
      }
      this.$emit('goodsOrder',chosen)
    },
    querySize(item,size){
      switch (size) {
        case 'big':
          return item.sizes.big;
        case 'middle':
          return item.sizes.middle;
        default:
          return item.sizes.small;
      }
    }
  }
}
</script>

<style scoped>
  .small-input{
    width: 3rem;
    height: 1rem;
  }
  .action{
    display: inline-flex;
  }
  .action>div{
    color: #ffffff;
    font-size: 1rem;
    width: 1rem;
    height: 1rem;
    text-align: center;
    line-height: 1rem;
  }
  .plus{
    background-color: #31af00;
  }
  .remove{
    background-color: #ff4400;
  }
</style>