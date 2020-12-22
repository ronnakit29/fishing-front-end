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
export default {
  data () {
    return {
      clipped: true,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-account-multiple',
          title: 'สมาชิก',
          to: '/users'
        },
        {
          icon: 'mdi-cart',
          title: 'รายการสินค้า',
          to: '/products'
        },
        {
          icon: 'mdi-cart',
          title: 'ประเภทสินค้า',
          to: '/categorys'
        },
        {
          icon: 'mdi-cart',
          title: 'สต๊อกสินค้า',
          to: '/stock'
        },
        {
          icon: 'mdi-fish',
          title: 'ประเภทปลา',
          to: '/fishs'
        },
        {
          icon: 'mdi-timeline-clock',
          title: 'ช่วงเวลาการเลี้ยง',
          to: '/timeline'
        },
        {
          icon: 'mdi-book-open-page-variant',
          title: 'คู่มือคำแนะนำ',
          to: '/guides'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'ForFishFarm Manager'
    }
  }
}
</script>
