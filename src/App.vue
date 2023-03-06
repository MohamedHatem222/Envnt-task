<template>
  <v-app>
    <Navbar @searchProducts="searchProducts" :cart="cart" :subtotalCart="subtotalCart" />
    <v-main class="d-flex align-center">
      <router-view :products="filteredProducts" @updateList="updateList" @addCart="addCart"></router-view>
    </v-main>
    <Footer v-if="pageLoaded" />
  </v-app>
</template>

<script>
import axios from "axios";
import Navbar from "./components/Navbar";
import Footer from "./components/Footer";

export default {
  name: "App",
  data: () => {
    return {
      pageLoaded: false,
      products: null,
      searchQuery: "",
      filteredCat: "",
      cart: [],
      filteredItems: []
    };
  },
  components: {
    Navbar,
    Footer
  },
  computed: {
    subtotalCart: function() {
      let sum = 0;
      this.cart.map(p => {
        sum += parseInt(p.price) * p.quantity;
      });

      return sum;
    },
    filteredProducts: function() {
      return this.filteredCat
        ? this.products.filter((item) => item.category == this.filteredCat)
        : this.products;
    }
  },
  mounted: function() {
    this.cart = localStorage.getItem("cart")
      ? JSON.parse(localStorage.getItem("cart"))
      : [];

    axios.get("https://dummyjson.com/products").then(res => {
      this.products = res.data.products;
    });

    setTimeout(() => (this.pageLoaded = true), 2000);
  },
  methods: {
    searchProducts: function(query) {
      this.searchQuery = query;
      return this.products.filter((item) => {
        return this.searchQuery.toLowerCase().split(' ').every(v => item.title.toLowerCase().includes(v));
      });
    },
    updateList(query2) {
      this.filteredCat = query2
    },
    addCart: function(products) {
      this.cart = products;
      localStorage.setItem("cart", JSON.stringify(products));
    }
  }
};
</script>

<style scoped>

</style>
