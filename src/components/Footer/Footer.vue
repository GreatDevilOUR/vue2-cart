<template>
  <div class="box">
    <div class="check">
      <span
        :class="['checkAll', isAllBtn ? 'checkAllACtive' : '']"
        @click="checkAllbtn"
      ></span>
      <span>全选</span>
    </div>
    <div class="count">
      <p>合计:</p>
      <P>￥{{ this.sum }}</P>
    </div>
    <div class="res">
      <button>结算（{{ this.count }}）</button>
    </div>
  </div>
</template>

<script>
import eventBus from "@/until/eventBus.js";
export default {
  data() {
    return {
      isAllBtn: false,
      sum: "",
      count: "",
    };
  },
  created() {
    eventBus.$on("getSum", this.getInfo);
  },
  methods: {
    checkAllbtn() {
      eventBus.$emit("checkAll", !this.isAllBtn);
      this.isAllBtn = !this.isAllBtn;
    },
    getInfo(sum, count) {
      this.sum = sum;
      this.count = count;
    },
  },
};
</script>

<style scoped>
.box {
  position: fixed;
  bottom: -1px;
  height: 40px;
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  font-size: 12px;
  background-color: #e8e8e8;
}
.check {
  display: flex;
  align-items: center;
  text-align: center;
}
.checkAll {
  display: block;
  height: 15px;
  width: 15px;
  border-radius: 15px;
  border: 1px solid;
  position: relative;
}
.checkAllACtive {
  background: #127aff;
  border: 1px solid #127aff;
}
.checkAllACtive::before {
  opacity: 1;
  content: "";
  position: absolute;
  width: 4px;
  height: 8px;
  border: 1px solid #e8e8e8;
  border-left: none;
  border-top: none;
  -moz-transform: rotate(35deg);
  -webkit-transform: rotate(35deg);
  -o-transform: rotate(35deg);
  left: 4px;
  top: 2px;
  background: transparent;
}
.res button {
  border-radius: 10px;
}
.count {
  display: flex;
}
</style>
