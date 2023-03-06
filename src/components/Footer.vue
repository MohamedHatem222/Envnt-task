<template>
  <v-footer dark padless class="d-flex justify-center primary darken-1 white--text text-center">
    <v-card flat tile class="primary darken-1 white--text text-center">
      <v-card-text
        class="white--text pt-5"
      >
      </v-card-text>
      <!-- <v-divider></v-divider> -->
      <v-card-text class="white--text">
        {{ new Date().getFullYear() }} —
        <strong>Made by 
          <v-tooltip top>
            <template v-slot:activator="{ on, attrs }">
              <a
                class="footer-link"
                href="https://github.com/MohamedHatem222"
                target="_blank"
                v-bind="attrs"
                v-on="on"
              >
                Mohamed Hatem
              </a>
            </template>
            <span>Move to GitHub Account and Follow Me 😄</span>
          </v-tooltip>
        </strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  name: "Footer"
};
</script>

<style scoped>
.footer-link {
  text-decoration: none;
  color: #a1a1a1;
  font-size: 16px;
}

.footer-link:hover {
  color: greenyellow;
}
</style>
