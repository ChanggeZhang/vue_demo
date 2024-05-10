<template>
  <div class="orders-container">
    <div class="order-group" v-for="order in orderList" :key="order.name">
      <h3 class="group-title">{{order.name}}</h3>
      <div class="order-container">
        <div v-for="item in order.list" :key="item.name">
          <order-group :group-name="order.name" :item="item" :ordered="orderedList.filter(o => o.name == item.name)" @choseGoods="chooseGoods"></order-group>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
 import OrderGroup from "@/components/OrderGroup.vue";

 export default {
   name: "OrderedList",
   components: {OrderGroup},
   props: {
     orderedList: {
       type: Array,
       required: true,
       default: []
     },
     orderList: {
       type: Array,
       required: true,
       default: []
     }
   },
   methods: {
     chooseGoods(chosen){
       this.$emit('orderCheck',chosen)
     }
   }
 }
</script>

<style scoped>
.orders-container {
  margin-top: 2rem;
  display: grid;
  position: relative;
}

.order-group {
  flex: 1;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;

  color: var(--color-text);
}

h3 {
  font-size: 1rem;
  font-weight: bold;
  padding-left: 1rem;
  background-color: var( --vt-c-black-soft);
  color: var(--color-background-mute);
}

@media (min-width: 1024px) {
  .item {
    margin-top: 0;
    padding: 0.4rem 0 1rem calc(var(--section-gap) / 2);
  }

  i {
    top: calc(50% - 25px);
    left: -26px;
    position: absolute;
    border: 1px solid var(--color-border);
    background: var(--color-background);
    border-radius: 8px;
    width: 50px;
    height: 50px;
  }

  .item:before {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    bottom: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:after {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    top: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:first-of-type:before {
    display: none;
  }

  .item:last-of-type:after {
    display: none;
  }
}
</style>
