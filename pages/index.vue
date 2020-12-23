
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
              @click="userLogin"
            >
              <v-icon left>mdi-login-variant </v-icon>เข้าสู่ระบบ
            </v-btn>
            <p
              class="red--text"
              v-if="status != null"
            >{{ status }}</p>
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
  layout: 'Authen',
  name: 'Login',
  created () {

  },
  data () {
    return {
      login: {
        username: null,
        password: null
      },
      status: ""
    }
  },
  props: {

  },
  methods: {
    async userLogin () {
      try {
        const login = await (this.$axios.$post("authen/login", this.login));
        if (login.user != null) {
          if (login.user.user_role == 'admin' || login.user.user_role == 'trainer') {
            location.href = "/users"
            this.status = "";
            Cookies.set("user", JSON.stringify(login.user));
          } else {
            this.status = "Sorry you can't access!"
          }

          // alert(Cookies.get("user"));
        } else {
          this.status = login.message
        }
      } catch (err) {
        console.log(err);
      }

    }
  },
}
</script>

<style lang="scss" scoped>
</style>

