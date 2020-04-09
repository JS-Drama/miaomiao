<template>
  <div class="movie_body">
    <Loading v-if="isLoading" />
    <Scroller v-else :handleToScroll="handleToScroll" :handleToTouchEnd="handleToTouchEnd">
      <ul>
        <li v-if="pullDownMsg != ''" v-text="pullDownMsg" class="pull_down"></li>
        <li v-for="list in comingList" :key="list.id">
          <div class="pic_show">
            <img :src="list.img | setWH('128.180')" />
          </div>
          <div class="info_list">
            <h2 v-text="list.nm"></h2>
            <img v-if="list.version" src="@/assets/maxs.png" alt />
            <p>
              <span class="person" v-text="list.wish"></span> 人想看
            </p>
            <p v-text="list.star"></p>
            <p v-text="list.comingTitle + '  上映'"></p>
          </div>
          <div class="btn_pre">预售</div>
        </li>
      </ul>
    </Scroller>
  </div>
</template>

<script>
export default {
  name: "commingSoon",
  data() {
    return {
      comingList: [],
      pullDownMsg: "",
      isLoading: true,
      prevCityId: -1
    };
  },
  activated() {
    this.isLoading = true;
    var cityId = this.$store.state.city.id;
    if (this.prevCityId === cityId) {
      this.isLoading = false;
      return;
    }
    this.axios
      .get("/api/movieComingList", {
        params: {
          cityId
        }
      })
      .then(res => {
        const status = res.data.status;
        if (!status) {
          this.comingList = res.data.data.comingList;
          this.prevCityId = cityId;
          this.isLoading = false;
        } else {
          console.log("服务器繁忙", res.data.msg);
        }
      });
  },
  methods: {
    handleToScroll(pos) {
      if (pos.y > 30) {
        this.pullDownMsg = "正在更新中...";
      }
    },
    handleToTouchEnd(pos) {
      setTimeout(() => {
        if (pos.y > 30) {
          this.pullDownMsg = "更新成功√";
          setTimeout(() => {
            this.pullDownMsg = "";
          }, 2000);
        }
      }, 2000);
    }
  }
};
</script>

<style lang="scss" scoped>
.movie_body {
  flex: 1;
  overflow: auto;
  .pull_down {
    margin-top: 12px;
    display: block;
    text-align: center;
    border-bottom: 0;
    padding-bottom: 10px;
  }
}
.movie_body ul {
  margin: 0 12px;
  overflow: hidden;
}
.movie_body ul li {
  margin-top: 12px;
  display: flex;
  align-items: center;
  border-bottom: 1px #e6e6e6 solid;
  padding-bottom: 10px;
}
.movie_body .pic_show {
  width: 64px;
  height: 90px;
}
.movie_body .pic_show img {
  width: 100%;
}
.movie_body .info_list {
  margin-left: 10px;
  flex: 1;
  position: relative;
}
.movie_body .info_list h2 {
  font-size: 17px;
  line-height: 24px;
  width: 150px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.movie_body .info_list p {
  font-size: 13px;
  color: #666;
  line-height: 22px;
  width: 200px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.movie_body .info_list .grade {
  font-weight: 700;
  color: #faaf00;
  font-size: 15px;
}
.movie_body .info_list img {
  width: 50px;
  position: absolute;
  right: 10px;
  top: 5px;
}
.movie_body .btn_mall,
.movie_body .btn_pre {
  width: 47px;
  height: 27px;
  line-height: 28px;
  text-align: center;
  background-color: #f03d37;
  color: #fff;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
}
.movie_body .btn_pre {
  background-color: #3c9fe6;
}
</style>