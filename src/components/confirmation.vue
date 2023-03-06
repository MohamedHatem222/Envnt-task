<template>
     <v-container>
      <v-row dense>
        <v-col cols="12" xs="6" md="8" sm="7" class="d-flex align-center justify-center mb-4">
          <h2>Thank You For Your Order</h2>
        </v-col>
        <v-col cols="12" xs="6" md="4" sm="5">
          <v-card>
            <v-container>
          <div class="d-flex align-center flex-wrap" v-for="item in items" :key="item.id">
            <v-col cols="4" xs="6" md="4" sm="6">
              <img :src="item.images[0]" alt="" style="max-width: 100%;">
            </v-col>
            <v-col cols="8" xs="6" md="6" sm="6">
              <h3>{{ item.title }}</h3>
              <p>€{{ item.price }}</p>
            </v-col>
          </div>
          <v-divider></v-divider>
          <v-row class="mt-2">
              <v-col cols="7" class="text-left">Subtotal:</v-col>
              <v-col cols="5" class="text-right">
                <strong style="color:#388E3C;">€ {{parseInt(subtotalCart).toFixed(2)}}</strong>
              </v-col>
            </v-row>
          </v-container>
        </v-card>
        </v-col>
      </v-row>
    </v-container>
</template>
<script>
export default {
  name: "Confirmation",
  props: [],
  data: () => {
    return {
      items:[]
    };
  },
  computed: {
    subtotalCart: function() {
      let sum = 0;
      this.items.map(p => {
        sum += parseInt(p.price) * p.quantity;
      });

      return sum;
    },
  },
  mounted: function() {
    this.items = JSON.parse(localStorage.getItem("cart"))
  },
};
</script>