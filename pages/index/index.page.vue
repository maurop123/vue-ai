<template>
  <h1>Hello</h1>
  <p>
    {{ state.message }}
  </p>
  <textarea
    cols="40" rows="6"
    v-model="state.input"
    placeholder="Write your message for AI"
  >
  </textarea>
  <br><br>
  <button @click="send('user', state.input)">Send</button>
</template>

<script setup>
import OpenAI from 'openai'
import { reactive } from 'vue'
import Counter from './Counter.vue'

const state = reactive({ message: '', input: ''})
const openai = new OpenAI({
  apiKey: import.meta.env.OPENAI_APIKEY,
  organization: '',
  project: '',
  dangerouslyAllowBrowser: true,
})
/* console.debug('openai', openai) */

async function send(_role, _content) {
  const completions = await openai.chat.completions.create({
    model: 'gpt-3.5-turbo-1106',
    messages: [{
      role: _role || 'system',
      content: _content || 'You are a helpful assistant',
    }],
  })
  /* console.debug('completions', completions) */

  state.message = completions.choices[0].message.content
  state.input = ''
}

send()
</script>
