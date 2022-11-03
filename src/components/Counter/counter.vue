<template>
  <div class="count">
    <button @click="menus(id)">-</button>
    <input type="text" readonly :value="count" />
    <button @click="add(id)">+</button>
  </div>
</template>

<script>
export default {
  props: {
    id: {
      required: true,
      type: Number,
    },
    count: {
      default: 0,
      type: Number,
    },
  },
  methods: {
    menus(id) {
      this.$emit("menus", id);
    },
    add(id) {
      this.$emit("add", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.count {
  display: flex;
}
.count input {
  width: 28px;
  line-height: 28px;
  text-align: center;
  background-color: #f2f3f5;
  border: none;
  margin: 0 2px;
}
.count button {
  background-color: #f2f3f5;
  border: none;
  width: 28px;
  height: 28px;
  font-size: 16px;
}
</style>
