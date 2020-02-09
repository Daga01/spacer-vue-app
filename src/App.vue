<template>
  <div :class="[{ flexStart: step === 1 }, 'wrapper']">
    <transition name="slide">
      <img src="./assets/logo.svg" class="logo" v-if="step === 1">
    </transition>
    <transition name="fade">
      <HeroImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1"/>
    <div class="results" v-if="results && !loading && step === 1">
      <Item v-for="item in results" :item="item"
      :key="item.data[0].nasa_id" @click.native="handleModalOpen(item)" />
    </div>
    <div class="loader" v-if="step === 1 && loading">
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
    </div>
    <Modal v-if="modalOpen" :item="modalItem" @closeModal="modalOpen = false"/>
  </div>
</template>
<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import Claim from '@/components/Claim.vue';
import SearchInput from '@/components/SearchInput.vue';
import HeroImage from '@/components/HeroImage.vue';
import Item from '@/components/Item.vue';
import Modal from './components/Modal.vue';

const API = 'https://images-api.nasa.gov/search';
export default {
  name: 'App',
  components: {
    Claim,
    SearchInput,
    HeroImage,
    Item,
    Modal,
  },
  data() {
    return {
      loading: false,
      step: 0,
      searchValue: '',
      results: [],
      modalOpen: false,
      modalItem: null,
    };
  },
  methods: {
    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalItem = item;
    },
    handleInput: debounce(function () {
      this.loading = true;
      console.log(this.searchValue);
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          console.log(response);
          this.results = response.data.collection.items;
          this.loading = false;
          this.step = 1;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&display=swap');

* {
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: Montserrat, sans-serif;
  margin: 0;
  padding: 0;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .3s ease;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.slide-enter-active, .slide-leave-active {
  transition: margin-top .3s ease;
}

.slide-enter, .slide-leave-to {
  margin-top: -50px;
}

.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 30px;
  width: 100%;
  min-height: 100vh;
  position: relative;

  &.flexStart {
    justify-content: flex-start;
  }
}

.logo {
  position: absolute;
  top: 30px;
}

.results {
  margin-top: 50px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px;

  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr 1fr;
  }

}

.loader {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
  margin-top: 100px;

}
.loader div {
  animation: loader 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  transform-origin: 40px 40px;
}
.loader div:after {
  content: " ";
  display: block;
  position: absolute;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #1e3d4a;
  margin: -4px 0 0 -4px;
}
.loader div:nth-child(1) {
  animation-delay: -0.036s;
}
.loader div:nth-child(1):after {
  top: 63px;
  left: 63px;
}
.loader div:nth-child(2) {
  animation-delay: -0.072s;
}
.loader div:nth-child(2):after {
  top: 68px;
  left: 56px;
}
.loader div:nth-child(3) {
  animation-delay: -0.108s;
}
.loader div:nth-child(3):after {
  top: 71px;
  left: 48px;
}
.loader div:nth-child(4) {
  animation-delay: -0.144s;
}
.loader div:nth-child(4):after {
  top: 72px;
  left: 40px;
}
.loader div:nth-child(5) {
  animation-delay: -0.18s;
}
.loader div:nth-child(5):after {
  top: 71px;
  left: 32px;
}
.loader div:nth-child(6) {
  animation-delay: -0.216s;
}
.loader div:nth-child(6):after {
  top: 68px;
  left: 24px;
}
.loader div:nth-child(7) {
  animation-delay: -0.252s;
}
.loader div:nth-child(7):after {
  top: 63px;
  left: 17px;
}
.loader div:nth-child(8) {
  animation-delay: -0.288s;
}
.loader div:nth-child(8):after {
  top: 56px;
  left: 12px;
}
@keyframes loader {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
