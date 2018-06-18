<template>
  <div class="row">
    <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <input type="text" placeholder="Search here.." v-on:click="openNav()">
      </div>
    </div>
     <!-- tabs -->
    <div class="row" >
      <div class="col-md-4 col-sm-4 col-xs-4 col-lg-4">
        <div class="btn_sm" v-on:click="tech()" >Technology</div>
      </div>
      <div class="col-md-4 col-sm-4 col-xs-4 col-lg-4">
        <div class="btn_sm" v-on:click="spotrs()" >Sports</div>
      </div>
      <div class="col-md-4 col-sm-4 col-xs-4 col-lg-4">
        <div class="btn_sm" v-on:click="science()" >Science</div>
      </div>
    </div>
     <!-- modal -->
    <div class="row">
      <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
        <div id="myNav" class="overlay">
            <p class="closebtn" v-on:click="closeNav()">&times;</p>
            <input type="text" placeholder="Search here.." v-model="search" class="inpt" v-on:keyup="apifun()" id="text-search">
            <div class="overlay-content">
               <div class="row">
                  <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
                    <div v-for="sec in second" :key="sec.id">
                      <div class="card">
                        <h4>{{ sec.title}}</h4>
                        <img :src="sec.urlToImage" width="400px" height="200px"/>
                        <p class="description"> {{ sec.description}}</p>
                        <a v-bind:href ="sec.url">ReadMore</a>
                      </div>
                    </div>
                  </div>
                </div>
            </div>
        </div>
      </div>
    </div>
     <!-- default page -->
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <div v-for="joke in fst" :key="joke.id">
            <div class="card">
              <h4>{{ joke.title}}</h4>
              <img :src="joke.urlToImage" width="400px" height="200px"/>
              <p class="description"> {{ joke.description}}</p>
              <a v-bind:href ="joke.url">ReadMore</a>
            </div>
          </div>
        </div>
      </div>
       <!-- load more -->
      <div class="row">
        <div class="col-md-12 col-sm-12 col-xs-12 col-lg-12">
          <div v-if="n < m" class="loadmore" v-on:click="dataProcess()" >Load More</div>
        </div>
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
      search: '',
      m: 0,
      n: 0,
      second: [],
      tec: 'yes',
      spo: 'no',
      gen: 'no'
    }
  },
  mounted () {
    this.fetchNews()
    this.newsApi()
  },
  methods: {
    // default page
    fetchNews: async function () {
      console.log('hii')
      try {
        await axios.get('http://newsapi.org/v2/top-headlines?sources=techcrunch&apikey=b790ae775a9d427c823e459337e97de4').then(response => {
          this.jokes = response.data.articles
          this.m = this.jokes.length
          this.dataProcess()
          console.log(this.jokes.length)
        })
      } catch (err) {
        console.log(err)
      }
    },
    // to set 3 news
    dataProcess: function () {
      for (let key = this.count; key < this.jokes.length; key++) {
        this.count = this.count + 1
        if (this.count % 3 === 0) {
          this.fst.push(this.jokes[key])
          this.n = this.fst.length
          break
        } else {
          console.log(this.count)
          this.fst.push(this.jokes[key])
          this.n = this.fst.length
          console.log(this.fst.length)
        }
      }
    },
    // 3 tabs tech sports & science
    newsApi: async function () {
      try {
        if (this.tec === 'yes') {
          await axios.get('http://newsapi.org/v2/top-headlines?sources=techcrunch&apikey=b790ae775a9d427c823e459337e97de4').then(response => {
            this.jokes = response.data.articles
            this.m = this.jokes.length
            this.dataProcess()
            console.log(this.jokes.length)
          })
        } else if (this.spo === 'yes') {
          await axios.get('https://newsapi.org/v2/top-headlines?sources=espn&apiKey=b790ae775a9d427c823e459337e97de4').then(response => {
            this.jokes = response.data.articles
            this.m = this.jokes.length
            this.dataProcess()
            console.log(this.jokes.length)
          })
        } else if (this.gen === 'yes') {
          await axios.get('https://newsapi.org/v2/top-headlines?sources=new-scientist&apiKey=b790ae775a9d427c823e459337e97de4').then(response => {
            this.jokes = response.data.articles
            this.m = this.jokes.length
            this.dataProcess()
            console.log(this.jokes.length)
          })
        }
      } catch (err) {
        console.log(err)
      }
    },
    tech: function () {
      this.tec = 'yes'
      this.spo = 'no'
      this.gen = 'no'
      this.fst = []
      this.newsApi()
    },
    spotrs: function () {
      this.tec = 'no'
      this.spo = 'yes'
      this.gen = 'no'
      this.fst = []
      this.newsApi()
    },
    science: function () {
      this.tec = 'no'
      this.spo = 'no'
      this.gen = 'yes'
      this.fst = []
      this.newsApi()
    },
    // modal function
    apifun: async function () {
      try {
        await axios.get('http://newsapi.org/v2/everything?q=' + this.search + '&apikey=b790ae775a9d427c823e459337e97de4').then(response => {
          this.second = response.data.articles
          for (let key = 0; key < 5; key++) {
            this.fst.push(this.second[key])
          }
          console.log('Lenght of fst', this.second[0].title)
        })
      } catch (err) {
        console.log(err)
      }
    },
    openNav: function () {
      document.getElementById('myNav').style.width = '100%'
      document.getElementById('text-search').value = ''
    },
    closeNav: function () {
      document.getElementById('myNav').style.width = '0%'
      this.second = []
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

h4{
  font-size: 1.4em;
  font-family: 'Amiri', serif;
}

.description{
  margin-top: 10px;
  font-size: 1.2em;
  text-align: justify;
  margin-left: 15px;
  margin-right: 10px;
  font-family: 'Amiri', serif;
}

a:link, a:visited {
  background-color: maroon;
  color: white;
  padding: 14px 25px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  margin-left: 300px;
  font-family: 'Amiri', serif;
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
  font-family: 'Amiri', serif;
}

.btn_sm {
  background-color: Transparent;
  background-repeat:no-repeat;
  border: none;
  cursor:pointer;
  overflow: hidden;
  outline:none;
  font-family: 'Amiri', serif;
  font-size: 20px;
}

.overlay {
  /* height: 100%; */
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  background-color: white;
  /* overflow-y: hidden; */
  transition: 0.5s;
  width: 0%
}

.overlay-content {
  position: relative;
  top: 25%;
  width: 100%;
  text-align: center;
  margin-top: 30px;
}

.overlay .hyper {
  /* padding: 8px; */
  text-decoration: none;
  font-size: 10px;
  color: #818181;
  /* display: block; */
  transition: 0.3s;
}

.overlay a:hover, .overlay a:focus {
  color: #f1f1f1;
}

.overlay .closebtn {
  position: absolute;
  top: 20px;
  right: 45px;
  font-size: 60px;
}

.closebtn {
  cursor: pointer;
}

@media screen and (max-height: 450px) {
  .overlay a {font-size: 20px}
  .overlay .closebtn {
  font-size: 40px;
  top: 15px;
  right: 35px;
  }
}

.inpt {
  color: #0a0a0a;
  background-color: rgba(241, 235, 235, 0.925);
  border-bottom: 1px solid black;
  border: none;
  margin-top: 50px;
}

.searchimg {
  height: 100px;
  width: 100px;
}
</style>
