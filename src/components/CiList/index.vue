<template>
  <div class="cinema_body">
    <ul>
      <li v-for="list in cinemasList" :key="list.id">
        <div class="cinema_name">
          <span v-text="list.nm"></span>
          <span class="q">
            <span class="price" v-text="list.sellPrice"></span> 元起
          </span>
        </div>
        <div class="address">
          <span v-text="list.addr"></span>
          <span v-text="list.distance"></span>
        </div>
        <div class="card">
          <div
            v-for="(num,key) in list.tag"
            v-if="num === 1"
            :key="key"
            :class="key | calssCard"
          >{{ key | formatCard }}</div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "ciList",
  data() {
    return {
      cinemasList: [] // 影院列表
    };
  },
  mounted() {
    this.axios
      .get("/api/cinemaList", {
        params: {
          cityId: 10
        }
      })
      .then(res => {
        console.log(res);
        var status = res.data.status;
        if (!status) {
          this.cinemasList = res.data.data.cinemas;
        }
      });
  },
  filters: {
    formatCard(key) {
      var card = [
        { key: "allowRefund", value: "改签" },
        { key: "endorse", value: "退" },
        { key: "sell", value: "折扣卡" },
        { key: "snack", value: "小吃" }
      ];
      for (var i = 0; i < card.length; i++) {
        if (card[i].key === key) {
          return card[i].value;
        }
      }
      return "";
    },
    calssCard(key) {
      var card = [
        { key: "allowRefund", value: "bl" },
        { key: "endorse", value: "bl" },
        { key: "sell", value: "or" },
        { key: "snack", value: "or" }
      ];
      for (var i = 0; i < card.length; i++) {
        if (card[i].key === key) {
          return card[i].value;
        }
      }
      return "";
    }
  }
};
</script>

<style lang="scss" scoped>
.cinema_body {
  flex: 1;
  overflow: auto;
  .cinema_name {
    display: flex;
    span:nth-of-type(1) {
      flex: 1;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      padding-right: 10px;
    }
  }
}
.cinema_body ul {
  padding: 20px;
}
.cinema_body li {
  border-bottom: 1px solid #e6e6e6;
  margin-bottom: 20px;
}
.cinema_body div {
  margin-bottom: 10px;
}
.cinema_body .q {
  font-size: 11px;
  color: #f03d37;
}
.cinema_body .price {
  font-size: 18px;
}
.cinema_body .address {
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #666;
  span:nth-of-type(1) {
    flex: 1;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    padding-right: 10px;
  }
}
.cinema_body .card {
  display: flex;
}
.cinema_body .card div {
  padding: 0 3px;
  height: 15px;
  line-height: 15px;
  border-radius: 2px;
  color: #f90;
  border: 1px solid #f90;
  font-size: 13px;
  margin-right: 5px;
}
.cinema_body .card div.or {
  color: #f90;
  border: 1px solid #f90;
}
.cinema_body .card div.bl {
  color: #589daf;
  border: 1px solid #589daf;
}
</style>