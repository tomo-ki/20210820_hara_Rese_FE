<template>
  <div id="login">
    <div class="header flex">
      <Header
        @menuViewToggle="menuViewToggle()"
      />
    </div>
    <main v-if="menuView == 0" class="login flex">
      <validation-observer ref="obs" v-slot="ObserverProps" class="login-box shadow">
        <div class="login-box__top">
          <h3 class="login-box__top-text">Login</h3>
        </div>
        <div class="login-box__center">
          <validation-provider v-slot="ProviderProps" rules="required" class="login-box__center-email flex-column">
            <div class="login-box__center-error error">{{ ProviderProps.errors[0] }}</div>
            <div class="login-box__center-formarea flex">
              <label for="Username" class="login-box__center-label login-box__center-email--label">
                <img src="/img/mail.png" alt="" class="login-box__center-img login-box__center-email--img">
              </label>
              <input
                type="email" v-model="email" placeholder="Email"
                id="Email" name="Email" class="login-box__center-form login-box__center-username--form">
            </div>
          </validation-provider>
          <validation-provider v-slot="ProviderProps" rules="required|min:6" class="login-box__center-password flex-column">
            <div class="login-box__center-error error">{{ ProviderProps.errors[0] }}</div>
            <div class="login-box__center-formarea flex">
              <label for="Username" class="login-box__center-label login-box__center-password--label">
                <img src="/img/key.png" alt="" class="login-box__center-img login-box__center-password--img">
              </label>
              <input
                type="password" v-model="password" placeholder="Password"
                id="Password" name="Password" class="login-box__center-form login-box__center-password--form">
            </div>
          </validation-provider>
        </div>
        <div class="login-box__bottom">
          <button
            @click="login()" class="login-box__bottom-button button hover" type="button"
            :disabled="ObserverProps.invalid || !ObserverProps.validated"
          >
            ログイン
          </button>
        </div>
      </validation-observer>
    </main>
  </div>
</template>

<script>
import Header from '../components/CommonHeader'
export default {
  components: {
    Header,
    // Footer,
    // Pagination,
  },
  data(){
    return {
      menuView: 0,
      email: null,
      password: null,
    };
  },
  methods: {
    async login() {
      try {
        await this.$auth.loginWith("laravelJWT", {
          data: {
            email: this.email,
            password: this.password,
          },
        });
        alert('ログインが完了しました')
        this.$router.push("/");
      } catch {
        alert("メールアドレスまたはパスワードが間違っております");
      }
    },
    menuViewToggle(){
      if(this.menuView == 0){
        this.menuView = 1;
      }else{
        this.menuView = 0;
      }
    }
  },
}
</script>
