<template>
  <div id="register">
    <div class="header flex">
      <Header
        @menuViewToggle="menuViewToggle()"
      />
    </div>
    <main v-if="menuView == 0" class="registration flex">
      <validation-observer ref="obs" v-slot="ObserverProps" class="registration-box shadow">
        <div class="registration-box__top">
          <h3 class="registration-box__top-text">Registration</h3>
        </div>
        <div class="registration-box__center">
          <validation-provider v-slot="ProviderProps" rules="required" class="registration-box__center-username flex-column">
            <div class="registration-box__center-error error">{{ ProviderProps.errors[0] }}</div>
            <div class="registration-box__center-formarea flex">
              <label for="Username" class="registration-box__center-label registration-box__center-username--label">
                <img src="/img/username.png" alt="" class="registration-box__center-img registration-box__center-username--img">
              </label>
              <input
                type="text" v-model="userName" placeholder="Username"
                id="Username" name="Username" class="registration-box__center-form registration-box__center-username--form">
            </div>
          </validation-provider>
          <validation-provider v-slot="ProviderProps" rules="required" class="registration-box__center-email flex-column">
            <div class="registration-box__center-error error">{{ ProviderProps.errors[0] }}</div>
            <div class="registration-box__center-formarea flex">
              <label for="Username" class="registration-box__center-label registration-box__center-email--label">
                <img src="/img/mail.png" alt="" class="registration-box__center-img registration-box__center-email--img">
              </label>
              <input
                type="email" v-model="email" placeholder="Email"
                id="Email" name="Email" class="registration-box__center-form registration-box__center-username--form">
            </div>
          </validation-provider>
          <validation-provider v-slot="ProviderProps" rules="required|min:6" class="registration-box__center-password flex-column">
            <div class="registration-box__center-error error">{{ ProviderProps.errors[0] }}</div>
            <div class="registration-box__center-formarea flex">
              <label for="Username" class="registration-box__center-label registration-box__center-password--label">
                <img src="/img/key.png" alt="" class="registration-box__center-img registration-box__center-password--img">
              </label>
              <input
                type="password" v-model="password" placeholder="Password"
                id="Password" name="Password" class="registration-box__center-form registration-box__center-password--form">
            </div>
          </validation-provider>
        </div>
        <div class="registration-box__bottom">
          <button
            @click="register" class="registration-box__bottom-button button hover" type="button"
            :disabled="ObserverProps.invalid || !ObserverProps.validated"
          >
            登録
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
      userName: null,
      email: null,
      password: null,
    };
  },
  methods: {
    async register() {
      try {
        await this.$axios.post("http://localhost:8000/api/auth/register", {
          name: this.userName,
          email: this.email,
          password: this.password,
        });
        alert('新規会員登録が完了しました');
        this.$router.push("/thanks");
      } catch {
        alert("メールアドレスがすでに登録されています");
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
