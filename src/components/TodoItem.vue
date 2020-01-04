<template>
  <div>
  <li class="list__item">
    <span class="list__item__span" v-bind:class="{ done: item.done }" @click="toggleItem(item.key)" >{{ item.text }}</span>
    <button class="list__item--delete-btn">
    <i class="btn ion-ios-close-outline" @click="deleteItem(item.key)" ></i></button>
  </li>
  </div>
</template>

<script>
export default {
  name: 'TodoItem',
  props: ['item'],
  methods: {
    deleteItem(key) {
      this.$emit('delete', key);
    },
    toggleItem(key) {
      this.$emit('toggle', key)
    }
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.list__item {
  padding: 10px;
  border-top: 1px solid #eee;
}

.list__item--delete-btn {
  margin-left: 10px;
  background-color: rgb(38, 228, 221);
  border: none;
  transition: all .2s;
}

.list__item--delete-btn:hover {
  transform: scale(1.2);
}

.list__item .done {
  color: #888;
  text-decoration: line-through;
}

</style>
