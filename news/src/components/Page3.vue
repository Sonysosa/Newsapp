<template>
  <div class="row">
    <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <input type="text" placeholder="Search here.." v-model="search" v-on:keyup="fetchNews">
        </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <div v-for="joke in jokes" :key="joke.id">
            <div class="card">
              <h4>{{ joke.title}}</h4>
              <img :src="joke.urlToImage" width="400px" height="200px"/>
              <p class="description"> {{ joke.description}}</p>
              <a v-bind:href ="joke.url">ReadMore</a>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <div class="loadmore" v-on:click="dataProcess()" >Load More</div>
        </div>
      </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      jokes: [],
      fst: [],
      count: 0,
      search: ''
    }
  },
  methods: {
    fetchNews: async function () {
      try {
        await axios.get('http://newsapi.org/v2/everything?q=' + this.search + '&apikey=b790ae775a9d427c823e459337e97de4').then(response => {
          this.jokes = response.data.articles
          console.log(this.jokes)
          // this.dataProcess()
        // console.log(this.jokes.length)
        })
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style>
input{
  height: 10%;
  width: 40%;
  font-size: 2em;
}

.card {
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    width: 38%;
    margin-left: 400px;
    margin-top: 10px;
    padding-top: 8px;
    padding-bottom: 15px;
}

.card:hover {
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

.h4{
  font-size: 1.4em;
}

.description{
  margin-top: 10px;
  font-size: 1.2em;
  text-align: justify;
  margin-left: 15px;
  margin-right: 10px;
}

a:link, a:visited {
    background-color: maroon;
    color: white;
    padding: 14px 25px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    margin-left: 300px;

}

a:hover, a:active {
    background-color: maroon;
}

.loadmore {
  color:#fff;
  background-color: maroon;
  width: 120px;
  height: 38px;
  font-size: 18px;
  border-radius: 2px;
  cursor: pointer;
  transition: 0.3s;
  margin-left: 600px;
  margin-top: 15px;
  padding-top: 5px;
}
</style>
