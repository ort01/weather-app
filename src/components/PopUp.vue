<template>
  <div class="popup" id="popup" @click.self="$emit('background-click')">
    <div class="popup__content">
      <div class="popup__top">
        <h1 class="popup__top--title">
          {{ wData.name }}
        </h1>
      </div>
      <div class="popup__bottom">
        <div class="popup__text">
          <div class="popup__buble popup__buble--temp">
            <h3>Temperature</h3>
            <p>
              Current: <span class="numbers">{{ wData.main.temp }} °C</span>
            </p>
            <p>
              Feels like:
              <span class="numbers">{{ wData.main.feels_like }} °C</span>
            </p>
            <p>
              Min:
              <span class="numbers">{{ wData.main.temp_min }} °C</span>
            </p>
            <p>
              Max:
              <span class="numbers">{{ wData.main.temp_max }} °C</span>
            </p>
          </div>
          <div class="popup__buble popup__buble--sun">
            <h3>Sun</h3>
            <p>
              Sunrise:
              <span class="numbers">{{
                new Date(wData.sys.sunrise * 1000).toLocaleTimeString()
              }}</span>
            </p>
            <p>
              Sunset:
              <span class="numbers">{{
                new Date(wData.sys.sunset * 1000).toLocaleTimeString()
              }}</span>
            </p>
          </div>
          <div class="popup__buble popup__buble--coord">
            <h3>Location of the City</h3>
            <p>
              Lat: <span class="numbers">{{ wData.coord.lat }}</span>
            </p>
            <p>
              Lon: <span class="numbers">{{ wData.coord.lon }}</span>
            </p>
          </div>
          <div class="popup__buble popup__buble--country">
            <h3>Country</h3>
            <h2 class="country-name">{{ wData.sys.country }}</h2>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
  
  <script lang="ts">
import { defineComponent, PropType } from "@vue/runtime-core";
import { Data } from "../interfaces/data";

export default defineComponent({
  name: "PopUp",
  props: {
    wData: {
      required: true,
      type: Object as PropType<Data>,
    },
  },
  methods: {},
});
</script>
  
  
  <style lang="scss">
.popup {
  background-color: blue;
  padding: 2rem;
  height: 100vh;
  width: 100vw;
  background-color: rgb($color-black, 0.7);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;

  opacity: 0;
  visibility: hidden;
  transition: all 0.3s;

  @supports (backdrop-filter: blur(8px)) or (-webkit-backdrop-filter: blur(8px)) {
    background-color: rgb($color-black, 0.2);
    backdrop-filter: blur(
      0.7rem
    ); //styles, what's behind the element that has backdrop-filter applied.
    -webkit-backdrop-filter: blur(0.7rem);
  }

  &:target {
    visibility: visible;
    opacity: 1;
  }

  &__content {
    @include absolute-center;
    background-color: $color-white;
    width: 60%;
    height: 40%;
    box-shadow: 0 2rem 4rem rgba($color-black, 0.2);
    border-radius: 0.5rem;
    display: table; //creates a table with containt the content- right and left
    opacity: 0;
    padding: 3.5rem;
    transform: translate(-50%, -50%) scale(0.5);
    transition: all 0.4s 0.2s;
    z-index: 101;
    display: table;
  }

  &:target &__content {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }

  &__top {
    display: table-row;
    height: 20%;

    &--title {
      padding: 0 1rem;
      font-size: 3rem;
      text-transform: uppercase;
      letter-spacing: 2px;
      display: inline-block;
      border-bottom: solid 5px $color-primary;
    }
  }

  &__bottom {
    height: 80%;
    display: table-row;
  }

  &__text {
    padding: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    gap: 2rem;
    flex-grow: 4;
  }

  &__buble {
    font-size: 1.5rem;
    padding: 1.5rem;
    margin: 1rem;
    background: rgb($color-tertiary, 1);
    border-radius: 0.3rem;
    box-shadow: 0rem 1rem 1.5rem rgb($color-black, 0.2);
    cursor: pointer;
    transition: all 0.2s ease;

    &:hover {
      transform: scale(1.15);
    }

    h3 {
      text-transform: uppercase;
    }

    p {
      text-align: left;
      padding-top: 0.3rem;
    }

    .numbers {
      font-weight: bold;
      display: inline-block;
      padding: 0rem 1rem;
      font-size: 1.7rem;
      position: relative;
      transform: rotateZ(0deg);
    }
    .numbers::before {
      content: "";
      display: block;
      width: 100%;
      height: 100%;
      background: rgba($color-primary, 0.8);
      position: absolute;
      z-index: -1;
      top: 0rem;
      left: 0;
      transform: rotateZ(3deg);
      box-shadow: 0rem 1rem 1.5rem rgba(0, 0, 0, 0.2);
    }

    .country-name {
      font-weight: bold;
      display: inline-block;
      padding: 0rem 1rem;
      font-size: 3rem;
      background: $color-primary;
      box-shadow: 0rem 1rem 1.5rem rgba(0, 0, 0, 0.2);
    }
  }
}
</style>

