<script setup>
import { onMounted, ref, reactive } from 'vue'
import { RouterLink, RouterView } from 'vue-router'

//抽籤
const showStraws = ref(false)
const random_num = ref(84)
const main_el = ref(false)
const result = ref(false)
const showStartBtn = ref(true)
const result_num = ref(0)
//擲筊
const random_num_2 = ref(3)
const result_num_2 = ref(0)
const result_2 = ref(false)
const showToss = ref(true)
const showTossBtn = ref(true)
//擲筊(2)
const tossStatus = ref(0)
const result_3 = reactive([])
const isHaveTossNum = ref(3)
//擲筊(2)流程紀錄
const processResultIndex = ref()
const processResultTossArray = ref([])
const processResultToss = reactive([
  [2, 2, 2],
  [1, 2, 3],
  [2, 3, 1],
  [3, 2, 1],
  [1, 2, 1],
  [1, 1, 2],
  [2, 1, 1],
  [2, 2, 1],
  [2, 2, 3],
  [3, 2, 3],
  [3, 3, 1],
  [3, 3, 2]
])

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

const animateButton_2 = (e) => {
  result_2.value = false
  showToss.value = true
  e.preventDefault
  e.target.classList.remove('animate')

  e.target.classList.add('animate')

  setTimeout(function () {
    e.target.classList.remove('animate')
    showTossBtn.value = false
    result_num_2.value = getRandom(random_num_2.value)
  }, 700)

  setTimeout(function () {
    if (result_num_2.value !== 2) {
      showTossBtn.value = true
    } else {
      showTossBtn.value = false
    }
    if (tossStatus.value === 1) {
      isHaveTossNum.value--
    }
    showToss.value = false
    result_2.value = true
  }, 1200)
}

const animateButton_3 = (e) => {
  result_2.value = false
  showToss.value = true
  e.preventDefault
  e.target.classList.remove('animate')

  e.target.classList.add('animate')

  setTimeout(function () {
    e.target.classList.remove('animate')
    showTossBtn.value = false
    if (processResultTossArray.value.length === 0) {
      const index = getRandom(12) - 1
      processResultTossArray.value = processResultToss[index]
      processResultIndex.value = index
    }
    result_num_2.value = processResultTossArray.value[0]
    processResultTossArray.value.shift()
    console.log(processResultTossArray, processResultIndex)
  }, 700)

  setTimeout(function () {
    if (result_num_2.value !== 2) {
      showTossBtn.value = true
    } else {
      showTossBtn.value = false
    }
    if (tossStatus.value === 1 && isHaveTossNum.value !== 0) {
      isHaveTossNum.value--
    }
    if (isHaveTossNum.value === 0 && processResultIndex.value !== 0) {
      console.log('No')
    } else if (isHaveTossNum.value === 0 && processResultIndex.value === 0) {
      console.log('Yes')
    }
    showToss.value = false
    result_2.value = true
  }, 1200)
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

const toss1Next = () => {
  tossStatus.value = 0
  showStraws.value = true
  showToss.value = false
  showTossBtn.value = false
  showStartBtn.value = true
  result_2.value = false
  result_num_2.value = 0
  isHaveTossNum.value = 3
}

const toss2Next = () => {
  tossStatus.value = 1
  showStraws.value = false
  showToss.value = true
  showTossBtn.value = true
  result.value = false
  result_num.value = 0
}
</script>

<template>
  <img id="main_bg" src="./assets/mainbg.png" />
  <div id="main_el2_area">
    <div v-if="showToss" id="main_el2_01" :class="!showTossBtn ? 'animate' : ''"></div>
    <div v-if="showToss" id="main_el2_02" :class="!showTossBtn ? 'animate' : ''"></div>
    <div v-if="result_2 && result_num_2 === 1" id="main_result_el2_01"></div>
    <div v-if="result_2 && result_num_2 === 2" id="main_result_el2_02"></div>
    <div v-if="result_2 && result_num_2 === 3" id="main_result_el2_03"></div>
  </div>
  <button v-if="showTossBtn && tossStatus == 0" class="bubbly_button" @click="animateButton_2">
    {{ !result_2 ? 'Click me!' : 'Again!' }}
  </button>
  <button
    v-if="result_2 && result_num_2 === 2 && tossStatus !== 1"
    class="bubbly_button"
    @click="toss1Next"
  >
    Next
  </button>
  <button
    v-if="tossStatus == 1 && isHaveTossNum !== 0"
    class="bubbly_button"
    @click="animateButton_3"
  >
    {{ `Start (${isHaveTossNum})` }}
  </button>
  <button v-if="isHaveTossNum === 0" class="bubbly_button" @click="toss1Next">
    {{ '重新抽籤' }}
  </button>

  <div v-if="showStraws" id="main_el" :class="main_el ? 'animate' : ''"></div>
  <div v-if="showStraws" id="main_small_area">
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
    v-if="showStraws"
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
    <button v-if="result" class="bubbly_button again_button" @click="toss2Next">Next</button>
  </div>
</template>
