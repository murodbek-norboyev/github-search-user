<template>
  <div>
    <header>
      <h1>devfinder</h1>
      <!--      <button class="btn btn-switch">dark</button>-->
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
              <a :href="results.html_url" target="_blank" class="result__user"
                >@{{ results.login }}</a
              >
            </div>

            <div class="result__date">{{ joined }}</div>
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
              <div class="result__stat-title">
                Followers
                <svg
                  fill="currentColor"
                  height="16"
                  viewBox="0 0 16 16"
                  version="1.1"
                  width="16"
                  style="vertical-align: bottom"
                >
                  <path
                    fill-rule="evenodd"
                    d="M5.5 3.5a2 2 0 100 4 2 2 0 000-4zM2 5.5a3.5 3.5 0 115.898 2.549 5.507 5.507 0 013.034 4.084.75.75 0 11-1.482.235 4.001 4.001 0 00-7.9 0 .75.75 0 01-1.482-.236A5.507 5.507 0 013.102 8.05 3.49 3.49 0 012 5.5zM11 4a.75.75 0 100 1.5 1.5 1.5 0 01.666 2.844.75.75 0 00-.416.672v.352a.75.75 0 00.574.73c1.2.289 2.162 1.2 2.522 2.372a.75.75 0 101.434-.44 5.01 5.01 0 00-2.56-3.012A3 3 0 0011 4z"
                  ></path>
                </svg>
              </div>
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
              <svg
                fill="currentColor"
                viewBox="0 0 16 16"
                version="1.1"
                width="16"
                height="16"
              >
                <path
                  fill-rule="evenodd"
                  d="M11.536 3.464a5 5 0 010 7.072L8 14.07l-3.536-3.535a5 5 0 117.072-7.072v.001zm1.06 8.132a6.5 6.5 0 10-9.192 0l3.535 3.536a1.5 1.5 0 002.122 0l3.535-3.536zM8 9a2 2 0 100-4 2 2 0 000 4z"
                ></path>
              </svg>
              {{ results.location ? results.location : "Not available" }}
            </div>
            <div class="result__info" :class="results.blog ? 'available' : ''">
              <a v-if="results.blog" :href="results.blog" target="_blank">
                <svg
                  fill="currentColor"
                  height="16"
                  viewBox="0 0 16 16"
                  version="1.1"
                  width="16"
                >
                  <path
                    fill-rule="evenodd"
                    d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"
                  ></path>
                </svg>
                {{ results.blog }}
              </a>
              <span v-else>Not available</span>
            </div>
            <div
              class="result__info"
              :class="results.twitter_username ? 'available' : ''"
            >
              <svg
                width="16"
                height="13"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 273.5 222.3"
              >
                <path
                  d="M273.5 26.3a109.77 109.77 0 0 1-32.2 8.8 56.07 56.07 0 0 0 24.7-31 113.39 113.39 0 0 1-35.7 13.6 56.1 56.1 0 0 0-97 38.4 54 54 0 0 0 1.5 12.8A159.68 159.68 0 0 1 19.1 10.3a56.12 56.12 0 0 0 17.4 74.9 56.06 56.06 0 0 1-25.4-7v.7a56.11 56.11 0 0 0 45 55 55.65 55.65 0 0 1-14.8 2 62.39 62.39 0 0 1-10.6-1 56.24 56.24 0 0 0 52.4 39 112.87 112.87 0 0 1-69.7 24 119 119 0 0 1-13.4-.8 158.83 158.83 0 0 0 86 25.2c103.2 0 159.6-85.5 159.6-159.6 0-2.4-.1-4.9-.2-7.3a114.25 114.25 0 0 0 28.1-29.1"
                  fill="currentColor"
                ></path>
              </svg>
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
              <svg
                fill="currentColor"
                viewBox="0 0 16 16"
                version="1.1"
                width="16"
                height="16"
              >
                <path
                  fill-rule="evenodd"
                  d="M1.5 14.25c0 .138.112.25.25.25H4v-1.25a.75.75 0 01.75-.75h2.5a.75.75 0 01.75.75v1.25h2.25a.25.25 0 00.25-.25V1.75a.25.25 0 00-.25-.25h-8.5a.25.25 0 00-.25.25v12.5zM1.75 16A1.75 1.75 0 010 14.25V1.75C0 .784.784 0 1.75 0h8.5C11.216 0 12 .784 12 1.75v12.5c0 .085-.006.168-.018.25h2.268a.25.25 0 00.25-.25V8.285a.25.25 0 00-.111-.208l-1.055-.703a.75.75 0 11.832-1.248l1.055.703c.487.325.779.871.779 1.456v5.965A1.75 1.75 0 0114.25 16h-3.5a.75.75 0 01-.197-.026c-.099.017-.2.026-.303.026h-3a.75.75 0 01-.75-.75V14h-1v1.25a.75.75 0 01-.75.75h-3zM3 3.75A.75.75 0 013.75 3h.5a.75.75 0 010 1.5h-.5A.75.75 0 013 3.75zM3.75 6a.75.75 0 000 1.5h.5a.75.75 0 000-1.5h-.5zM3 9.75A.75.75 0 013.75 9h.5a.75.75 0 010 1.5h-.5A.75.75 0 013 9.75zM7.75 9a.75.75 0 000 1.5h.5a.75.75 0 000-1.5h-.5zM7 6.75A.75.75 0 017.75 6h.5a.75.75 0 010 1.5h-.5A.75.75 0 017 6.75zM7.75 3a.75.75 0 000 1.5h.5a.75.75 0 000-1.5h-.5z"
                ></path>
              </svg>
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
      const isoStr = this.results.created_at;
      const result = new Date(isoStr);

      return `Joined ${result.getHours()} ${result.toLocaleString("en-US", {
        month: "short",
      })} ${result.getFullYear()}`;
      // return this.results.created_at ? this.results.created_at.toString() : "";
    },
  },
  methods: {
    search: function () {
      if (this.username) {
        axios
          .get("https://api.github.com/users/" + this.username)
          .then((response) => {
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

a
  text-decoration: none
</style>
