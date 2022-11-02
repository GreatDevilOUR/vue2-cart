<template>
  <div class="box">
    <div class="list" v-for="item in arrlist" :key="item.id">
      <div
        :class="['checkboxFour', ids.includes(item.id) ? 'checkboxFourAc' : '']"
        @click="onchecp(item.id)"
      ></div>
      <img :src="item.goods_img" alt="" />
      <div class="content">
        <p class="title">{{ item.goods_name }}</p>
        <div class="upper">
          <p class="price">￥{{ item.goods_price }}</p>
          <div class="count">
            <button @click="menus(item.id)">-</button>
            <input type="text" readonly v-model="item.count" />
            <button @click="add(item.id)">+</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import eventBus from "@/until/eventBus.js";
export default {
  props: {
    list: {
      type: Array,
    },
  },
  data() {
    return { ids: [], arrlist: [], isCheck: false };
  },
  created() {
    eventBus.$on("checkAll", (isfa) => {
      console.log("123");
      this.getAll(isfa);
    });
  },
  mounted() {
    this.getlist();
  },
  watch: {
    list(newval) {
      this.arrlist = newval;
      this.getlist();
    },
  },
  computed: {
    getSum() {
      let sum = this.arrlist
        .filter((item) => this.ids.includes(item.id))
        .reduce((sum, current) => {
          return (sum += current.goods_price * current.count);
        }, 0);
      return sum;
    },
    setAllCheck() {
      return this.ids.length === this.arrlist.length;
    },
  },
  methods: {
    onchecp(id) {
      if (!this.ids.includes(id)) {
        this.ids.push(id);
        this.arrlist
          .filter((item) => item.id == id)
          .forEach((item) => (item.count = 1));
      } else {
        let index = this.ids.indexOf(id);
        this.ids.splice(index, 1);
        this.arrlist
          .filter((item) => item.id == id)
          .forEach((item) => (item.count = 0));
      }
      eventBus.$emit("getSum", this.getSum, this.ids.length);
      if (this.setAllCheck) {
        eventBus.$emit("setAll", true);
      } else {
        eventBus.$emit("setAll", false);
      }
    },
    getlist() {
      this.arrlist = this.list;
      this.arrlist.forEach((item) => {
        this.$set(item, "count", 0); //利用$set属性给列表添加一项
      });
    },
    add(id) {
      if (this.ids.includes(id)) {
        this.arrlist
          .filter((item) => item.id == id)
          .forEach((item) => {
            item.count += 1;
          });
      } else {
        this.ids.push(id);
        this.arrlist
          .filter((item) => item.id == id)
          .forEach((item) => {
            item.count += 1;
          });
      }
      eventBus.$emit("getSum", this.getSum, this.ids.length);
      if (this.setAllCheck) {
        eventBus.$emit("setAll", true);
      } else {
        eventBus.$emit("setAll", false);
      }
    },
    menus(id) {
      if (this.ids.includes(id)) {
        this.arrlist
          .filter((item) => item.id == id)
          .forEach((item) => {
            if (item.count >= 1) {
              item.count -= 1;
              if (this.ids.includes(id) && item.count == 0) {
                let index = this.ids.indexOf(id);
                this.ids.splice(index, 1);
              }
            }
          });
      }
      eventBus.$emit("getSum", this.getSum, this.ids.length);
      if (this.setAllCheck) {
        eventBus.$emit("setAll", true);
      } else {
        eventBus.$emit("setAll", false);
      }
    },
    getAll(isCheckAll) {
      console.log("兄弟组件数据", isCheckAll);
      if (isCheckAll) {
        this.arrlist.forEach((item) => {
          if (!this.ids.includes(item.id)) {
            this.ids.push(item.id);
            this.arrlist.forEach((item) => (item.count = 1));
          }
        });
        // eventBus.$emit("getSum", this.getSum, this.ids.length);
      } else {
        this.ids = [];
        this.arrlist.forEach((item) => (item.count = 0));
      }
      eventBus.$emit("getSum", this.getSum, this.ids.length);
      if (this.setAllCheck) {
        eventBus.$emit("setAll", true);
      } else {
        eventBus.$emit("setAll", false);
      }
    },
  },
};
</script>

<style scoped>
.box {
  margin-top: 30px;
  margin-bottom: 30px;
}
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
