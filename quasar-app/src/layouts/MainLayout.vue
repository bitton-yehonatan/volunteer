<template>
  <q-layout view="lHh lpR fFf">

    <q-header reveal elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          <q-avatar square size="md">
            <img src="statics/Shakuf-Logo-Main-Transparent-White-Website.png">
          </q-avatar>
          Shakuf Volunteers
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="leftDrawerOpen" side="left" bordered content-class="bg-grey-1">
      <q-card>
        <q-card-section class="items-center">
          <span class="q-ml-sm">Hello {{ loggedInUser.full_name }}</span>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="Logout" color="primary" to="/" @click="onLogout" />
        </q-card-actions>
      </q-card>

      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Menu
        </q-item-label>
        <q-item
          clickable
          v-ripple
          v-for="menuItem in displayMenu"
          v-bind:key="menuItem.link"
          v-bind="menuItem"
          :to="menuItem.link"
          exact>
          <q-item-section avatar>
            <q-icon :name="menuItem.icon" />
          </q-item-section>
          <q-item-section>
            {{ menuItem.title }}
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script>
export default {
  data () {
    return {
      leftDrawerOpen: false,
      // TODO: should we get the menu items from the server according to login permissions (admin / volunteer)
      menuItems: [
        {
          title: 'Admin Volunteers Dashbaord',
          icon: 'people',
          link: '/admin_volunteers_dashboard',
          displayToRole: 'admin'
        },
        {
          title: 'Admin Tasks Dashbaord',
          icon: 'assignment',
          link: '/tasks_dashboard',
          displayToRole: 'admin'
        },
        {
          title: 'Tasks Dashbaord',
          icon: 'assignment_turned_in',
          link: '/tasks_dashboard',
          displayToRole: 'volunteer'
        },
        {
          title: 'About',
          icon: 'help',
          link: '/about',
          displayToRole: 'all'
        }
      ]
    }
  },

  computed: {
    displayMenu () {
      return this.menuItems.filter(menuItem => menuItem.displayToRole === this.userRole || menuItem.displayToRole === 'all')
    },
    loggedInUser () {
      return this.$store.state.user
    },
    userRole () {
      return this.loggedInUser.role
    }
  },

  methods: {
    onLogout () {
      this.$store.commit('user/logout')
    }
  }
}
</script>
