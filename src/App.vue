<script setup lang="ts">
import { ref } from 'vue'

const array = ref<number[]>([])
const inputnum = ref<number | null>(null)
const gameStart = ref(true)
const waterarray = ref<number[]>([])

const addItem = () => {
  if (!inputnum.value) {
    array.value.push(0) // Push a default value of 0
    inputnum.value = null
    return
  }
  array.value.push(Number(inputnum.value))
  inputnum.value = null
}
const toggle = () => {
  if (gameStart.value) {
    waterarray.value = trapRainWater(array.value)
  } else {
    array.value.length = 0
  }

  gameStart.value = !gameStart.value
}

const trapRainWater = (heights: number[]) => {
  let left = 0,
    right = heights.length - 1
  let maxLeft = 0,
    maxRight = 0
  let trappedWater = 0
  let waterArray = new Array(heights.length).fill(0)

  while (left < right) {
    if (heights[left] < heights[right]) {
      if (heights[left] >= maxLeft) {
        maxLeft = heights[left]
      } else {
        trappedWater = maxLeft - heights[left]
        waterArray[left] = trappedWater
      }
      left++
    } else {
      if (heights[right] >= maxRight) {
        maxRight = heights[right]
      } else {
        trappedWater = maxRight - heights[right]
        waterArray[right] = trappedWater
      }
      right--
    }
  }

  return waterArray
}
</script>
<template>
  <div class="start-item">
    <h1>{{ array }}</h1>
    <div class="input-group mb-3" v-if="gameStart">
      <button @click="addItem" class="btn btn-success" type="button" id="button-addon1">ADD</button>
      <input
        v-model="inputnum"
        type="number"
        class="form-control"
        placeholder="اضف رقم الي المصفوفة"
        aria-label="Example text with button addon"
        aria-describedby="button-addon1"
      />
    </div>
    <button @click="toggle" class="btn btn-success" type="button" id="button-addon1">
      {{ gameStart ? 'START' : 'AGAIN' }}
    </button>
  </div>
  <div class="mountains" v-if="!gameStart">
    <div class="mountains-height" v-for="(mountain, index) in array" :key="index">
      <div class="mountain" v-for="(m, mIndex) in mountain" :key="mIndex"></div>
      <div v-for="(water, waterIndex) in waterarray[index]" :key="waterIndex" class="water"></div>
    </div>
  </div>
</template>

<style>
button {
  background-color: greenyellow;
}
* {
  color: white;
}
body,
html {
  width: 100%;
  height: 100%;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url('https://cdn.pixabay.com/animation/2023/03/05/12/05/12-05-54-62_512.gif');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}
.form-control {
}

.mountains {
  margin-top: 100px;
  width: 90%;
  display: flex;
  flex-direction: row;
}
.mountains-height {
  display: flex;
  flex-direction: column-reverse;
}
.mountain {
  border: 20px solid rgb(110, 246, 110);
  box-shadow: #00000038 4px -4px 16px 11px;
}
.water {
  border: 20px solid rgb(85 170 254);
  box-shadow: #ffffff3b 0 0 16px 11px;
}
@media screen and (max-width: 768px) {
  .mountain {
    border: 16px solid rgb(110, 246, 110);
  }
  .water {
    border: 16px solid rgb(85 170 254);
  }
}

.start-item {
  display: flex;
  flex-direction: column;
  text-align: center;
}
</style>
