<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/" exact>商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import {urlParse} from './common/js/util.js'
  import header from './components/header/header.vue'
  const ERR_OK = 0
  export default {
    name: 'app',
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse()
//            console.log(queryParam)
            return queryParam.id
          })()
        }
      }
    },
    created() {
//      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      this.$http.get('/api/seller').then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.seller = response.data
//          this.seller = Object.assign({}, this.seller, response.data)
          console.log(this.seller)
        }
      })
    },
    components: {
      'v-header': header
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus">
  @import "./common/stylus/mixin.styl";
  .tab
    display: flex
    color: rgb(77, 85, 93)
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 21, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        &.active
          color: rgb(240, 10, 10)
</style>
