<script setup>
import { onMounted, ref } from 'vue'
import { RouterLink, RouterView } from 'vue-router'

const random_num = ref(86)
const main_el = ref(false)
const result = ref(false)
const showStartBtn = ref(true)
const result_num = ref(0)

const animateButton = (e) => {
  e.preventDefault
  e.target.classList.remove('animate')

  e.target.classList.add('animate')

  setTimeout(function () {
    closeResult('AGAIN')
    main_el.value = true
    showStartBtn.value = false
    result_num.value = getRandom(random_num.value)
  }, 700)
  setTimeout(function () {
    e.target.classList.remove('animate')
  }, 700)
  setTimeout(function () {
    main_el.value = false
    result.value = true
  }, 2600)
}

const getRandom = (x) => {
  return Math.floor(Math.random() * x) + 1
}

const closeResult = (type) => {
  if (type !== 'AGAIN') {
    showStartBtn.value = true
  }
  result.value = false
  result_num.value = 0
}
</script>

<template>
  <img id="main_bg" src="./assets/mainbg.png" />
  <div id="main_el" :class="main_el ? 'animate' : ''"></div>
  <div id="main_small_area">
    <div>
      <img
        class="main_small"
        id="main_el_01"
        :class="main_el ? 'animate' : ''"
        src="./assets/main_el_01.png"
      />
      <img
        class="main_small"
        id="main_el_02"
        :class="main_el ? 'animate' : ''"
        src="./assets/main_el_02.png"
      />
      <img
        class="main_small"
        id="main_el_03"
        :class="main_el ? 'animate' : ''"
        src="./assets/main_el_03.png"
      />
      <img
        class="main_small"
        id="main_el_04"
        :class="main_el ? 'animate' : ''"
        src="./assets/main_el_04.png"
      />
    </div>
  </div>

  <button
    :style="!showStartBtn ? 'opacity: 0' : ''"
    class="bubbly_button"
    id="bubbly_button"
    @click="animateButton"
  >
    Click me!
  </button>
  <div id="result" :class="result ? 'animate' : ''">
    <img
      id="close"
      @click="closeResult"
      src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAAACXBIWXMAAAsTAAALEwEAmpwYAAABcUlEQVR4nO3ZQU7DMBAFUN+C2GbBgntUHUeIPQHU+eHsLDgDQgIkkFHTDZaaOvbYifyl7qrMPDtpHVuplpaWlk1l3F3fguxQqj7IDr6HRRd53l3dwJlXkPnmffeihIPePMGZL3b6LRpzQjjz8/cRxuCImOpHY5jso2/+BDliDk5DZc7BaYRq+56SjIrEzCBXTUkMcteSwIgNGDIWEr+FkaFgiecweeFiiJQNFEekaKQaxJKGqkPENFYt4pIGq0fMaXQ1iHOLvVKLz0UJjv5aZmI2Zk2IzUCwhVuLe82rf9ixhZ9f9u/3gUZB3Th9p3oMz0BUj+ELENViOAJRHQZkh1hENRgkQJR/X6cwYsl/gvwOCqVHyO9pUT6E3C4j2YGd/pRYYiAXZnTdQwjh11QqU5AaE7yd/KFL7NZ+imOFPmIAsdd37PSH1EycPegh847euLiLTRhhxL+jtyWIKaPr7ksehnKvedxbKlW/paWlRWXJLypMyUcg0fgmAAAAAElFTkSuQmCC"
    />
    <img id="res_main_el" src="./assets/main_el_01.png" />
    <div id="res_num" v-if="result_num !== 85 && result_num !== 86">{{ result_num }}</div>
    <div id="res_head_num" v-if="result_num === 85"></div>
    <div id="res_first_num" v-if="result_num === 86"></div>
    <button v-if="result" class="bubbly_button again_button" @click="animateButton">Again!</button>
  </div>
</template>
