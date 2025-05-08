<script setup>
import { ref, watch } from 'vue'

const question = ref('')
const answer = ref('Questions usually contain a question mark. ;-)')
const imageUrl = ref('')
const loading = ref(false)



// watch works directly on a ref
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    imageUrl.value = ''
    try {
      const res = await fetch('https://yesno.wtf/api')
      const data = await res.json()
      answer.value = data.answer
      imageUrl.value = data.image
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
      imageUrl.value = ''
    } finally {
      loading.value = false
    }
  }
})
</script>

<template>
  <p>
    Ask a yes/no question:
    <input v-model="question" :disabled="loading" />
  </p>
  <p>{{ answer }}</p>
  <p v-if="imageUrl">
    <img :src="imageUrl"/>
  </p>
</template>