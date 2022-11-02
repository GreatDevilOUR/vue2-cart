<template>
  <div class="box">
    <div class="check">
      <span
        :class="['checkAll', 'sumac', isAllBtn ? 'checkAllACtive' : '']"
        @click="checkAllbtn"
      ></span>
      <span>全选</span>
    </div>
    <div class="count">
      <p class="sumac">合计:</p>
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
      sum: 0,
      count: 0,
    };
  },
  created() {
    eventBus.$on("getSum", this.getInfo);
    eventBus.$on("setAll", this.setAll);
  },
  methods: {
    checkAllbtn() {
      eventBus.$emit("checkAll", !this.isAllBtn);
      // this.isAllBtn = !this.isAllBtn;
    },
    getInfo(sum, count) {
      this.sum = sum;
      this.count = count;
    },
    setAll(isbollen) {
      if (isbollen) {
        this.isAllBtn = true;
      } else {
        this.isAllBtn = false;
      }
    },
  },
};
</script>

<style scoped>
.box {
  position: fixed;
  bottom: -2px;
  height: 40px;
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  font-size: 12px;
  background-color: #faf9f9;
  border-top: 1px solid #e8e8e8;
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
.sumac {
  margin-right: 5px;
}
.res {
  font-size: 14px;
}
.res button {
  border: none;
  color: #faf9f9;
  background-color: #127aff;
  height: 30px;
  width: 98px;
  border-radius: 41px;
}
</style>
