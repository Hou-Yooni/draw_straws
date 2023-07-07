<script setup>
import { ref, reactive, computed } from 'vue'
import NUMBERS from './data/numbers'
import GODS from './data/gods'

//抽籤
const showStraws = ref(false)
const random_num = ref(85)
const main_el = ref(false)
const result = ref(false)
const showStartBtn = ref(true)
const result_num = ref(0)
const result_num_record = ref(0)
//擲筊
const random_num_2 = ref(3)
const result_num_2 = ref(0)
const result_2 = ref(false)
const showToss = ref(true)
const showTossBtn = ref(true)
//擲筊(2)
const tossStatus = ref(0)
const isHaveTossNum = ref(3)
const isProcess = ref(true)
const openTossPaper = ref(false)
//擲筊(2)流程紀錄
const recordResultTossNum = ref(null)
const processResultIndex = ref(null)
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

const loadingTime = ref(false)
const isFirstAgain = ref(false)

const animateButton = (e) => {
  e.preventDefault
  e.target.classList.remove('animate')

  e.target.classList.add('animate')

  setTimeout(function () {
    closeResult('AGAIN')
    main_el.value = true
    showStartBtn.value = false
    result_num.value = getRandom(random_num.value)
    result_num_record.value = result_num.value
  }, 700)
  setTimeout(function () {
    e.target.classList.remove('animate')
  }, 700)
  setTimeout(function () {
    main_el.value = false
    result.value = true
  }, 2600)
}

const animateButton_2 = (e, type) => {
  if (type === 'AGAIN') {
    isFirstAgain.value = true
  }
  result_2.value = false
  showToss.value = true
  loadingTime.value = false
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
  }, 1100)

  setTimeout(() => {
    loadingTime.value = true
  }, 1900)
}

const animateButton_3 = (e) => {
  result_2.value = false
  showToss.value = true
  isProcess.value = false
  loadingTime.value = false
  e.preventDefault
  e.target.classList.remove('animate')

  e.target.classList.add('animate')

  setTimeout(function () {
    e.target.classList.remove('animate')
    showTossBtn.value = false

    if (processResultTossArray.value.length === 0) {
      const index = getRandom(12) - 1
      processResultTossArray.value = [...processResultToss[index]]
      processResultIndex.value = index
    }
    result_num_2.value = processResultTossArray.value[0]
    processResultTossArray.value.shift()
  }, 700)

  // setTimeout(function () {
  //   showToss.value = false
  //   result_2.value = true
  //   isProcess.value = true
  //   if (result_num_2.value !== 2) {
  //     showTossBtn.value = true
  //   } else {
  //     showTossBtn.value = false
  //   }
  //   if (tossStatus.value === 1 && isHaveTossNum.value !== 0) {
  //     isHaveTossNum.value--
  //   }
  // }, 950)

  setTimeout(function () {
    showToss.value = false
    result_2.value = true
    isProcess.value = true
    showTossBtn.value = true
    if (tossStatus.value === 1 && isHaveTossNum.value !== 0) {
      isHaveTossNum.value--
    }
  }, 1100)

  setTimeout(() => {
    loadingTime.value = true
  }, 1800)
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
  loadingTime.value = false

  processResultTossArray.value = []
}

const toss2Next = () => {
  tossStatus.value = 1
  showStraws.value = false
  showToss.value = true
  showTossBtn.value = true
  result.value = false
  result_num.value = 0
}

const openResult3 = () => {
  openTossPaper.value = true
  result.value = true
}

const reload = () => {
  window.location.reload()
}

const calcString = computed(() => {
  return function value(s) {
    let style = null
    switch (s?.length) {
      case 2:
        style = 'top:38%;font-size: 55px;letter-spacing: 22px;'
        break
      case 3:
        style = 'top:29%;font-size: 55px;letter-spacing: 22px;'
        break
      case 4:
        style = 'top:24%;font-size: 55px;letter-spacing: 12px;'
        break
      case 5:
        style = 'top:24%;font-size: 48px;letter-spacing: 3px;'
        break
      case 7:
        style = 'top:24%;font-size: 35px;letter-spacing: 1px;'
        break
      case 8:
        style = 'top:23%;font-size: 33px;letter-spacing: 0px;'
        break
      case 9:
        style = 'top:23%;font-size: 30px;letter-spacing: 0px;'
        break
      case 10:
        style = 'top:22%;font-size: 27px;letter-spacing: 0px;'
        break
      case 11:
        style = 'top:22%;font-size: 25px;letter-spacing: 0px;'
        break
      default:
        break
    }
    return style
  }
})
</script>

