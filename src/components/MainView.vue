<template>
  <div>
    <header>
      <h1>devfinder</h1>
      <button class="btn btn-switch">dark</button>
    </header>

    <div>
      <div class="search__block">
        <input
          v-model="username"
          v-on:keydown.13="search"
          type="text"
          placeholder="Search Github username..."
        />
        <span class="search__error">{{ error }}</span>
        <button class="btn btn-search" @click="search">Search</button>
      </div>

      <div class="result__block" v-if="results">
        <div class="result__avatar">
          <img
            :src="results.avatar_url"
            :alt="'Github avatar for ' + results.name"
          />
        </div>

        <div class="result__content">
          <div class="result__box">
            <div>
              <div class="result__name">{{ results.name }}</div>
              <div class="result__user">@{{ results.login }}</div>
            </div>

            <div class="result__date">Joined {{ joined }}</div>
          </div>

          <div v-if="results.bio" class="result__bio">{{ results.bio }}</div>
          <div v-else class="result__bio">This profile has no bio</div>

          <div class="result__stats">
            <div class="result__stat">
              <div class="result__stat-title">Repos</div>
              <strong class="result__stat-value">{{
                results.public_repos
              }}</strong>
            </div>
            <div class="result__stat">
              <div class="result__stat-title">Followers</div>
              <strong class="result__stat-value">{{
                results.followers
              }}</strong>
            </div>
            <div class="result__stat">
              <div class="result__stat-title">Following</div>
              <strong class="result__stat-value">{{
                results.following
              }}</strong>
            </div>
          </div>

          <div class="result__data">
            <div
              class="result__info"
              :class="results.location ? 'available' : ''"
            >
              {{ results.location ? results.location : "Not available" }}
            </div>
            <div class="result__info" :class="results.blog ? 'available' : ''">
              {{ results.blog ? results.blog : "Not available" }}
            </div>
            <div
              class="result__info"
              :class="results.twitter_username ? 'available' : ''"
            >
              {{
                results.twitter_username
                  ? results.twitter_username
                  : "Not available"
              }}
            </div>
            <div
              class="result__info"
              :class="results.company ? 'available' : ''"
            >
              {{ results.company ? results.company : "Not available" }}
            </div>
          </div>
        </div>
      </div>
      <div v-else class="result__notfound">No results</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      username: "",
      results: [],
      error: "",
    };
  },
  watch: {
    username: function () {
      this.error = "";
    },
  },
  computed: {
    joined() {
      return this.results.created_at.split("T").shift().split("-");
    },
  },
  methods: {
    search: function () {
      if (this.username) {
        axios
          .get("https://api.github.com/users/" + this.username)
          .then((response) => {
            console.log(response.data);
            this.results = response.data;
            this.error = "";
          })
          .catch((error) => {
            this.results = "";
            if (error.response.status === 404) {
              this.error = "No results";
            } else {
              this.error = error;
            }
          });
      }
    },
  },
  created() {
    axios
      .get("https://api.github.com/users/murodbek-norboyev")
      .then((response) => {
        console.log(response.data);
        this.results = response.data;
        this.error = "";
      })
      .catch((error) => {
        this.results = "";
        if (error.response.status === 404) {
          this.error = "No results";
        } else {
          this.error = error;
        }
      });
  },
};
</script>

<style lang="sass" scoped>
header
  display: flex
  align-items: center
  justify-content: space-between

.search__block
  display: flex
  justify-content: space-between
  position: relative
  border-radius: 15px
  padding: 10px
  margin-bottom: 50px
  background-color: #FEFEFE
  box-shadow: 0px 16px 30px -10px rgba(70, 96, 187, 0.2)

  input
    flex: 1
    background: none
    border: none
    outline: 0
    color: #4B6A9B

  .btn-search
    border-radius: 10px
    background-color: #0079ff
    color: #fff
    padding: 14px 20px
    text-align: center

  .search__error
    position: absolute
    right: 130px
    color: red
    font-weight: 700
    top: 50%
    transform: translateY(-50%)

.btn
  cursor: pointer
  border: none
  background-color: transparent
  font-weight: 700

.result__notfound
  text-align: center

.result__block
  display: flex
  background-color: #fff
  padding: 40px
  border-radius: 15px
  box-shadow: 0px 16px 30px -10px rgba(70, 96, 187, 0.2)

.result__content
  flex: 1

.result__box
  display: flex
  justify-content: space-between
  margin-bottom: 16px

.result__name
  font-size: 26px
  font-weight: 700
  color: #2B3442

.result__user
  color: #0079ff

.result__date
  color: #4B6A9B

.result__bio
  color: #4B6A9B
  margin-bottom: 40px

.result__avatar
  margin-right: 40px

  img
    max-width: 117px
    border-radius: 50%

.result__stats
  display: flex
  justify-content: space-between
  padding: 20px 30px
  border-radius: 15px
  background-color: #F6F8FF
  margin-bottom: 40px

.result__stat-title
  font-size: 14px
  color: #4B6A9B
  margin-bottom: 20px

.result__stat-value
  font-size: 22px

.result__data
  display: flex
  flex-wrap: wrap
  justify-content: space-between
  margin-top: -16px

  .result__info
    margin-top: 16px
    max-width: 50%
    flex: 0 0 50%
    color: rgb(74 105 156 / 50%)

    &.available
      color: #4B6A9B
</style>
