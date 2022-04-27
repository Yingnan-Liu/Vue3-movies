<template>
  <div class="home">
    <div class="feature-card">
      <!-- nthis needs change -->
      <router-link to="/movie/tt3546">
          <img class="feature-img" src="https://tse1-mm.cn.bing.net/th/id/R-C.4b051e5d69ed2f27c2b2295dade8426d?rik=XqJA%2fJAgRYqGSw&riu=http%3a%2f%2fstatic.guim.co.uk%2fsys-images%2fGuardian%2fPix%2fpictures%2f2014%2f9%2f3%2f1409755613016%2fFriends-season-10-014.jpg&ehk=u6KIb7oEbC2OskrdCMXkaUXhgRbF5v5MbLzugblZ5Co%3d&risl=&pid=ImgRaw&r=0" alt="Hi Friends">
          <div class="detail">
            <h3>Friends</h3>
            <p>They are six 20 something year olds living in New York City. Over the course of 10 years and seasons, these friends go through family, love, drama, friendship, and comedy.</p>
          </div>
      </router-link>       
    </div>
    <form @submit.prevent="SearchMovies()" action="" class="search-box">
          <input type="text" placeholder="What are you looking for?" v-model="search">
          <input type="submit" value="Search">
    </form>
    <!-- 搜索结果 -->
    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID" >
        <router-link class="movie-link" to="'/movie/' + movie.imdbID">
          <div class="product-image">
            <img :src="movie.Poster" alt="movie poster"/>
            <div class="type">{{movie.Type}}</div>
          </div>
          <div class="detail">
            <p class="year">{{movie.Year}}</p>
            <h3>{{movie.Title}}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';
import env from '@/env.js';

export default {
  
   setup(){
     const search=ref("");
     const movies=ref([])
    
     const SearchMovies=()=>{
      //  input输入的值
       if(search.value!=""){         
         fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
         .then(response=>response.json())
         .then(data=>{
           movies.value=data.Search;
           search.value=""; //reset input
           console.log(movies)
         }).catch(error=>console.log(error))
       }
     }

     return{
       search,
       movies,
       SearchMovies
     }
   }
}
</script>

<style lang="scss">
.home{
  .feature-card{
    position:relative;
    .feature-img{
      // 将img标签转换为block
      display: block;
      width:100%;
      object-fit: cover;
      position: relative;
      z-index:0;
    }
    .detail{
      position:absolute;
      bottom:0;
      left:0;
      right:0;
      background-color:rgba(0,0,0,0.6);
      padding:16px;
      z-index:1;
      h3{
        color:#FFF;
        margin-bottom:16px;
      }
      p{
        color:#FFF;
      }
    }
  }
  
  .search-box{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items:center;
    padding:1rem;

    input{
      display: block;
      appearance: none;
      border:none;
      outline:none;
      background: none;
      &[type="text"]{
        width:100%;
        color:#FFF;
        background-color: #496583;
        font-size:20px;
        padding:10px 16px;
        border-radius:8px;
        margin-bottom:15px;
        transition:0.4s;
        &::placeholder{
          color:#F3F3F3;
          opacity:0.8;
        }
        &:focus{
          box-shadow:0px 3px 6px rgba(0,0,0,0.2)
        }
      }

      &[type="submit"]{
        width:100%;
        max-width:300px;
        background-color:#42B883;
        padding:10px 16px;
        border-radius: 8px;
        color:white;
        font-size:20px;
        text-transform: uppercase;
        transition: 0.4s;
        &:active{
          background-color: #3B8070;
        }
      }
    }
  }
  .movies-list{
    display: flex;
    flex-wrap:wrap;
    margin:0 8px;
    .movie{
      max-width:50%;
      flex:1 1 50%;
      padding: 16px 8px;
      .movie-link{
        display: flex;
        flex-direction: column;
        height:100%;
        .product-image{
          position: relative;
          display: block;
          img{
            display: block;
            width:100%;
            height:275px;
            object-fit: cover;
          }
          .type{
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883e2;
            border-radius: 0px 6px 6px 0px;
            color: #FFF;
            bottom: 16px;
            left:0;
            text-transform: capitalize;
          }
        }
        .detail{
          background-color: #496583;
          padding:16px 8px;
          flex:1 1 100%;
          border-radius: 0px 0px 8px 8px;
          .year{
            color: #AAA;
            font-size: 15px;
          }
          h3{
            font-size: 18px;
            color:#FFF
          }
        }
      }
    }
  }
}
</style>