<template>
  <div id="main">
    <Header title="喵喵电影"></Header>
    <div id="content">
      <div class="movie_menu">
        <router-link tag="div" to="/movie/city" class="city_name">
          <span v-text="$store.state.city.nm"></span>
          <i class="iconfont icon-lower-triangle"></i>
        </router-link>
        <div class="hot_swtich">
          <router-link tag="div" to="/movie/nowPlaying" class="hot_item">正在热映</router-link>
          <router-link tag="div" to="/movie/commingSoon" class="hot_item">即将上映</router-link>
        </div>
        <router-link tag="div" to="/movie/search" class="search_entry">
          <i class="iconfont icon-sousuo"></i>
        </router-link>
      </div>
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </div>
    <TabBar></TabBar>
  </div>
</template>

<script>
import Header from "@/components/Header";
import TabBar from "@/components/TabBar";
import { messageBox } from "@/components/JS";

export default {
  name: "Movie",
  components: {
    Header,
    TabBar
  },
  mounted() {
    this.axios.get("/api/getLocation").then(res => {
      var status = res.data.status;
      var data = res.data.data;
      var currentCityName = this.$store.state.city.nm;
      var currentCityId = this.$store.state.city.id;
      console.log(currentCityName, data.nm, currentCityId, data.id);
      if (!status && currentCityName != data.nm && currentCityId != data.id) {
        setTimeout(() => {
          messageBox({
            title: "定位",
            content: data.nm,
            cancel: "取消",
            ok: "切换定位",
            handleOk() {
              window.localStorage.setItem("nowNm", data.nm);
              window.localStorage.setItem("nowId", data.id);
              window.location.reload();
            }
          });
        }, 2000);
      }
    });
    // messageBox({
    //   title: "定位",
    //   content: "深圳",
    //   cancel: "取消",
    //   ok: "切换定位",
    //   handleCancel(){
    //     console.log(1)
    //   },
    //   handleOk(){
    //     console.log(2)
    //   }
    // });
  }
};
</script>

<style lang="scss" scoped>
#main {
  height: 100%;
  display: flex;
  flex-direction: column;
}
#content {
  flex: 1;
  overflow: auto;
  margin-bottom: 50px;
  position: relative;
  display: flex;
  flex-direction: column;
}
#content .movie_menu {
  width: 100%;
  height: 45px;
  border-bottom: 1px solid #e6e6e6;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  z-index: 10;
}
.movie_menu .city_name {
  margin-left: 20px;
  height: 100%;
  line-height: 45px;
}
.movie_menu .city_name.router-link-active {
  color: #ef4238;
  border-bottom: 2px #ef4238 solid;
}
.movie_menu .hot_swtich {
  display: flex;
  height: 100%;
  line-height: 45px;
}
.movie_menu .hot_item {
  font-size: 15px;
  color: #666;
  width: 80px;
  text-align: center;
  margin: 0 12px;
  font-weight: 700;
}
.movie_menu .hot_item.router-link-active {
  color: #ef4238;
  border-bottom: 2px #ef4238 solid;
}
.movie_menu .search_entry {
  margin-right: 20px;
  height: 100%;
  line-height: 45px;
}
.movie_menu .search_entry.router-link-active {
  i {
    color: #ef4238;
  }
  border-bottom: 2px #ef4238 solid;
}
.movie_menu .search_entry i {
  font-size: 24px;
  color: #666;
}
</style>