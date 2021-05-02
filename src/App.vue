<template>
  <v-app>
    <v-app-bar
      app
      color="#0F4C81"
      dark
    >
      <div class="d-flex align-center">
        <a href="/" class="font-weight-bold text-h4" style="color: #fff;text-decoration: none;">NCB</a>
      </div>
      <v-spacer></v-spacer>
      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Contact</span>
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
