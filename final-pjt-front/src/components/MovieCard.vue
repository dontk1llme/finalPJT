<template>
  <div class="movie-card">
    <br>
    <img :src="movie?.poster_URL" alt="#">
    <div class="content1">
      <h1>{{ movie?.title }}</h1>
      <!-- 좋아요 수 -->
      <h3>
        <font-awesome-icon @click="likeUnlike" icon="fa-solid fa-heart" v-show="is_liked"/>
        <font-awesome-icon @click="likeUnlike" icon="fa-regular fa-heart"  v-show="!is_liked"/>
      </h3>
      <p>{{ like_users_count }}명이 좋아합니다</p>
      <hr>
      <p><span class="name">장르</span> <span class="wall">|</span>
          <span  v-for="(genre, index) in movie?.genres" :key="index">
            {{ genre.name }}
          </span>
      </p>
      <p><span class="name">평점</span> <span class="wall">| </span>{{ movie?.popularity }}</p>
      <p><span class="name">개봉일</span><span class="wall"> | </span>{{ movie?.release_date }}</p>
    </div>
    
    <div class="overview">
      <p>{{ movie?.overview }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

const API_URL = 'http://127.0.0.1:8000'

export default {
  name: 'MovieCard',
  computed: {
    movie() {
      return this.$store.state.movie
    },
    is_liked() {
      if (this.$store.state.movie.like_users.indexOf(this.$store.state.userId) === -1) {
        return false
      } else {
        return true
      }
    },
    like_users_count() {
      return this.$store.state.movie.like_users.length
    },
  },
  methods: {
    getMovieLike(movie_id) {
      axios({
        method: 'post',
        url: `${API_URL}/api/v1/movies/${movie_id}/likes/`,
        headers: {
          'Authorization': `Token ${this.$store.state.token}`
        }
      })
        .then((response) => {
          this.is_liked = response.data.is_liked
          this.like_users_count = response.data.like_users_count
          this.$store.dispatch('getOneMovie', movie_id)
        })
      },
    likeUnlike() {
      this.getMovieLike(this.$route.params.movie_id)
    },
  },
}
</script>


<style scoped>
.movie-card {
  /* margin-top: 30px;
  display: inline-block;
  position: relative;
  vertical-align: top;
  justify-content1: center;
  text-align: left;
  width: 100%;
  vertical-align: top; */
  display: inline-block;
  position: relative;
  text-align: left;
  height: auto;
  /* padding: 5px; */
  /* vertical-align: top; */
}

.movie-card > img {
  display: inline-block;
  width: 30%;
  /* max-height: 80vh; */
  margin-right: 5%;
  margin-left: 5%;
  vertical-align: middle;
  /* border: 3px solid #958a8a41; */
  box-shadow: 12px 12px #958a8a7a;
}
.content1 {
  display: inline-block;
  width: 40%;
  margin-right: 5%;
  margin-left: 5%;
  vertical-align: top;

}
.content1 > p{
  /* display: flex; */
  white-space: normal; 
  word-break: keep-all;
  margin-bottom: 10px;
  font-size: 70%;
}

.content1 > span{
  font-size: 5px;
}

.content1 > h1 {
  margin-bottom: 2px;
  width: 150%;
  font-size: 20px;
  /* font-size: 150%; */
  text-shadow: 2px 2px  #746a6a61;
  white-space: normal; 
  word-break: keep-all;
}

.name {
  display: inline-block;
  width: 40%;
  text-align: center;
  font-weight: 600;
}

.wall {
  margin-right: 1.5vw;
}


svg {
  color: rgb(208, 93, 93);
  font-size: 80%;
}

.overview {
  margin-top: 15px;
  /* margin-right: 5%;
  margin-left: 5%; */
  padding: 3%;
  border: 3px dashed #E6E6E6;
  border-radius: 30px;
  background-color: rgb(230, 230, 230, 0.5);
}

.overview > p {
  font-size: 80%;
  white-space: normal; 
  word-break: keep-all;
}
</style>