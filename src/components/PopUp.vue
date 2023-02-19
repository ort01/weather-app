<template>
  <div class="popup" id="popup" @click.self="$emit('background-click')">
    <div class="popup__content">
      <h1 class="popup__title">
        {{ wData.name }}
      </h1>
      <!-- <p class="popup__text">
        {{ wData.name }}
      </p> -->
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
    box-shadow: 0 2rem 4rem rgba($color-black, 0.2);
    border-radius: 3px;
    display: table; //creates a table with containt the content- right and left
    opacity: 0;
    padding: 20rem;
    transform: translate(-50%, -50%) scale(0.5);
    transition: all 0.4s 0.2s;
    z-index: 101;
  }

  &:target &__content {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }

  &__title {
    text-transform: uppercase;
  }
}
</style>