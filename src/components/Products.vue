<template>
  <div class="container mx-auto">
    <v-container fluid>
      <v-row dense>
        <v-col cols="12" class="d-flex justify-center mb-2">
          <span>all - {{ filteredCat }}</span>
        </v-col>
      </v-row>
      <v-divider></v-divider>
      <v-row dense class="justify-end mt-2">
      <v-col cols="12" sm="6" md="6">
        <v-select
          v-model="filteredCat"
          :items="items"
          label="Standard"
          @change="updateList()"
        ></v-select>
        </v-col>
      </v-row>
      <v-row dense>
        <v-col v-for="item in products" :key="item.id" cols="12" xs="6" md="4" sm="6">
          <v-skeleton-loader v-if="loadingProducts" type="image"></v-skeleton-loader>
          <v-card v-else>
            <v-list-item-title class="headline">
              <v-list-item>
              <v-list-item-content>
                <v-list-item-title class="headline">
                  <h6>{{item.category}}</h6>
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            </v-list-item-title>
            <v-img :src="item.images[0]" class="white--text align-end" style="height: 300px;"></v-img>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title class="headline">
                  <h6>{{item.title}}</h6>
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-card-actions>
              <v-chip class="mr-2" color="primary lighten-1">
                €{{parseInt(item.price).toFixed(2)}}
              </v-chip>
              <v-spacer></v-spacer>
              <v-btn depressed color="primary"  v-on:click="addToCart(item)">
                <!-- <v-icon>add_shopping_cart</v-icon> -->
                Add To Cart
              </v-btn>
              <!-- <v-btn v-if="isAddToCart" icon v-on:click="removeCart(item)">
                <span>Remove To Cart</span>
              </v-btn> -->
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
      <v-fab-transition>
        <v-btn
          fab
          color="white"
          v-scroll="onScrollBtn"
          v-show="showBtnScrollUp"
          @click="goToTop"
          
          fixed
          right
          bottom
        >
          <v-icon>keyboard_arrow_up</v-icon>
        </v-btn>
      </v-fab-transition>
      <v-snackbar v-model="snackbar.visible" bottom :timeout="snackbar.timeout">
        {{ snackbar.text }}
      </v-snackbar>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "Products",
  props: ["products"],
  data: () => {
    return {
      isAddToCart: false,
      loadingProducts: true,
      cart: [],
      filteredItems: [],
      filteredCat: null,
      showBtnScrollUp: false,
      snackbar: {
        visible: false,
        timeout: 700,
        text: ""
      },
      sharewithsheet: {
        visible: false
      },
      items:['laptops', 'smartphones', 'fragrances', 'skincare'],
    };
  },
  watch: {
    cart: function() {
      this.$emit("addCart", this.cart);
    },
    
  },
  mounted: function() {
    this.products= this.filteredItems
    this.cart = localStorage.getItem("cart")
      ? JSON.parse(localStorage.getItem("cart"))
      : [];

      setTimeout(() => (this.loadingProducts = false), 2000);
  },
  methods: {
    // updateList() {
    //   this.filteredItems = this.products.filter((item) => item.category == this.filteredCat);
    //   return this.filteredItems
    // },
    updateList: function() {
      this.$emit("updateList", this.filteredCat);
    },
    goToTop: function() {
      this.$vuetify.goTo(0);
    },
    onScrollBtn: function(event) {
      this.showBtnScrollUp =
        (window.pageYOffset || event.target.scrollTop || 0) > 40;
    },
    productAddedMessage: function() {
      this.snackbar.visible = true;
      this.snackbar.text = "Product added to your cart";
    },
    addToCart: function(item) {
      let indexProduct = this.cart.findIndex(p => p.id === item.id);
      if (indexProduct > -1) {
        let tempProduct = this.cart[indexProduct];
        this.cart.splice(indexProduct, 1);
        tempProduct.quantity++;
        this.cart.push(tempProduct);
      } else {
        item.quantity = 1;
        this.cart.push(item);
      }
      this.isAddToCart = true
      this.productAddedMessage();
    },
    removeCart: function(item) {
      this.cart.splice(item, 1);
      localStorage.setItem("cart", JSON.stringify(this.cart));
      this.isAddToCart = false
    },
  }
};
</script>

<style>
.v-skeleton-loader__image {
  height: 308px;
}
</style>
