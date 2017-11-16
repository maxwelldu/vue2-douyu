<template lang="html">
  <div class="back-top" v-if="scroll">
    <i @click="backTop" class="icon-arrow-up"></i>
  </div>
</template>

<script>
export default {
  data () {
    return {
      scroll: false
    }
  },
  methods: {
    backTop () {
      let top = document.documentElement.scrollTop || document.body.scrollTop
      let speed = 50
      let timer = setInterval(() => {
        document.body.scrollTop = document.documentElement.scrollTop = top -= speed
        if (top <= 0) {
          top = 0
          clearInterval(timer)
        }
      }, 30)
    }
  },
  mounted () {
    window.onscroll = () => {
      let top = document.documentElement.scrollTop || document.body.scrollTop
      this.scroll = top > 300
    }
  }
}
</script>

<style lang="css">
.back-top {
  position: fixed;
  right: 20px;
  bottom: 40px;
}
.back-top i {
  font-size: 20px;
  display: inline-block;
  padding: 10px;
  border: 1px solid #ccc;
  color: #ccc;
  border-radius: 50%;
  background: #fff;
}
</style>
