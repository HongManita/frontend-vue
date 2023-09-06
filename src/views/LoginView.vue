<template>
  <v-container>
  <v-row justify="center">
    <v-col cols="12" align="center">
      <LoginForm />
    </v-col>
  </v-row>
</v-container>
</template>

<script>
import LoginForm from "../components/Login.vue";
export default {
  components: {
    LoginForm,
  },
};
</script>
