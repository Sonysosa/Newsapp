<template>
  <div class="row">
    <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <img src="../assets/news.jpg">
        </div>
      </div>
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <div id="progress-container">
            <div id="progress"  :style="{width: progressWidth + '%'}"></div>
          </div>
          <div v-if="progressWidth == 100" class="button" v-on:click=redirection>
            Read now
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
#progress-container {
  width: 18%;
  border: 1px solid #fff;
  height: 10px;
  margin-left: 540px;
}

#progress {
  height: 50%;
  width: .1%;
  background-color: maroon;
}

.button {
  margin-top:50px;
  color:#fff;
  background-color: maroon;
  width: 120px;
  height: 35px;
  font-size: 18px;
  padding-top: 6px;
  border-radius: 2px;
  cursor: pointer;
  transition: 0.3s;
  margin-left: 600px;
  font-family: 'Amiri', serif;
}
</style>

<script>
export default {
  name: 'Page1',
  data () {
    return {
      progressWidth: 0, // initial width = 0
      duration: 5 * 1000, // concert duration, ms
      start: (new Date()).valueOf()
    }
  },
  computed: {
    end: function () { // when event finishes
      return this.start + this.duration
    }
  },
  methods: {
    startEvent: function (start, end) {
      const int = setInterval(() => { // calculate width periodically
        const now = (new Date()).valueOf()
        if (now >= end) { // if finished
          this.progressWidth = 100
          clearInterval(int)
          return
        };
        this.progressWidth = ((now - start) / (end - start)) * 100
        return this.progressWidth
      }, 10)
    },
    redirection: function () {
      this.$router.push({path: '/Page2'})
    }
  },
  mounted () {
    this.startEvent(this.start, this.end) // start event
  }
}
</script>
