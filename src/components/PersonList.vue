<template>
  <div class="person-list-selected">
    <div class="person" v-for="p in persons" :key="p.name" >
      <input type="radio" name="personName" :value="p.name" :checked="p.name === this.dp" @change="selectPerson" />{{p.name}}
    </div>
  </div>
</template>

<script>
export default {
  name: "PersonList",
  props: {
    persons: {
      type: Array,
      required: true
    },
    defaultPerson: {
      type: String,
      required: true
    }
  },
  watch: {
    defaultPerson(newV,oldV) {
      this.dp = newV
    }
  },
  data(){
    return {
      dp: ''
    }
  },
  methods: {
    selectPerson(e){
      this.dp = e.target.value
      this.$emit('changePerson',this.dp)
    }
  }

}
</script>

<style scoped>

.person-list-selected{
  display: flex;
  gap: 41px;
}
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
