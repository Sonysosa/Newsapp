<template>
  <input type="text" class="form__field" v-model="keywords" v-on:keyup="queryForKeywords">
</template>
<script>
export default {
  data () {
    return {
      search : ''
    }
  },
  methods:{
    queryForKeywords: function(event) {
      console.log("keywords value: " + this.keywords);
      
    }
  }
}
</script>
