<template>
  <div id="app">
    <Header title="购物车案例"></Header>
    <List :list="list"></List>
    <Footer></Footer>
  </div>
</template>
<script>
import Header from "@/components/Header/Header.vue";
import Footer from "@/components/Footer/Footer.vue";
import List from "@/components/List/List.vue";
import axios from "axios";
export default {
  data() {
    return { list: [] };
  },
  components: {
    Header,
    List,
    Footer,
  },
  created() {
    this.getlist();
  },
  methods: {
    async getlist() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      console.log(res);
      this.list = res.list;
    },
  },
};
</script>
<style scoped></style>
