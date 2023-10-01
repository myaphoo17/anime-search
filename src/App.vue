<template>
  <div class="app">
    <header>
      <h1>My Anime Database</h1>
      <form action="search" class="search-box" @submit.prevent="HandleSearch">
        <input
          type="search"
          class="search-field"
          placeholder="  Search anime.."
          required
          v-model="search_query"
        />
      </form>
    </header>

    <main>
      <div class="cards">
        <CardView
          v-for="anime in animeList"
          :key="anime.mal_id"
          :anime="anime"
        />
      </div>
    </main>
  </div>
</template>
<script>
import CardView from "./components/CardView.vue";
import { ref } from "vue";

export default {
  setup() {
    const search_query = ref("");
    const animeList = ref([]);

    const HandleSearch = async () => {
      try {
        const response = await fetch(
          `https://api.jikan.moe/v4/anime?q=${search_query.value}`
        );
        const data = await response.json();

        if (response.status >= 400) {
          console.error("API request error:", response.status);
          // Set an error message for the user
          animeList.value = [];
          return;
        }

        // Assuming you want to extract the 'data' array from the response
        if (data && Array.isArray(data.data)) {
          animeList.value = data.data;
          console.log(animeList.value);
        } else {
          console.error("Invalid API response:", data);
          // Set an error message for the user
          animeList.value = [];
        }
      } catch (error) {
        console.error("Error fetching data:", error);
        // Set an error message for the user
        animeList.value = [];
      }
    };

    return {
      search_query,
      animeList,
      HandleSearch,
    };
  },
  components: {
    CardView,
  },
};
</script>


<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  background: #333333;
}

a {
  text-decoration: none;
}

header {
  padding-top: 2rem;
  padding-bottom: 5rem;

  h1 {
    color: #ffffff;
    font-family: "Courier New", Courier, monospace;

    font-size: 3rem;
    font-weight: 550;
    text-align: center;
    text-transform: lowercase;
    margin-bottom: 2rem;
    animation: movetoleft 1s ease forwards;

    strong {
      color: #000;
    }
  }
  @keyframes movetoleft {
    0% {
      opacity: 0;
      transform: translateX(-50px);
    }
    80% {
      transform: translateX(20px);
    }
    100% {
      opacity: 1;
      transform: translateX(0);
    }
  }

  @keyframes movetoBtm {
    0% {
      opacity: 0;
      transform: translateY(30px);
    }
    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .search-box {
    display: flex;

    padding-left: 0px;
    padding-right: 0px;
    margin-left: 32%;
    animation: movetoBtm 1s ease forwards;
    background: none;

    .search-field {
      appearance: none;
      background: none;
      border: none;
      outline: none;
      background-color: #ffffff7a;
      box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.555);
      display: flex;
      width: 55%;
      max-width: 500px;
      padding: 15px;
      border-radius: 20px;
      color: rgb(99, 94, 96);
      font-size: 1rem;
      transition: 0.4s;

      &::placeholder {
        color: white;
      }
      &:focus,
      &:valid {
        color: white;
        background-color: #8a8484be;

        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
      }
    }
  }

  main {
    max-width: 1000px;
    margin: 0 auto;
    padding-left: 30px;
    padding-right: 30px;
  }
}

.cards {
  display: flex;

  flex-wrap: wrap;
  justify-content: space-between;
  justify-content: center;

  margin: 10px 40px;
}

.card {
  flex: 1 1 calc(33.333% - 12px);
  max-width: calc(33.333% - 12px);
  padding: 0px 8px;
  margin-bottom: 16px;
  justify-content: space-between;
  background: none;
}

img {
  background: none;

  width: 65%;
  height: 280px;
  object-fit: fill;
  border-radius: 15px;

  transition: 0.4s;
}

h3 {
  padding: 16px;
  color: #313131;
  transition: 0.4s;
  background: none;
}

.card:hover img {
  transform: scale(1.05);
}

.card:hover h3 {
  color: #ffffff;
}

@media only screen and (max-width: 650px) and (min-width: 200px) {
  header {
    padding: 1rem 0 2rem;

    h1 {
      font-size: 1rem;
    }
  }

  img {
    background: none;
    width: 90%;
    height: 5rem;
    object-fit: fill;
    border-radius: 5px;

    transition: 0.4s;
  }

  .search-box {
    flex-direction: row;

    .search-field {
      font-size: 0.5rem;
    }
  }

  .cards {
    justify-content: space-between;
  }

  .card {
    font-size: 0.5rem;
    flex: 1 1 calc(33.333% - 1rem);
    max-width: calc(33.333% - 1rem);
    margin: 1px;
  }
}
</style>
