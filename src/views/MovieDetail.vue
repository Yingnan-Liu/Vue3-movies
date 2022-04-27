<template>
  <div class="movie-detail" >
      <h2>{{movie.Title}}</h2>
      <p>{{movie.Year}}</p>
      <img :src="movie.Poster" alt="movie" class="poster-img"/>
      <p>{{movie.Plot}}</p>
  </div>
</template>

<script>
import { reactive ,onBeforeMount } from 'vue';
import {useRoute} from "vue-router"
import env from '@/env.js'
export default {
  setup(){
    const state=reactive({
      movie:{}
    })
    const route = useRoute()
    
    onBeforeMount(()=>{
      fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
      .then((res)=>res.json())
      .then(data=>{
        state.movie=data
        console.log(data)
      }).catch(error=>console.log(error))
    })

    return(
      state
    )
  }
}
</script>

<style lang="scss">
  .movie-detail{
    padding: 16px;
    h2{
      color:#FFF;
      font-size: 28px;
      font-weight: 600;
      margin-bottom: 16px;

    }
    .featured-img{
      display: block;
      max-width:200px;
      margin-bottom:16px;
    }
    p{
      color:#FFF;
      font-size: 14px;
      line-height: 1.4;
    }
  }
</style>