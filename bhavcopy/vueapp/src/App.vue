<template>
  <div class="container main">
    <div class="search">
      <Searchbar @get-details="fetchStockDetails"/>
    </div>
    <div class="row download">
        <CSVDownloader :stockDetails="stockDetails"/>
      </div>
      <p></p>
      <Stockdetails :stockDetails="stockDetails"/>
  </div>
</template>

<script>
import Searchbar from './components/searchbar.vue'
import Stockdetails from './components/stockdetails'
import CSVDownloader from './components/csvdownload'
// import StockCard from './components/stockdetailscard'
// import Cookies from 'js-cookie'

export default {
  name: 'App',
  components: {
    Searchbar,
    Stockdetails,
    CSVDownloader
  },

  data(){
    return{
      stockDetails:[]
    }
  },
  methods:{
    getCookie(name) {
    let cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        const cookies = document.cookie.split(';');
        for (let i = 0; i < cookies.length; i++) {
            const cookie = cookies[i].trim();
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
},
    async fetchStockDetails(data){

      const csrftoken = this.getCookie("csrftoken")
      const response = await fetch('http://ec2-3-223-158-5.compute-1.amazonaws.com/api/stocks/details',{
                method:'POST',
                headers: {'Content-Type': 'application/json','X-CSRFToken':csrftoken},
                body:JSON.stringify(data)
      })
      try{        
          const res = await response.json()
          this.stockDetails=res
              
      }catch(err){
          console.log(err)
      }
      }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; 
}
.search{
  height:50vh;
}
</style>
