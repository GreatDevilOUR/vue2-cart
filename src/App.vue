<template>
  <div id="app">
    <Header title="购物车案例"></Header>
    <div class="box">
      <List
        v-for="item in list"
        :key="item.id"
        :id="item.id"
        :name="item.goods_name"
        :img="item.goods_img"
        :price="item.goods_price"
        :count="item.goods_count"
        :state="item.goods_state"
        @onChange-state="changeState"
      >
        <counter
          :id="item.id"
          :count="item.goods_count"
          @menus="menus"
          @add="add"
        ></counter>
      </List>
    </div>
    <Footer
      @checkAll="checkAll"
      :isFull="isFull"
      :sum="getSum"
      :count="fullNum"
    ></Footer>
  </div>
</template>
<script>
import Header from "@/components/Header/Header.vue";
import Footer from "@/components/Footer/Footer.vue";
import List from "@/components/List/List.vue";
import axios from "axios";
import Counter from "@/components/Counter/counter.vue";
export default {
  data() {
    return { list: [] };
  },
  components: {
    Header,
    List,
    Footer,
    Counter,
  },
  created() {
    this.getlist();
  },
  computed: {
    isFull() {
      return this.list.every((item) => item.goods_state);
    },
    getSum() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((am, item) => (am += item.goods_price * item.goods_count), 0);
    },
    fullNum() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((am, item) => (am += item.goods_count), 0);
    },
  },
  methods: {
    menus(id) {
      this.list.some((item) => {
        if (item.id == id && item.goods_count > 1) {
          item.goods_count = item.goods_count - 1;
        }
      });
    },
    add(id) {
      this.list.some((item) => {
        if (item.id == id) {
          item.goods_count = item.goods_count + 1;
        }
      });
    },
    async getlist() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      console.log(res);
      this.list = res.list;
    },
    changeState(info) {
      console.log(info);
      this.list.some((item) => {
        if (item.id == info.id) {
          item.goods_state = !info.state;
          return true;
        }
      });
    },
    checkAll(isStatue) {
      console.log(isStatue);
      this.list.forEach((item) => {
        item.goods_state = !isStatue;
      });
    },
  },
};
</script>
<style scoped>
.box {
  margin-top: 30px;
  margin-bottom: 30px;
}
</style>
