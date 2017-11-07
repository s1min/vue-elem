<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link tag="div" to="/goods">
          <a>商品</a>
        </router-link>
      </div>
      <div class="tab-item">
        <router-link tag="div" to="/ratings">
          <a>评论</a>
        </router-link>
      </div>
      <div class="tab-item">
        <router-link tag="div" to="/seller">
          <a>商家</a>
        </router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
  import {urlParse} from '@/common/js/util'
  import header from '@/components/header/header'

  const ERR_OK = 0;  // 定义0为正确

  export default {
    name: 'app',
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse();
            return queryParam.id;
          })()
        }
      }
    },
    created() {
      this.$http.get('/api/seller' + '?id=' + this.seller.id).then((response) => {
        response = response.body;
          if (response.errno === ERR_OK) {
              this.seller = response.data;
          }
      }, (error) => {
          console.log(error);
      })
    },
    components: {
      'v-header': header
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      border-bottom: 1px solid rgba(7, 77, 27, 0.1)
      .tab-item
        flex: 1
        text-align: center
        .router-link-active > a
          color: rgb(240, 20, 20)
        div
          &>a
            display: block
            text-decoration: none
            font-size: 14px
            color: rgb(77, 85, 93)

</style>
