<template lang="html">
  <div class="mr-root">
    <app-header>
      <p class="title">全部分类</p>
    </app-header>
    <div class="mr-content">
      <ul>
        <li is="game-list" v-for="(g, index) in games" :game="g" :key="index"></li>
      </ul>
    </div>
    <loading v-if="showLoading"></loading>
    <back-top></back-top>
  </div>
</template>

<script>
import Public from '../public'
import GameList from '../components/GameList'
import BackTop from '../components/BackTop'
export default {
  data () {
    return {
      games: [],
      showLoading: true
    }
  },
  components: {
    GameList,
    BackTop
  },
  mixins: [
    Public
  ],
  created () {
    this.$http.get('/douyuapi/RoomApi/game')
    .then(res => {
      this.games = res.data.data
      setTimeout(() => {
        this.showLoading = false
      }, 2000)
    })
    .catch(err => {
      console.log(err)
    })
  }
}
</script>

<style lang="css" scoped>
.mr-content{
  padding: 44px 0 0 0;
}
.mr-content ul{
  display: flex;
  flex-wrap: wrap;
}
</style>
