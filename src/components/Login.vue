<template>
  <v-form
    ref="form" :lazy-validation="lazy">
    <v-card width="50%" height="100%">
    <v-card-title>เข้าสู่ระบบ</v-card-title>
    <v-card-text>
      <!-- username -->
      <v-row dense>
        <v-col cols="12" align="start">
          <span>ชื่อผู้ใช้งาน</span>
          <v-text-field v-model="username" rounded outlined dense :rules="Rules.usernameRules">
          </v-text-field>
        </v-col>
      </v-row>

      <!-- password -->
      <v-row dense>
        <v-col cols="12" align="start">
          <span>รหัสผ่าน: </span><br/>
          <v-text-field v-model="password" rounded outlined dense :rules="Rules.passwordRules"></v-text-field>
        </v-col>
      </v-row>
    </v-card-text>
    <v-card-actions>
      <v-btn block rounded color="#178169" class="white--text" @click="login()">เข้าสู่ระบบ</v-btn>
    </v-card-actions>
  </v-card>
  </v-form>
</template>

<script>
export default {
    data(){
      return {
        lazy: false,
        username: '',
        password: '',
        Rules: {
          usernameRules:[
            v => !!v || 'กรุณาระบุชื่อผู้ใช้งาน'
          ],
          passwordRules: [
            v => !!v || '`กรุณาระบุรหัสผ่าน'
          ]
        }
      }
    },
    methods: {
      login () {
        if (this.$refs.LoginForm.validate(true)) {
          localStorage.setItem('username', this.username)
          this.$EventBus.$emit('getUsername')
          this.$EvenBus.$emit('checklogin')
          this.$router.push({path: '/'}).catch(() =>{})
        }
      }
    }
  }
</script>

<style>

</style>