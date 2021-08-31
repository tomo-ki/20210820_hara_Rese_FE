<template >
  <div id="CommonHeader">
    <header v-if="menuView == 0" class="header__left flex">
      <button @click="menuViewToggle()" class="header__left-btn hover shadow flex-column">
        <span class="header__left-btn--line header__left-btn--line1"></span>
        <span class="header__left-btn--line header__left-btn--line2"></span>
        <span class="header__left-btn--line header__left-btn--line3"></span>
      </button>
      <NuxtLink to="/" ><h1 class="header__left-title">Rese</h1></NuxtLink>
    </header>

<!-- ======================================================================= -->
    <header v-if="menuView == 1" class="header__left flex">
      <button @click="menuViewToggle()" class="header__left-btn hover shadow batsu">
      </button>
    </header>
    <div v-if="menuView == 1" class="menu flex-column">
      <nav v-if="authStatus == 0">
        <li><p @click="toHome()" class="menu__link hover">Home</p></li>
        <li><p @click="logout()" class="menu__link hover">Logout</p></li>
        <li><NuxtLink to="/mypage" class="menu__link hover">Mypage</NuxtLink></li>
      </nav>
      <nav v-if="authStatus == 1">
        <li><p @click="toHome()" class="menu__link hover">Home</p></li>
        <li><NuxtLink to="/register" class="menu__link hover">Registration</NuxtLink></li>
        <li><NuxtLink to="/login" class="menu__link hover">Login</NuxtLink></li>
      </nav>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      menuView: 0,
    };
  },
  computed: {
    authStatus(){
      let status = 1;
      if(this.$auth.user){
        status = 0;
        return status;
      }else{
        status = 1;
        return status;
      }
    }
  },
  methods: {
    menuViewToggle(){
      if(this.menuView == 0){
        this.$emit('menuViewToggle');
        this.menuView = 1;
      }else{
        this.menuView = 0;
        this.$emit('menuViewToggle');
      }
    },
    async logout() {
      try {
        await this.$auth.logout();
        alert('ログアウトしました');
        this.menuViewToggle();
        this.$router.push("/");
      } catch (error) {
        console.log(error);
      }
    },
    toHome(){
      this.menuViewToggle();
      this.$router.push("/");
    },
  },
}
</script>
