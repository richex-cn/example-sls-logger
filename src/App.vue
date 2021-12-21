<script setup>
import defaultData from './default-data'
import Logger from 'js-sls-logger'
import { ref } from 'vue'

const input = ref(JSON.stringify(defaultData, null, 2))

const logger = new Logger({
  host: 'cn-shenzhen.log.aliyuncs.com',
  project: 'cent',
  logstore: 'web',
  time: 1,
  count: 10,
  // compress: true
})

const log = () => {
  const data = JSON.parse(input.value)
  console.log('[ data ]', data)
  logger.send(data)
}

const bigData = () => {
  const obj = Object.assign(JSON.parse(JSON.stringify(defaultData)), {
    data: {
      test: '0'.repeat(9e5)
    }
  })
  input.value = JSON.stringify(obj, null, 2)
}
</script>

<template>
  <textarea :rows="10" v-model="input" />
  <button @click="bigData()">Write BigData</button>
  <button @click="log()">Send</button>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 22px;
}

textarea {
  display: block;
  width: 80vw;
  height: 50vh;
}
</style>
