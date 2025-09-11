<template>
  <!-- พื้นหลังร้านหนังสือมินิมอล -->
  <v-app class="mini-bookstore-bg">
    <!-- เมนูด้านข้าง: สีขาว ขอบมน ไอคอนหนังสือ -->
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      class="mini-drawer"
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
            <v-icon color="deep-purple accent-2">{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title class="mini-menu-title" v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <!-- แถบด้านบน: สีชมพู-ม่วง ขอบมน -->
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      class="mini-app-bar"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="fixed = !fixed"
      >
        <v-icon>mdi-minus</v-icon>
      </v-btn>
      <v-toolbar-title class="mini-title" v-text="title" />
      <v-spacer />
      <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>
    <v-content>
      <v-container class="mini-content">
        <nuxt />
      </v-container>
    </v-content>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
      class="mini-drawer"
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :fixed="fixed"
      app
      class="mini-footer"
    >
      <span>&copy; {{ new Date().getFullYear() }} ร้านหนังสือ MiniBook</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-email',
          title: 'Contact',
          to: '/context'
        },
         {
          icon: 'mdi-login',
          title: 'Login',
          to: '/loing'
        },
         
          {
          icon: 'mdi-book-open-page-variant',
          title: 'book',
          to: '/book_select'
        },
         {
          icon: 'mdi-shield-account',
          title: 'Admin Login',
          to: '/Ln-Am1'
        },
      
        {
          icon: 'mdi-shield-account',
          title: 'Admin Dashboard',
          to: 'AdminDashboard'
        },
      
          
      
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'MiniBook'
    }
  }
}
</script>

<style scoped>
.mini-bookstore-bg {
  background: linear-gradient(120deg, #fff6fa 0%, #ffe3f6 100%);
  min-height: 100vh;
  color: #111 !important;
}
.mini-drawer {
  background: #fff !important;
  border-radius: 0 24px 24px 0;
  box-shadow: 0 4px 24px 0 #fbbf24aa;
}
.mini-menu-title {
  color: #7c3aed !important;
  font-weight: bold;
  letter-spacing: 0.5px;
}
.mini-app-bar {
  background: linear-gradient(90deg, #7548ff 10%, #f472b6 100%) !important;
  border-radius: 0 0 24px 24px;
  box-shadow: 0 2px 12px #fbbf24aa;
}
.mini-title {
  color: #ff8bc7 !important;
  font-weight: bold;
  letter-spacing: 1px;
}
.mini-content {
  background: transparent;
  min-height: 80vh;
}
.mini-footer {
  background: #fffbe7 !important;
  color: #7c3aed !important;
  font-weight: bold;
  border-radius: 24px 24px 0 0;
  box-shadow: 0 -2px 12px #fbbf24aa;
}
</style>
