<template>
  <v-app>
    <v-app-bar dense app>
      <v-img
          alt="Logo"
          class="shrink mr-2"
          contain
          src="../assets/bluewhale.png"
          max-height="48"
        />
      <v-btn plain to="/home">
        <v-icon>mdi-home</v-icon>
        首页
      </v-btn>
      <v-btn plain to="/articles">
        <v-icon>mdi-book-open-variant</v-icon>
        文章
      </v-btn>
      <v-btn plain to="/subscribes">
        <v-icon>mdi-account-multiple</v-icon>
        领域
      </v-btn>
      <v-btn plain to="/qa">
        <v-icon>mdi-chat-processing</v-icon>
        交流
      </v-btn>
      <v-btn plain to="/rank">
        <v-icon>mdi-clipboard-list</v-icon>
        榜单
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn icon to="/articles/editor/add" v-show="isAuthenticated">
        <v-icon>mdi-book-plus</v-icon>
      </v-btn>
      <v-menu
        open-on-click
        left bottom
        offset-y>
          <template v-slot:activator="{ on, attrs }">
          <!-- <v-btn icon
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-account</v-icon>
          </v-btn> -->
          <v-avatar
            v-bind="attrs"
            v-on="on">
            <img v-if="isAuthenticated"
              :src="gravatar"
            >
            <v-icon v-else>
              mdi-account
            </v-icon>
          </v-avatar>
        </template>
        <v-list dense nav>
          <v-list-item to="/login" v-if="!isAuthenticated">
            <v-list-item-icon>
              <v-icon>mdi-login</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              登录
            </v-list-item-content>
          </v-list-item>
          <v-list-item v-else @click="signOutUser">
            <v-list-item-icon>
              <v-icon>mdi-logout</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              注销登录
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>
    <v-main>
      <Snackbar/>
      <router-view></router-view>
    </v-main>
    <v-footer app>
      <v-col
        class="text-center"
        cols="12"
      >
        Datawhale - Bluewhale
        <v-icon color="pink" small>mdi-heart</v-icon>
        {{ new Date().getFullYear() }}
      </v-col>
    </v-footer>
  </v-app>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import Snackbar from '@/components/Snackbar';

export default {
  name: 'Layout',
  components: {
    Snackbar,
  },
  computed: {
    ...mapGetters({
      isAuthenticated: 'user/isAuthenticated',
      gravatar: 'user/gravatar',
    }),
  },
  methods: {
    ...mapActions({
      setUserInfo: 'user/setUserInfo',
      showSnack: 'snackbar/showSnack',
    }),
    signOutUser() {
      this.$store.dispatch('user/signOutUser', this);
    },
  },
  created() {
    this.$store.dispatch('user/fetchUserInfo', this);
  },
};
</script>
