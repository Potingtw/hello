<script>
  import { ref } from 'vue';

  export default {
    setup() {
      //這邊把API回傳json資料中需要使用的資料丟到ref裡
      const movieTitle = ref('');
      const moviePoster = ref('');
      const movieName = ref('');
      const movieActors = ref('');
      const movieDirector = ref('');
      const movieReleased = ref('');
      const movieYear = ref('');
      const moviePlot = ref('');
      
      //宣告函式movieData以fetch方式取得API傳來的資料,再用try,catch把成功或是失敗的結果紀錄
      const movieData = async () => {
        try {
          //宣告變數response儲存回傳的資料
          const response = await fetch(`https://www.omdbapi.com/?i=tt3896198&apikey=ed6d449e&t=${movieTitle.value}`);
          if (!response.ok) {
            throw new Error("fetch失败");
          }
          //宣告變數result 把response的資料轉乘json格式
          const result = await response.json();
          console.log(result);
          //在剛剛ref那邊宣告的變數要加.value才可以存需要的資料
          moviePoster.value = result.Poster
          movieName.value = result.Title
          movieActors.value = result.Actors
          movieDirector.value = result.Director
          movieReleased.value = result.Released
          movieYear.value = result.Year
          moviePlot.value = result.Plot
        } catch (error) {
          //這邊就是連結API有錯的話會跟你說錯誤是啥
          console.error(error);
          throw error;
        }
      }
      return {
        //這邊把剛剛宣告的變數retrun出去 然後不確定是不是ref那邊才可以用哈哈,反正應該大概是這樣要用的就return出去
        movieTitle,
        movieData,
        moviePoster,
        movieName,
        movieActors,
        movieDirector,
        movieReleased,
        movieYear,
        moviePlot
      };
    },
  };
</script>

<template>
  <h1>MovieList</h1>
  <form @submit.prevent>
    <label>電影名稱:</label>
    <input v-model="movieTitle">
    <button @click="movieData">搜尋</button>
  </form>
  <div>
    <img :src="moviePoster" >
    <h1>電影名稱:{{movieName}}</h1>
    <p>電影簡介:{{moviePlot}}</p>
    <p>電影演員:{{movieActors}}</p>
    <p>電影導演:{{movieDirector}}</p>
    <p>電影上映日:{{movieReleased}}</p>
    
  </div>
  
  
</template>

<style>
.movname{
  font-size:18px
}
  
</style>
