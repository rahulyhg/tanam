<template>
  <v-container fluid fill-height class="primary">
    <v-layout column align-center justify-center>
      <v-flex xs12 d-flex align-center>
        <v-card flat color="transparent" class="text-xs-center">
          <div class="head-wrapper mb-4">
            <div class="logo-wrapper pa-1 mb-2">
              <img src="@/assets/images/tanam-circle.png">
            </div>
            <h2 class="white--text head-title">Welcome to Tanam
              <span>CMS</span>
            </h2>
          </div>
          <div id="firebaseui-container" class="text-xs-center" />
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex';
import { store } from '@/store';
import { AUTO_LOGIN, CURRENT_USER, CONTENT_TYPE_GET } from '@/store/types';
import firebase from 'firebase/app';
import { firebaseUI } from '@/utils/firebase';

const uiConfig = {
  signInOptions: [firebase.auth.GoogleAuthProvider.PROVIDER_ID],
  tosUrl: '/#tos',
  signInFlow: 'popup',
  callbacks: {
    signInSuccessWithAuthResult: function(authResult) {
      store.dispatch(AUTO_LOGIN, authResult.user);
      store.dispatch(CONTENT_TYPE_GET);
      return false;
    }
  }
};

export default {
  computed: {
    ...mapGetters({
      currentUser: CURRENT_USER
    })
  },
  watch: {
    currentUser(user) {
      if (user !== null && user !== undefined) {
        this.$router.push('/authenticate');
      }
    }
  },
  mounted() {
    firebaseUI.start('#firebaseui-container', uiConfig);
  }
};
</script>

<style src="firebaseui/dist/firebaseui.css">
</style>

<style lang="scss" scoped>
.head-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  .logo-wrapper {
    display: flex;
    align-items: center;
    border: 2px solid #fff;
    border-radius: 100%;

    img {
      max-width: 100px;
    }
  }

  .head-title {
    font-weight: 400;

    span {
      font-weight: 700;
    }
  }
}
</style>
