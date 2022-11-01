<template>
  <div class="Header">{{ title }}</div>
</template>

<script>
export default {
  props: {
    title: {
      default: "",
      require: true,
    },
  },
};
</script>

<style>
.Header {
  width: 100%;
  height: 40px;
  background-color: #127aff;
  text-align: center;
  line-height: 40px;
  color: #e8e8e8;
  position: fixed;
  top: 0;
  z-index: 999;
}
</style>
