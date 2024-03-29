<template>
  <div class="home">
    <transition name="locator" appear>
      <Locator class="home__locator" @keydown.enter="getData" @cityName="getCityName"
        :class="{ locatorResults: weather.hasData, locatorError: error }" />
    </transition>
    <transition name="results" mode="out-in">
      <Results class="home__results" v-if="weather.hasData" :wData="weather.data" />
    </transition>
    <transition name="error" mode="out-in">
      <div v-if="error" class="home__error">
        <img src="../../public/static/images/error-televes.jpg" alt="error-img" class="home__error--img" />
        <p class="home__error--msg">{{ errorMsg }}</p>
      </div>
    </transition>
    <div class="home__popup" v-if="weather.hasData">
      <a href="#popup" class="home__popup--button gradient">Show more info</a>
      <PopUp id="popup" @background-click="togglePopUp" :wData="weather.data" />
    </div>
    <footer class="footer"></footer>
  </div>
</template>

<script lang="ts">
import Locator from "../components/Locator.vue";
import Results from "../components/Results.vue";
import PopUp from "../components/PopUp.vue";
import { defineComponent } from "vue";
import axios from "axios";
import { Response, Data } from "../interfaces/data";
import Error from "../interfaces/error";

export default defineComponent({
  name: "Home",
  components: {
    Locator,
    Results,
    PopUp,
  },
  props: {},
  data: () => {
    return {
      weather: {
        data: {} as Data,
        description: "" as string,
        hasData: false as boolean,
      },
      cityName: "" as string,
      error: false as boolean,
      errorMsg: "" as string,
    };
  },
  methods: {
    getData(): void {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&units=metric&appid=a93ae0265bb3daeb09fd3c01d03ee938`
        )
        .then((res: Response): void => {
          console.log(res);

          this.weather.data = res.data;
          this.weather.hasData = true;
          this.error = false;
        })
        .catch((err: Error): void => {
          this.error = true;
          this.weather.hasData = false;
          if (err.response.status === 404) {
            this.errorMsg = "City Not Found";
          } else {
            this.errorMsg = "Unknown Error";
            console.log("Unknown Error");
          }
        });
    },
    getCityName(name: string): void {
      this.cityName = name;
    },
    togglePopUp(): void {
      window.location.href = "#";
    },
  },
});
</script>


<style lang="scss">
.home {
  text-align: center;

  &__locator {
    width: fit-content;
    margin: 50rem auto 0;
    transition: all 0.5s;
  }

  &__results {
    position: relative;
    margin: 0 auto;
    // animation: showResults 0.7s ease-in;
  }

  &__popup {

    &--button:visited,
    &--button:link {
      display: inline-block;
      text-decoration: none;
      text-transform: uppercase;
      color: $color-white;
      background: rgba($color-primary, 1);
      border-radius: 5rem;
      padding: 0.5rem 2rem;
      font-size: 1.5rem;
      transition: all 0.4s ease-in-out;
      background-size: 300% 100%;
      // animation: showResults 0.7s ease-in;
    }

    &--button:hover {
      box-shadow: 0rem 1rem 1.5rem rgba($color-black, 0.2);
      background-position: 100% 0;
      transform: translateY(-0.3rem);
    }

    &--button:active {
      box-shadow: 0rem 0.5rem 1rem rgba($color-black, 0.2);
      transform: translateY(-0.1rem);
    }

    &--button.gradient {
      background-image: linear-gradient(to right,
          $color-primary,
          rgb(192, 192, 192),
          $color-primary );
      box-shadow: 0 4px 15px 0 rgba($color-light-blue, 0.75);
    }
  }

  //error

  &__error {
    width: 50%;
    // animation: showError 0.7s ease;
    font-size: 4rem;
    margin: 10rem auto 0;

    &--img {
      margin: 0 auto;
      width: 75%;
      border-radius: 8px;
      display: block;
      box-shadow: 0rem 1rem 4rem rgba(0, 0, 0, 0.2);
    }

    &--msg {
      margin: 3rem 0;
      color: white;
      display: inline-block;
      padding: 0 1.5rem 0.5rem;
      text-transform: uppercase;
      letter-spacing: 0.5rem;
      position: relative;
      transform: rotateZ(0deg);

      &::before {
        content: "";
        display: block;
        width: 100%;
        height: 100%;
        background: rgba(235, 167, 42, 0.9);
        position: absolute;
        z-index: -1;
        top: -0.5rem;
        left: 0;
        transform: rotateZ(-2deg);
        box-shadow: 0rem 1rem 4rem rgba(0, 0, 0, 0.2);
      }
    }
  }
}

//footer

.footer {
  height: 10rem;
}

//animations

.locatorResults {
  margin: 20rem auto 0rem;
  animation: showResults 0.7s ease-in;
}

.locatorError {
  margin: 20rem auto 0;
}

// vue transitions

.locator {
  &-enter-from {
    opacity: 0;
    transform: translateY(2rem);
  }

  &-enter-active {
    transition: all 0.6s ease;
  }
}

.results {
  &-leave-to {
    opacity: 0;
    transform: translateX(-15rem);
  }

  &-leave-active {
    transition: all 0.3s ease-in;
    position: absolute;
  }
}

.error {
  &-enter-from {
    opacity: 0;
    transform: translateX(20rem);
  }

  &-enter-active {
    transition: all 0.6s ease;
  }

  &-leave-to {
    opacity: 0;
    transform: translateX(-20rem);
  }

  &-leave-active {
    transition: all 0.3s ease-in;
    position: absolute;
  }
}
</style>