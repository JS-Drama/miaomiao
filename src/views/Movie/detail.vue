<template>
  <div id="detailContrainer" class="slide-enter-active">
    <Header title="影片详情">
      <i @touchstart="handleToBack()" class="iconfont icon-right"></i>
    </Header>
    <Loading v-if="isLoading" />
    <div v-else id="content" class="contentDetail">
      <div class="detail_list">
        <div class="detail_list_bg"></div>
        <div class="detail_list_filter"></div>
        <div class="detail_list_content">
          <div class="detail_list_img">
            <!-- 这里使用过滤报错 replace方法不存在 -->
            <img :src="detailMovie.img | setWH('128.180')" :alt="detailMovie.nm" />
            <!-- <img :src="detailMovie.img" :alt="detailMovie.nm" /> -->
          </div>
          <div class="detail_list_info">
            <h2 v-text="detailMovie.nm"></h2>
            <p v-text="detailMovie.enm"></p>
            <p v-text="detailMovie.sc"></p>
            <p v-text="detailMovie.cat"></p>
            <p v-text="detailMovie.src + ' / ' + detailMovie.dur + '分钟'"></p>
            <p v-text="detailMovie.pubDesc"></p>
          </div>
        </div>
      </div>
      <div class="detail_intro">
        <p v-text="detailMovie.dra"></p>
      </div>
      <div class="detail_player swiper-container" ref="detail_player">
        <ul class="swiper-wrapper">
          <li v-for="(item,index) in detailMovie.photos" class="swiper-slide" :key="index">
            <div>
              <img :src="item | setWH('128.180')" alt />
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header";
export default {
  name: "Detail",
  data() {
    return {
      detailMovie: {},
      isLoading: true
    };
  },
  components: {
    Header
  },
  props: ["movieId"],
  mounted() {
    // if (!this.movieId) {
    //   this.$router.back();
    // }
    this.isLoading = true;
    this.axios
      .get("/api/detailmovie", {
        params: {
          movieId: this.movieId
        }
      })
      .then(res => {
        var status = res.data.status;
        if (!status) {
          this.detailMovie = res.data.data.detailMovie;
          // this.detailMovie.img = this.detailMovie.img.replace(
          //   /w\.h/,
          //   "128.180"
          // );
          this.isLoading = false;
          this.$nextTick(() => {
            new Swiper(this.$refs.detail_player, {
              slidesPerView: "auto",
              freeMode: true,
              freeModeSticky: true
            });
          });
        }
      });
  },
  methods: {
    handleToBack() {
      this.$router.back();
    }
  }
};
</script>

<style lang="scss" scoped>
#detailContrainer {
  position: absolute;
  left: 0;
  top: 0;
  z-index: 100;
  background: white;
  min-height: 100%;
  width: 100%;
}
.slide-enter-active {
  animation: 0.3s slideMove;
}
@keyframes slideMove {
  0% {
    transform: translateX(100%);
  }
  100% {
    transform: translateX(0);
  }
}
.contentDetail {
  display: block;
  margin-bottom: 0;
}
#content .detail_list {
  height: 200px;
  width: 100%;
  position: relative;
  overflow: hidden;
}
.detail_list .detail_list_bg {
  width: 100%;
  height: 100%;
  background: url(/images/movie_1.jpg) 0 40%;
  filter: blur(20px);
  background-size: cover;
  position: absolute;
  left: 0;
  top: 0;
}
.detail_list .detail_list_filter {
  width: 100%;
  height: 100%;
  position: absolute;
  background-color: #40454d;
  opacity: 0.55;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 1;
}
.detail_list .detail_list_content {
  display: flex;
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 2;
}
.detail_list .detail_list_img {
  width: 108px;
  height: 150px;
  border: solid 1px #f0f2f3;
  margin: 20px;
}
.detail_list .detail_list_img img {
  width: 100%;
  height: 100%;
}
.detail_list .detail_list_info {
  margin-top: 20px;
}
.detail_list .detail_list_info h2 {
  font-size: 20px;
  color: white;
  font-weight: normal;
  line-height: 40px;
}
.detail_list .detail_list_info p {
  color: white;
  line-height: 20px;
  font-size: 14px;
  color: #ccc;
}
.detail_intro {
  padding: 10px;
}
.detail_player {
  margin: 20px;
}
.detail_player .swiper-slide {
  width: 70px;
  margin-right: 20px;
  text-align: center;
  font-size: 14px;
}
.detail_player .swiper-slide img {
  width: 100%;
  margin-bottom: 5px;
}
.detail_player .swiper-slide p:nth-of-type(2) {
  color: #999;
}
</style>