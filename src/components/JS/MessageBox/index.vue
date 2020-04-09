<template>
  <div class="messageBox">
    <h2 v-text="title"></h2>
    <p v-text="content"></p>
    <div class="btn">
      <div v-text="cancel" @touchstart="handleCancel"></div>
      <div v-text="ok" @touchstart="handleOk"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MessageBox"
};
</script>

<style lang="scss" scoped>
.messageBox {
  width: 200px;
  height: 120px;
  border: 1px solid #666;
  border-radius: 6px;
  background: white;
  box-shadow: 3px 3px 3px 3px #ccc;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -100px;
  margin-top: -60px;
  z-index: 999;
  h2 {
    text-align: center;
    line-height: 40px;
    font-size: 18px;
  }
  p {
    text-align: center;
    line-height: 40px;
  }
  .btn {
    width: 100%;
    display: flex;
    position: absolute;
    bottom: 0;
    border-top: 1px solid #666;
    div {
      flex: 1;
      text-align: center;
      line-height: 30px;
      border-right: 1px solid #666;
      &:last-child {
        border: none;
      }
    }
  }
}
</style>