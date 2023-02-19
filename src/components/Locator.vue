<template>
  <div class="locator">
    <div class="locator__group">
      <input
        type="text"
        id="city"
        autocomplete="off"
        required
        class="locator__input"
        v-model="cityName"
        @keydown.enter="changeCityName"
        @keyup.enter="clearInput"
      />
      <span class="locator__highlight"></span>
      <span class="locator__bar"></span>
      <label for="city" class="locator__label">Enter the city name</label>
    </div>
  </div>
</template>




<script lang="ts">
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
  name: "Locator",
  data: () => {
    return {
      cityName: "" as string,
    };
  },
  methods: {
    changeCityName(e: KeyboardEvent): void {
      let target = e.target as HTMLInputElement;
      this.cityName = target.value;
      this.$emit("cityName", this.cityName);
    },
    clearInput(): void {
      this.cityName = "";
    },
  },
  mounted(): void {
    this.cityName = "";
  },
});
</script>




<style lang="scss">
.locator {
  position: relative;

  &__group {
    margin: 0 auto;
  }

  &__input {
    cursor: pointer;
    font-size: 1.6rem;
    padding: 1rem 1rem 1rem 0.5rem;
    display: block;
    width: 25rem;
    border: none;
    border-bottom: 1px solid rgba($color-grey-dark, 0.6);
    background: transparent;
    color: rgba($color-grey-dark, 0.6);

    &:focus {
      outline: none;
    }
  }

  &__label {
    color: #999999;
    font-size: 1.8rem;
    font-weight: normal;
    text-transform: uppercase;
    letter-spacing: 0.26rem;
    position: absolute;
    pointer-events: none;
    left: 0.5rem;
    top: 0.5rem;
    transition: all 0.2s ease;
  }

  &__input:focus ~ &__label,
  &__input:valid ~ &__label {
    top: -2rem;
    font-size: 1.3rem;
    letter-spacing: 0.1rem;
    color: rgb($color-primary, 1);
  }

  &__bar {
    position: relative;
    display: block;
    width: 25rem;

    &:before,
    &:after {
      content: "";
      height: 0.2rem;
      width: 0;
      position: absolute;
      bottom: 0;
      background: $color-primary;
      transition: 0.4s ease-out all;
    }

    &:before {
      left: 50%;
    }
    &:after {
      right: 50%;
    }
  }

  &__input:focus ~ &__bar:before,
  &__input:focus ~ &__bar:after {
    width: 50%;
  }

  &__highlight {
    position: absolute;
    height: 60%;
    width: 0;
    top: 25%;
    left: 0;
    pointer-events: none;
    opacity: 0.5;
  }

  &__input:focus ~ &__highlight {
    animation: input-focus 0.5s ease;
  }
}
</style>