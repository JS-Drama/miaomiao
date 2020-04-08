<template>
  <div class="city_body">
    <div class="city_list">
      <div class="city_hot">
        <h2>热门城市</h2>
        <ul class="clearfix">
          <li v-for="hot in hotList" :key="hot.id" v-text="hot.nm"></li>
        </ul>
      </div>
      <div class="city_sort" ref="city_sort">
        <div v-for="city in cityList" :key="city.index">
          <h2 v-text="city.index"></h2>
          <ul>
            <li v-for="list in city.list" v-text="list.nm" :key="list.id"></li>
          </ul>
        </div>
      </div>
    </div>
    <div class="city_index">
      <ul v-for="(city,index) in cityList" :key="city.index">
        <li v-text="city.index" @touchstart="handleToIndex(index)"></li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "city",
  data() {
    return {
      cityList: [], // 城市列表
      hotList: [] // 热门城市
    };
  },
  mounted() {
    this.axios.get("/api/cityList").then(res => {
      const status = res.data.status; //返回状态
      if (!status) {
        // 获取返回城市数据
        const cities = res.data.data.cities;

        var { cityList, hotList } = this.fromatCityList(cities);
        this.cityList = cityList;
        this.hotList = hotList;
      }
    });
  },
  methods: {
    fromatCityList(cities) {
      var cityList = [];
      var hotList = [];

      for (var i = 0; i < cities.length; i++) {
        // 热门城市
        if (cities[i].isHot > 0) {
          hotList.push(cities[i]);
        }
        //
        var firstLetter = cities[i].py.substring(0, 1).toUpperCase();
        if (this.toCom(firstLetter, cityList)) {
          for (var j = 0; j < cityList.length; j++) {
            if (cityList[j].index === firstLetter) {
              cityList[j].list.push({
                nm: cities[i].nm,
                id: cities[i].id,
                isHot: cities[i].isHot,
                py: cities[i].py
              });
            }
          }
        } else {
          cityList.push({
            index: firstLetter,
            list: [
              {
                nm: cities[i].nm,
                id: cities[i].id,
                isHot: cities[i].isHot,
                py: cities[i].py
              }
            ]
          });
        }
      }

      // 首字母排序
      cityList.sort((n1, n2) => {
        return n1.index.charCodeAt(0) - n2.index.charCodeAt(0);
      });

      // 返回数据
      return {
        cityList,
        hotList
      };
    },
    // 判断是否已经添加过同类首字母城市
    toCom(firstLetter, cityList) {
      for (var i = 0; i < cityList.length; i++) {
        if (cityList[i].index == firstLetter) {
          return true;
        }
      }
      return false; //玩死我 注意循环与判断的嵌套关系
    },
    // 跳转
    handleToIndex(index) {
      var h2 = this.$refs.city_sort.getElementsByTagName("h2");
      this.$refs.city_sort.parentNode.scrollTop = h2[index].offsetTop;
    }
  }
};
</script>

<style lang="scss" scoped>
.city_body {
  margin-top: 45px;
  display: flex;
  width: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
}
.city_body .city_list {
  flex: 1;
  overflow: auto;
  background: #fff5f0;
}
.city_body .city_list::-webkit-scrollbar {
  background-color: transparent;
  width: 0;
}
.city_body .city_hot {
  margin-top: 20px;
}
.city_body .city_hot h2 {
  padding-left: 15px;
  line-height: 30px;
  font-size: 14px;
  background: #f0f0f0;
  font-weight: normal;
}
.city_body .city_hot ul li {
  float: left;
  background: #fff;
  width: 29%;
  height: 33px;
  margin-top: 15px;
  margin-left: 3%;
  padding: 0 4px;
  border: 1px solid #e6e6e6;
  border-radius: 3px;
  line-height: 33px;
  text-align: center;
  box-sizing: border-box;
}
.city_body .city_sort div {
  margin-top: 20px;
}
.city_body .city_sort h2 {
  padding-left: 15px;
  line-height: 30px;
  font-size: 14px;
  background: #f0f0f0;
  font-weight: normal;
}
.city_body .city_sort ul {
  padding-left: 10px;
  margin-top: 10px;
}
.city_body .city_sort ul li {
  line-height: 30px;
  line-height: 30px;
}
.city_body .city_index {
  width: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  border-left: 1px #e6e6e6 solid;
}
</style>