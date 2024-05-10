<template>
    <div  :class="['items-container',ordered && ordered.length ? 'active-item' : '']">
      <div class="title">{{item.name}}</div>
      <div class="package-content"  v-if="groupName !== '套餐'"></div>
      <div class="sizes" v-if="item.sizes">
        <div :class="['size-pack',ordered.find(o => o.size == 'big') ? 'active-item' : '']">大
          <span class="price">{{item.sizes.big}}</span>
          <Action :item="item" :num="(ordered.find(o => o.size == 'big')?.number || 0) + ''" :size="'big'" @goodsOrder="orderGoods"></Action>
        </div>
        <div :class="['size-pack',ordered.find(o => o.size == 'middle') ? 'active-item' : '']">中
          <span class="price">{{item.sizes.middle}}</span>
          <Action :item="item" :num="(ordered.find(o => o.size == 'middle')?.number || 0) + ''" :size="'middle'" @goodsOrder="orderGoods"></Action>
        </div>
        <div :class="['size-pack',ordered.find(o => o.size == 'small') ? 'active-item' : '']">小
          <span class="price">{{item.sizes.small}}</span>
          <Action :item="item" :num="(ordered.find(o => o.size == 'small')?.number || 0) + ''" :size="'small'" @goodsOrder="orderGoods"></Action>
        </div>
      </div>
      <div class="package-content" v-if="groupName === '套餐'">
        {{groupName === '套餐' ? item.goods.map(goods => `${goods.num}份${sizes[goods.size] || ''}${goods.name}`).join(' + ') : ''}}
      </div>
      <span class="price" v-if="!item.sizes">{{item.price}}
      </span>
      <Action v-if="!item.sizes" :item="item" :num="(ordered && ordered.length ? ordered[0].number : 0) + ''" @goodsOrder="orderGoods"></Action>
    </div>
</template>

<script>
  import Action from "@/components/Action.vue";

  export default {
    name: 'OrderGroup',
    components: {Action},
    props: {
      groupName: {
        type: String,
        required: true,
        default: ''
      },
      item: {
        type: Object,
        required: true,
        default: () => {}
      },
      ordered: {
        type: Array,
        required: false,
        default: () => {}
      }
    },
    data(){
      return {
        sizes: {
          big: '大碗',
          middle: '中碗',
          small: '小碗'
        }
      }
    },
    methods: {
      orderGoods(item){
        this.$emit('choseGoods',item)
      }
    }
  }
</script>

<style scoped>

.items-container{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
.items-container.active{
  background-color: aquamarine;
}
.sizes{
  display: inline-flex;
  gap: 0.8rem;
}
.sizes.active{
  background-color: deepskyblue;
}
 .size-pack{
   display: flex;
 }
 span.price{
   text-align: center;
 }
</style>
