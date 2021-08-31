<template>
  <div id="thanks">
    <div class="header flex">
      <Header
        @menuViewToggle="menuViewToggle()"
      />
    </div>
    <main v-if="menuView == 0" class="thanks">
      <div class="thanks-box shadow flex-column">
        <h3 class="thanks-box__text">会員登録ありがとうございます</h3>
        <button @click="toLogin()" class="thanks-box__btn button hover">ログインする</button>
      </div>
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
    };
  },
  methods: {
    async toLogin() {
      this.$router.push("/login");
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
