<template>
  <div class="movie">

  </div>
</template>

<script>
import {toRefs} from 'vue'
export default {
    setup(props){
        const {movies}=toRefs(props)
        console.log(movies)
        return{

        }
    }
}
</script>

<style>

</style>