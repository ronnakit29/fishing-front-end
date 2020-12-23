
<template>
  <div>
    <v-row
      align="center"
      justify="center"
    >
      <v-col
        cols="12"
        md="5"
        sm="8"
        lg="3"
      >
        <v-card
          elevation="5"
          shaped
        >
          <v-card-title>
            <h3>Sign In - เข้าสู่ระบบ</h3>

          </v-card-title>
          <v-card-text>
            <div class="mb-3">เข้าสู่ระบบด้วยชื่อผู้ใช้งานและรหัสผ่าน</div>

            <v-text-field
              label="ชื่อผู้ใช้งาน"
              required
              v-model="login.username"
              outlined
            ></v-text-field>
            <v-text-field
              label="รหัสผ่าน"
              required
              v-model="login.password"
              type="password"
              outlined
            ></v-text-field>

            <v-btn
              dark
              color="indigo"
              large
              block
              elevation="0"
            >
              <v-icon left>mdi-login-variant </v-icon>เข้าสู่ระบบ
            </v-btn>
          </v-card-text>
          <v-card-text>
            <v-img src="/download.png">
            </v-img>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import Cookies from 'vue-cookie';
export default {
  middleware: 'authen',
  layout: 'Authen',
  name: 'Login',
  created () {

  },
  data () {
    return {
      login: {
        username: null,
        password: null
      }
    }
  },
  props: {

  },
  methods: {
    async userLogin () {
      const login = await (this.$axios.$post("authen/login", this.login));
      if (login.user != null) {
        window.location.href = "/users"
        Cookies.set("user", login.user);
      } else {

      }
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

