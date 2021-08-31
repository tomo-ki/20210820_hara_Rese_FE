<template>
  <div id="detail">
    <div class="header-left flex-column">
      <Header
        @menuViewToggle="menuViewToggle()"
      />
    </div>
    <main v-if="menuView == 0" class="detail-left">
      <div class="detail-left__top flex">
        <button @click="toHome()" class="detail-left__top-button shadow hover">&lt;</button>
        <h2 class="detail-left__top-shopname">{{detailShop.name}}</h2>
      </div>
      <div class="detail-left__img">
        <img :src="detailShop.image_path" alt="">
      </div>
      <div class="detail-left__tag flex">
        <p class="detail-left__tag-prefecture">#{{detailShop.prefecture}}</p>
        <p class="detail-left__tag-genre">#{{detailShop.genre}}</p>
      </div>
      <div class="detail-left__desc">
        <p>{{detailShop.description}}</p>
      </div>
    </main>
    <aside v-if="menuView == 0" class="detail-right shadow">
      <div class="detail-right__main">
        <h3 class="detail-right__main-title">予約</h3>
        <div class="detail-right__main-formarea flex-column">
          <input
            type="date" v-model="reservation.date"
            class="detail-right__main-form detail-right__main-form--date"
          >
          <input
            type="time" v-model="reservation.time"
            class="detail-right__main-form detail-right__main-form--time"
          >
          <input
            type="number" min="0" v-model="reservation.number"
            class="detail-right__main-form detail-right__main-form--number"
          >
        </div>
        <table class="detail-right__table">
          <tr class="detail-right__table-tr">
            <th class="detail-right__table-th">Shop</th>
            <td class="detail-right__table-td">{{detailShop.name}}</td>
          </tr>
          <tr class="detail-right__table-tr">
            <th class="detail-right__table-th">Date</th>
            <td class="detail-right__table-td">{{reservation.date}}</td>
          </tr>
          <tr class="detail-right__table-tr">
            <th class="detail-right__table-th">Time</th>
            <td class="detail-right__table-td">{{reservation.time}}</td>
          </tr>
          <tr class="detail-right__table-tr">
            <th class="detail-right__table-th">Number</th>
            <td class="detail-right__table-td">{{reservation.number}}人</td>
          </tr>
        </table>
      </div>
      <button @click="reserve()" class="detail-right__bottom button hover">予約する</button>
    </aside>
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
      detailShop: "",
      menuView: 0,
      reservationDate: null,
      reservationTime: null,
      reservationNumber: 0,
      reservation:{
        date: null,
        time: null,
        number: 0,
      },
    };
  },
  methods: {
    async getShop(){
      const resData = await this.$axios.get(
        "http://127.0.0.1:8000/api/v1/shop/"
      );
      this.shopLists = resData.data.data;
      for(const shop of this.shopLists){
        if(shop.id == this.$route.query.shop_id){
          this.detailShop = shop;
        }
      };
    },
    menuViewToggle(){
      if(this.menuView == 0){
        this.menuView = 1;
      }else{
        this.menuView = 0;
      }
    },
    toHome(){
      this.$router.push('/');
    }
  },
  created() {
    this.getShop();
  },
}
</script>

