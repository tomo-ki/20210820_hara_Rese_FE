<template>
  <div id="index">
    <div class="header flex">
      <Header
        @menuViewToggle="menuViewToggle()"
      />
      <div v-if="menuView == 0" class="header__right">
        <div class="flex">
          <input type="text">
          <input type="text">
          <input type="text">
        </div>
      </div>
    </div>
    <main v-if="menuView == 0">
      <ul class="shoplist flex">
        <li v-for="shop in shopLists" :key="shop.id" class="shoplist__card shadow flex-column">
          <div class="shoplist__card-img">
            <img :src="shop.image_path" alt="画像">
          </div>
          <div class="shoplist__card-textarea">
            <h2 class="shoplist__card-textarea--shopname">{{shop.name}}</h2>
            <div class="shoplist__card-textarea--tag flex">
              <p class="shoplist__card-textarea--tagarea">#{{shop.prefecture}}</p>
              <p class="shoplist__card-textarea--taggenre">#{{shop.genre}}</p>
            </div>
            <div class="shoplist__card-textarea--btn flex">
              <button @click="toDetailPage(shop.id)" class="shoplist__card-textarea--btn1 button hover">詳しく見る</button>
              <button class="shoplist__card-textarea--btn2 button">
                <img src="/img/heart.png" alt="" class="hover">
              </button>
            </div>
          </div>
        </li>
      </ul>
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
      shopLists: [],
      menuView: 0,
    };
  },
  methods: {
    async getShop(){
      const resData = await this.$axios.get(
        "http://127.0.0.1:8000/api/v1/shop/"
      );
      this.shopLists = resData.data.data;
    },
    menuViewToggle(){
      if(this.menuView == 0){
        this.menuView = 1;
      }else{
        this.menuView = 0;
      }
    },
    toDetailPage( shopId ){
      this.$router.push({ path: '/detail' , query :{ shop_id: shopId }});
    },
  },
  created() {
    this.getShop();
  },
}
</script>
