<template>
<div class="container">
<Header title="Hilfsbedarf"/>
<HelpReqList :helps="pollResponse"/>

</div>

</template>

<script>
import vue, { computed } from 'vue'
import Header from './components/Header'
import HelpReqList from './components/HelpReqList'

export default {
  name: 'App',
  components: {
    Header,
    HelpReqList
  },
  data() {
    return{
      pollResponse: { data: [] }
      }
  },

  methods: {
     async withPolling() {
      const endpoint = 'http://localhost:3000/help'
      setInterval(async () => {
        const response = await fetch(endpoint)
        const json = await response.json()
        let formated =this.formatTime(json)
        this.pollResponse = formated
        
      }, 1000)
  },
       formatTime(requests) {
        requests.forEach((request)=>{
        let date= new Date(request.requestTimestamp)
        request.requestTimestamp=date.toLocaleString()
        })
        return requests
            
   
     }
  },

  created(){
    console.log("intial ", this.pollResponse)
    this.withPolling()
  }
}

</script>

<style>@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
