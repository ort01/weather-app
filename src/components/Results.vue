<template>
  <div class="results">
    <h1 class="results__cityName">{{ wData.name }}</h1>
    <div>
      <img
        :src="getImg"
        alt="weather-picture"
        class="results__img"
        :class="getAnimation"
      />
    </div>
    <div class="results__weather">
      <h3 class="results__weather--description">
        {{ wData.weather[0].description }}
      </h3>
      <p class="results__weather--temp">{{ Math.round(wData.main.temp) }} °C</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "@vue/runtime-core";
import { Data } from "../interfaces/data";

export default defineComponent({
  name: "Results",
  props: {
    wData: {
      required: true,
      type: Object as PropType<Data>,
    },
  },

  data: () => {
    return {};
  },
  methods: {},
  computed: {
    getImg() {
      switch (this.wData.weather[0].main) {
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
    getAnimation() {
      switch (this.wData.weather[0].main) {
        case "Clear":
          return "rotate360";
        case "Snow":
          return "cloudShake";
        case "Clouds":
          return "cloudMovement";
        case "Mist":
          return "cloudMovementLinear";
        case "Fog":
          return "cloudMovementLinear";
        case "Thunderstorm":
          return "cloudShake";
        case "Rain":
          return "cloudShake";
        case "Drizzle":
          return "cloudShake";
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
      background: rgba($color-primary, 1);
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
    width: 50%;
    opacity: 0.9;
    margin-top: -2rem;
    animation: horizontal-shaking 2s ease-out 1s;

    @include respond(tab-land) {
      width: 70%;
    }

    @include respond(tab-port) {
      width: 90%;
    }

    @include respond(phone) {
      width: 100%;
    }
  }
  &__weather {
    text-transform: uppercase;
    letter-spacing: 0.4rem;
    margin-top: -2rem;
  }
}

.rotate360 {
  animation: rotate360 2s ease 1s;
}
.cloudMovement {
  animation: cloudMovement 3s ease-out 1s;
}
.cloudMovementLinear {
  animation: cloudMovementLinear 3s ease-out 1s;
}
</style>