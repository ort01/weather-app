<template>
  <div class="results">
    <h1 class="results__cityName">{{ cityName }}</h1>
    <div>
      <img :src="getImg" alt="weather-picture" class="results__img" />
    </div>
    <div class="results__weather">
      <h3 class="results__weather--description">
        {{ wDescription }}
      </h3>
      <p class="results__weather--temp">{{ wTemp }} °C</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "@vue/runtime-core";
import { Data } from "../interfaces/data";

export default defineComponent({
  name: "Results",
  props: {
    // wData: {
    //   required: true,
    //   type: {} as PropType<Data>,
    // },
    wDescription: {
      required: true,
      type: String,
    },
    wTemp: {
      required: true,
      type: Number,
    },
    wImgDescription: {
      required: true,
      type: String,
    },
    cityName: {
      required: true,
      type: String,
    },
  },

  // ["wDescription", "wTemp", "wImgDescription", "cityName"],
  data: () => {
    return {};
  },
  methods: {},
  computed: {
    getImg() {
      switch (this.wImgDescription) {
        case "Clear":
          return "../src/assets/clear-sky.png";
        case "Snow":
          return "../src/assets/snow.png";
        case "Clouds":
          return "../src/assets/few-clouds.png";
        case "Mist":
          return "../src/assets/mist.png";
        case "Fog":
          return "../src/assets/mist.png";
        case "Thunderstorm":
          return "../src/assets/thunderstorm.png";
        case "Rain":
          return "../src/assets/rain.png";
        case "Drizzle":
          return "../src/assets/shower-rain.png";
      }
    },
  },
});
</script>


<style lang="scss">
.results {
  font-size: 2rem;
  width: 50%;
  text-align: center;
  padding: 8rem 8rem 2rem;

  &__cityName {
    text-transform: uppercase;
    letter-spacing: 0.4rem;
    margin: 2.5rem 0;
    color: white;
    display: inline-block;
    padding: 0 1.5rem 0.5rem;
    position: relative;
    transform: rotateZ(0deg);

    &::before {
      content: "";
      display: block;
      width: 100%;
      height: 100%;
      background: rgba($color-secondary, 0.9);
      position: absolute;
      z-index: -1;
      top: -0.5rem;
      left: 0;
      border-radius: 0.5rem;
      transform: rotateZ(-2deg);
      box-shadow: 0rem 2rem 4rem rgba(0, 0, 0, 0.2);
    }
  }

  &__img {
    width: 90%;
    opacity: 0.9;
    margin-top: -5rem;
    animation: rotate360 1s ease 1s;
  }
  &__weather {
    text-transform: uppercase;
    letter-spacing: 0.4rem;
    margin-top: -2rem;
  }
}
</style>