<template>
  <img id="main_bg" src="./assets/mainbg.png" />
  <div id="main_el2_area">
    <div v-if="showToss" id="main_el2_01" :class="!showTossBtn ? 'animate' : ''"></div>
    <div v-if="showToss" id="main_el2_02" :class="!showTossBtn ? 'animate' : ''"></div>
    <!-- <div v-if="result_2 && result_num_2 === 1" id="main_result_el2_01"></div>
    <div v-if="result_2 && result_num_2 === 2" id="main_result_el2_02"></div>
    <div v-if="result_2 && result_num_2 === 3" id="main_result_el2_03"></div> -->

    <img
      v-show="result_2 && result_num_2 === 1"
      id="main_result_el2_01"
      src="./assets/main_el2_03.png"
    />
    <img
      v-show="result_2 && result_num_2 === 2"
      id="main_result_el2_02"
      src="./assets/main_el2_04.png"
    />
    <img
      v-show="result_2 && result_num_2 === 3"
      id="main_result_el2_03"
      src="./assets/main_el2_05.png"
    />
  </div>
  <button
    v-if="showTossBtn && tossStatus == 0 && !result_2 && !isFirstAgain"
    class="bubbly_button"
    @click="animateButton_2"
  >
    {{ 'Click me!' }}
  </button>
  <button
    v-if="showTossBtn && tossStatus == 0 && result_2 && loadingTime"
    class="bubbly_button"
    @click="animateButton_2($event, 'AGAIN')"
    style="position: absolute; bottom: 28%"
  >
    {{ 'Again !' }}
  </button>
  <button
    v-if="result_2 && result_num_2 === 2 && tossStatus !== 1 && loadingTime"
    class="bubbly_button"
    @click="toss1Next"
    style="position: absolute; bottom: 28%"
  >
    Next
  </button>
  <button
    v-show="isProcess"
    v-if="
      isHaveTossNum === 0
        ? tossStatus == 1 && (result_num_2 === 0 || (result_num_2 === 2 && isHaveTossNum !== 0))
        : tossStatus == 1 && (result_num_2 === 0 || (result_num_2 === 2 && loadingTime))
    "
    class="bubbly_button"
    @click="animateButton_3"
    style="position: absolute; bottom: 28%"
  >
    {{ `Start` }}
  </button>
  <button
    v-if="tossStatus == 1 && result_num_2 !== 2 && result_num_2 !== 0 && loadingTime"
    class="bubbly_button"
    @click="toss1Next"
    style="position: absolute; bottom: 28%"
  >
    {{ '重新抽籤' }}
  </button>
  <button
    v-if="isHaveTossNum === 0 && processResultIndex === 0"
    class="bubbly_button"
    @click="openResult3"
    style="position: absolute; bottom: 28%"
  >
    {{ 'Open !' }}
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
      @click="reload"
      src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAAACXBIWXMAAAsTAAALEwEAmpwYAAABcUlEQVR4nO3ZQU7DMBAFUN+C2GbBgntUHUeIPQHU+eHsLDgDQgIkkFHTDZaaOvbYifyl7qrMPDtpHVuplpaWlk1l3F3fguxQqj7IDr6HRRd53l3dwJlXkPnmffeihIPePMGZL3b6LRpzQjjz8/cRxuCImOpHY5jso2/+BDliDk5DZc7BaYRq+56SjIrEzCBXTUkMcteSwIgNGDIWEr+FkaFgiecweeFiiJQNFEekaKQaxJKGqkPENFYt4pIGq0fMaXQ1iHOLvVKLz0UJjv5aZmI2Zk2IzUCwhVuLe82rf9ixhZ9f9u/3gUZB3Th9p3oMz0BUj+ELENViOAJRHQZkh1hENRgkQJR/X6cwYsl/gvwOCqVHyO9pUT6E3C4j2YGd/pRYYiAXZnTdQwjh11QqU5AaE7yd/KFL7NZ+imOFPmIAsdd37PSH1EycPegh847euLiLTRhhxL+jtyWIKaPr7ksehnKvedxbKlW/paWlRWXJLypMyUcg0fgmAAAAAElFTkSuQmCC"
    />
    <img id="res_main_el" v-if="!openTossPaper" src="./assets/main_el_01.png" />
    <div id="res_num" v-if="result_num !== 85 && result_num !== 1 && !openTossPaper">
      {{ NUMBERS[result_num - 1] }}
    </div>
    <div id="res_head_num" v-if="result_num === 85 && !openTossPaper"></div>
    <div id="res_first_num" v-if="result_num === 1 && !openTossPaper"></div>

    <div
      id="res_paper_num"
      v-if="openTossPaper && result_num_record !== 85 && result_num_record !== 1"
    >
      {{ NUMBERS[result_num_record - 1] }}
      <p :style="calcString(GODS[result_num_record - 2])">{{ GODS[result_num_record - 2] }}</p>
    </div>

    <div id="res_head_paper_num" v-if="openTossPaper && result_num_record === 85"></div>
    <div id="res_first_paper_num" v-if="openTossPaper && result_num_record === 1"></div>
    <button v-if="result && !openTossPaper" class="bubbly_button again_button" @click="toss2Next">
      Start !
    </button>
    <button
      v-if="result && openTossPaper"
      class="bubbly_button again_button openTossPaper"
      @click="reload"
    >
      Again !
    </button>
  </div>
</template>
