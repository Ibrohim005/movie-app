<template>
  <div class="container text-center">
    <img
      src="./assets/regnum_picture_162756124240681_normal.jpeg"
      class="bg position-fixed top-0 w-100 vh-full"
      alt=""
    />
    <div class="row">
      <div class="col-4 offset-4">
        <h1 class="title p-3">Search movie:</h1>
        <input
          class="input p-2"
          type="text"
          placeholder="Search..."
          v-model="query"
          @keypress.enter="search"
        />
      </div>
    </div>
    <div class="row">
      <div
        class="col-4 pt-4"
        v-for="(item, index) in searchResults.results"
        :key="index"
      >
        <div class="card">
          <img
            class="card__img"
            :src="
              item.multimedia != null
                ? item.multimedia.src
                : './assets/pageNotFound.jpg'
            "
            alt="Movie's image not found"
          />
          <div class="card-info">
            <h1 class="card__title">
              {{ item.display_title }}
            </h1>
            <p class="card__headline">
              <span class="title">Headline:</span> {{ item.headline }}
            </p>
            <p class="card__summary">
              <span class="title">Summary:</span> {{ item.summary_short }}
            </p>
            <div class="card__publish">
              {{ item.publication_date }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      base_url:
        "https://api.nytimes.com/svc/movies/v2/reviews/search.json?query=",
      api_key: "9KHMuc2vTIYJWLcMGbAnNtn9gD2E0QNh",
      query: "",
      searchResults: {
        results: [
          {
            display_title: "Movie Name",
            headline: "Headline",
            publication_date: "Publication date",
            summary_short: "Summary",
            link: {
              url: "Url",
            },
            multimedia: {
              src: "./assets/pageNotFound.jpg",
            },
          },
        ],
      },
    };
  },
  // created: function () {
  //   this.search();
  // },
  methods: {
    search() {
      axios
        .get(`${this.base_url}${this.query}&api-key=${this.api_key}`)
        .then((response) => {
          this.searchResults = response.data;
          console.log(this.searchResults);
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
};
</script>

<style lang="scss">
.input {
  outline: none;
  border: none;
  border-radius: 4px;
  background: transparent;
  width: 100%;
  transition: 0.4s;
  color: white;
  border-bottom: 2px solid white;
  &:focus {
    border-radius: 16px;
  }
  ::placeholder {
    color: white;
  }
}
.bg {
  z-index: -1;
  left: 0;
}
.title {
  color: white;
}
.card {
  position: relative;
  height: 400px;
  &:hover {
    .card-info {
      opacity: 1;
      transform: translate3d(0, 0, 0);
    }
    .card__img {
      transform: scale(0.9);
    }
  }
  &__headline,
  &__summary {
    text-align: justify;
    .title {
      font-style: bold;
      font-weight: 600;
      color: black;
    }
  }
  &__title {
    font-size: 20px;
  }
  &__img {
    height: 100%;
    width: 100%;
  }
  &__link {
    text-decoration: none;
    color: aqua;
  }
  &-info {
    position: absolute;
    bottom: 0;
    background: rgba(white, 0.9);
    padding: 20px;
    height: 100%;
    width: 100%;
    opacity: 0;
    transform: translate3d(0, 10%, 0);
    transition: opacity 350ms ease-in-out 150ms,
      transform 350ms ease-in-out 150ms;
  }
}
</style>
