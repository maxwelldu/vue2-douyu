<template lang="html">
  <div class="mr-root">
    <back>
      <p class="title">{{ $route.params.gameName }}</p>
    </back>
    <div class="mr-content">
      <div class="room-info" v-for="(r, index) in rooms" :key="index">
        <img v-lazy="r.room_src" alt="">
        <div class="room-title">
          <div class="room-name"> {{ r.room_name | message }} </div>
          <div class="nickname">
            <i class="icon-user"></i>
            <span>{{ r.nickname }}</span>
          </div>
          <div class="online">
            <i class="icon-group"></i>
            <span>{{ r.online | number }}</span>
          </div>
        </div>
      </div>
      <div class="load-more" v-if="!showLoad">
        <span @click="loadMore">加载更多</span>
      </div>
    </div>
    <back-top></back-top>
    <loading v-if="showLoad"></loading>
    <p v-if="error">网络请求失败，请稍后再试...</p>
  </div>
</template>

<script>
import Back from '../components/Back'
import BackTop from '../components/BackTop'
import Loading from '../components/Loading'
export default {
  data () {
    return {
      rooms: [],
      error: false,
      showLoad: true,
      page: 0
    }
  },
  components: {
    Back,
    BackTop,
    Loading
  },
  created () {
    this.getInfo(this.page)
  },
  methods: {
    getInfo (page) {
      this.$http.get(`/douyuapi/RoomApi/live/${this.$route.params.gameId}?offset=${page}&limit=20`)
      .then(res => {
        this.error = false
        this.rooms = this.rooms.concat(res.data.data)
        setTimeout(() => {
          this.showLoad = false
        }, 2000)
      })
      .catch(() => {
        this.error = true
        this.showLoad = false
      })
    },
    loadMore () {
      this.page++
      this.getInfo(this.page)
    }
  }
}
</script>

<style lang="css" scoped>
.room-info {
  margin-top: 20px;
  overflow: hidden;
}

.room-info img {
  float: left;
  width: 4rem;
}

.room-info .room-title {
  float: left;
  margin-left: 20px;
}
</style>
