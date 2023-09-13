<template>
  <v-form ref="form" :lazy-validation="lazy">
    <v-card width="50%" height="100%">
      <v-card-title>เข้าสู่ระบบ</v-card-title>
      <v-card-text>
        <!-- username -->
        <v-row dense>
          <v-col cols="12" align="start">
            <v-text-field
           
              v-model="username"
              label="ชื่อผู้ใช้"
              hide-details="auto"
              :rules="Rules.usernameRules"
            >
            </v-text-field>
          </v-col>
        </v-row>
     
        <!-- password -->
        <v-row dense>
          <v-col cols="12" align="start">
            <v-text-field
            
              v-model="password"
              label="รหัสผ่าน"
              hide-details="auto"
              :rules="Rules.passwordRules"
            ></v-text-field>
          </v-col>
        </v-row>
      </v-card-text>
    
      <v-card-actions>
        <v-btn
        
          block
          rounded
          color="success"
          class="white--text"
          @click="login()"
          >เข้าสู่ระบบ</v-btn
        >
      </v-card-actions>
    </v-card>
  </v-form>
</template>

<script>
export default {
  data() {
    return {
    
      lazy: false,
      username: "",
      password: "",
      Rules: {
        usernameRules: [(v) => !!v || "กรุณาระบุชื่อผู้ใช้งาน"],
        passwordRules: [(v) => !!v || "กรุณาระบุรหัสผ่าน"],
      },
    };
  },
  methods: {
    login() {
      if (this.$refs.form.validate(true)) {
        localStorage.setItem("username", this.username);
        this.$EventBus.$emit("getUsername");
        this.$EventBus.$emit("checklogin");
        this.$router.push({ path: "/" }).catch(() => {});
       
      }
    },
  },
};
</script>

<style></style>
