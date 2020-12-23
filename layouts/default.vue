<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
        <v-list-item @click="logout">
          <v-list-item-action>
            <v-icon>mdi-logout</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="'ออกจากระบบ'" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      color="indigo"
      dark
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import Cookies from 'vue-cookie';
export default {
  created () {
    // const login = Cookies.get('user');
    // console.log(login);
    // this.items.forEach(item => {
    //   if (item.role.includes(login.user_role)) { item.show = true; }
    //   else { item.show = false; }
    // });
  },
  data () {
    return {
      clipped: true,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-account-multiple',
          title: 'สมาชิก',
          to: '/users',
          role: ['admin', 'trainer']
        },
        {
          icon: 'mdi-cart',
          title: 'รายการสินค้า',
          to: '/products',
          role: ['admin', 'trainer']
        },
        {
          icon: 'mdi-cart',
          title: 'ประเภทสินค้า',
          to: '/categorys',
          role: ['admin']
        },
        {
          icon: 'mdi-cart',
          title: 'สต๊อกสินค้า',
          to: '/stock',
          role: ['trainer']
        },
        {
          icon: 'mdi-fish',
          title: 'ประเภทปลา',
          to: '/fishs',
          role: ['trainer']
        },
        {
          icon: 'mdi-fish',
          title: 'บ่อเลี้ยงปลา',
          to: '/pond',
          role: ['trainer']
        },
        {
          icon: 'mdi-timeline-clock',
          title: 'ช่วงเวลาการเลี้ยง',
          to: '/timeline',
          role: ['trainer']
        },
        {
          icon: 'mdi-book-open-page-variant',
          title: 'คู่มือคำแนะนำ',
          to: '/guides',
          role: ['trainer']
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'ForFishFarm Manager'
    }
  },
  methods: {
    async logout () {
      location.href = "/";
    }
  }
}
</script>
