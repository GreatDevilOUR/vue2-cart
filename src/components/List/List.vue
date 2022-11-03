<template>
  <div class="list">
    <div
      :class="['checkboxFour', state == true ? 'checkboxFourAc' : '']"
      @click="onchecp(id)"
    ></div>
    <img :src="img" alt="" />
    <div class="content">
      <p class="title">{{ name }}</p>
      <div class="upper">
        <p class="price">￥{{ price }}</p>
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    id: {
      required: true,
      type: Number,
    },
    name: {
      default: "",
      type: String,
    },
    img: {
      default: "",
      type: String,
    },
    price: {
      default: "0.0",
    },
    count: {
      default: 0,
      type: Number,
    },
    state: {
      default: true,
      type: Boolean,
    },
  },
  methods: {
    onchecp(id) {
      this.$emit("onChange-state", { id, state: this.state });
    },
  },
};
</script>

<style scoped>
.list {
  width: 100%;
  height: 150px;
  display: flex;
  justify-content: left;
  align-items: center;
  padding: 16px 5px;
  box-sizing: border-box;
}
.checkboxFour {
  margin-right: 5px;
  text-align: center;
  width: 15px;
  height: 15px;
  border-radius: 15px;
  border: 1px solid;
  position: relative;
}
.checkboxFourAc {
  border: 1px solid #127aff;
  background: #127aff;
}
.checkboxFourAc::before {
  opacity: 1;
  content: "";
  position: absolute;
  width: 3px;
  height: 7px;
  background: transparent;
  top: 2.5px;
  right: 5px;
  border: 2px solid #fff;
  border-top: none;
  border-left: none;
  transform: rotate(35deg);
  -moz-transform: rotate(35deg);
  -webkit-transform: rotate(35deg);
  -o-transform: rotate(35deg);
}
.list img {
  width: 100px;
  height: 120px;
  margin-right: 10px;
}
.content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: left;
  flex: 1;
  height: 100%;
}
.upper {
  display: flex;
  justify-content: space-between;
}
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
.title {
  font-size: 14px;
}
.price {
  color: #bd2d30;
}
</style>
