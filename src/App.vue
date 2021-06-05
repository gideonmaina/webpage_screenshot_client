<template>
  <div class="app">
    <header>
      <h1>Enter URL to take a screenshot</h1></header>
    <main>
      <form @submit.prevent="takeScreenshot">
        <input type="url" name="url" id="url" v-model="url">
        <input type="submit" value="Take Screenshot">
      </form>
      <a v-if="screenshotUrl !=''" :href="screenshotUrl" target="_blank" download="">
         <img :src="screenshotUrl" alt="" srcset="">
      </a>
    </main>
  </div>
</template>

<script> 

import { ref } from "vue";

export default {
  name: 'App',

  // data(){
  //   return{
  //     url:"",
  //     screenshotUrl:""

  //   }
  // },
  // methods:{
  //   async takeScreenshot(){
  //     console.log("taking screenshot")
  //     const res=await fetch("http://localhost:5000/screenshot",{
  //       method:"POST",
  //       headers:"Content-Type:application/json",
  //       body:JSON.stringify({url:this.url.value})
  //     }).then(data=>data.json())
  //     console.log(res)
  //   }
  // },

  setup(){
    const url=ref("http://");
    const screenshotUrl=ref();
    
    const takeScreenshot=async()=>{
      const res=await fetch("http://localhost:5000/screenshot",{
        method:"POST",
        headers:{'Content-Type':'application/json'},
        body:JSON.stringify({url:url.value})
      }).then(data=>data.json()).
      catch(err=>console.log(err.message))
      console.log(res)
      screenshotUrl.value="http://localhost:5000/static/screenshots/"+res.ID+".png"
    }
    return{
      url,screenshotUrl,takeScreenshot
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
header{
  margin:8px;
}
img{
 margin-top:2rem;
}
input{
  border-radius:5px;
  font-size:28px;
  padding:10px;
  margin:5px;
  outline:none;
  border:none;
}
input[type="url"]{
  background:rgb(48, 46, 46);
  color:white;
  border:1px solid #0609b6
}
input[type="submit"]{
  background:#0609b6;
  color:white;
}
</style>
