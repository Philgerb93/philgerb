<template>
  <div class="contact" id="contact">
    <div class="page-section">
      <h2>Support</h2>

      <form ref="formEl" class="box" @submit.prevent="sendEmail">
        <template v-if="!messageSent">
          <div class="info">
            <h3 class="with-subheader">Need help?</h3>
            <p>Send me a message and I’ll help you out. I usually answer within a day or two.</p>
            <div class="field">
              <label for="name">Name</label>
              <div class="error" v-if="submitted && !name">Field is required</div>
              <input v-model="name" type="text" id="name" name="user_name" placeholder="Your name" />
            </div>
            <div class="field">
              <label for="email">Email</label>
              <div class="error" v-if="submitted && !email">Field is required</div>
              <div class="error" v-if="submitted && email && !validEmail">Must be a valid email</div>
              <input v-model="email" type="text" id="email" name="user_email" placeholder="you@email.com" />
            </div>
            <div class="field">
              <label for="supportType">Support type</label>
              <div class="error" v-if="submitted && !supportType">Field is required</div>
              <select v-model="supportType" id="supportType" name="support_type">
                <option disabled value="">Select one</option>
                <option>Question</option>
                <option>Technical issue</option>
                <option>Feedback</option>
                <option>Other</option>
              </select>
            </div>
          </div>
          <div class="content">
            <div class="field">
              <label for="subject">Subject</label>
              <div class="error" v-if="submitted && !subject">Field is required</div>
              <input v-model="subject" type="text" id="subject" name="subject" placeholder="What is this about?" />
            </div>
            <div class="field message-field">
              <label for="message">Message</label>
              <div class="error" v-if="submitted && !message">Field is required</div>
              <textarea v-model="message" id="message" name="message" rows="4" cols="50" placeholder="Describe your issue or question in detail..."></textarea>
            </div>
            <button type="submit" :disabled="loading">
              <span v-if="!loading">SEND SUPPORT REQUEST</span>
              <span v-else>SENDING…</span>
            </button>
          </div>
        </template>

        <div v-else class="confirmation">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
          </svg>
          <p>Your support request has been sent!</p>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import emailjs from '@emailjs/browser'

const formEl      = ref(null)
const name        = ref('')
const email       = ref('')
const supportType = ref('')
const subject     = ref('')
const message     = ref('')
const submitted   = ref(false)
const loading     = ref(false)
const messageSent = ref(false)

const validEmail = computed(() => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value))

function isValid() {
  return name.value && email.value && validEmail.value && supportType.value && subject.value && message.value
}

async function sendEmail() {
  submitted.value = true
  if (!isValid()) return
  loading.value = true
  try {
    // Replace the three strings below with your own EmailJS credentials
    await emailjs.sendForm(
      'service_1jm0wev',
      'template_lpmj7bp',
      formEl.value,
      'user_F566qi3f7sd2W3JcQOrgV'
    )
    messageSent.value = true
  } catch (err) {
    console.error('EmailJS error:', err)
  } finally {
    loading.value = false
  }
}

function watchForAnim() {
  document.querySelectorAll('.box').forEach(el => {
    if (!el.classList.contains('slided') && elemInViewport(el)) el.classList.add('slided')
  })
}
function elemInViewport(el) {
  const navH   = document.querySelector('.nav')?.clientHeight ?? 0
  const top    = el.getBoundingClientRect().top
  const center = top + el.clientHeight / 2
  return center > navH && center < window.innerHeight
}

onMounted(() => { watchForAnim(); window.addEventListener('scroll', watchForAnim) })
onUnmounted(() => window.removeEventListener('scroll', watchForAnim))
</script>

<style lang="scss" scoped>
@use 'sass:color';
@use '../global.scss' as *;

.contact {
//   background-color: rgba($color-accent-dark, 0.03);
}

.box {
  text-align: start;
  background-color: $color-dark;
  max-width: 500px;
  min-height: 500px;
  display: flex;
  margin: 0 auto;
  color: $color-light;
  flex-direction: column;
  box-shadow: 0 16px 38px -12px rgba(0,0,0,.56),
              0 4px 25px 0px rgba(0,0,0,.12),
              0 8px 10px -5px rgba(0,0,0,.2);
  opacity: 0;

  &.slided { animation: slideInUp 1s forwards; }

  @include width-above(820px) {
    flex-direction: row;
    max-width: 1000px;
  }

  .info {
    flex: 2;
    padding: 0 60px;
    margin-top: 60px;

    @include width-above(820px) {
      padding-right: 40px;
      margin-bottom: 50px;
    }

    h3 { margin-left: 0; text-align: start; letter-spacing: 0.1em; }
    p  { margin-bottom: 5rem; }
  }

  .content {
    flex: 3;
    display: flex;
    flex-direction: column;
    margin-top: 8px;

    @include width-above(820px) {
      border-left: 1px solid rgba($color-light, 0.3);
      margin-top: 60px;
    }

    .field {
      padding: 0 60px;
      margin-bottom: 64px;

      @include width-above(820px) {
        border-left: 1px solid rgba($color-light, 0.3);
        padding: 20px;
        margin-bottom: 0;
      }

      textarea {
        width: 100%;
        min-height: 160px;
      }
    }

    .message-field {
      flex: 1;
      display: flex;
      flex-direction: column;
      min-height: 0;
      justify-content: flex-start;

      textarea {
        flex: 1;
        min-height: 0;
        min-height: 220px;
      }
    }

    button {
      height: 50px;
      border: none;
      background-color: $color-brand;
      color: $color-light;
      font-weight: bold;
      transition: 0.2s all;
      font-size: 1.4rem;
      letter-spacing: 0.1em;

      &:not(:disabled):hover { background-color: color.adjust($color-brand, $lightness: 10%); }
    }
  }

  .field {
    display: flex;
    flex-direction: column;
    margin-bottom: 3rem;

    label {
      opacity: 0.6;
      font-size: 1.6rem;
    }

    input, textarea, select {
      caret-color: $color-light;
      border: none;
      background-color: transparent;
      resize: none;
      font-size: 1.6rem;
      color: $color-light;
      margin-bottom: 4px;
      &:focus { outline: none; }
    }

    select {
      height: 40px;
      border-bottom: 1px solid rgba($color-light, 0.3);
      color: $color-light;
      appearance: none;
      -webkit-appearance: none;
      -moz-appearance: none;
    }

    input {
      height: 40px;
      border-bottom: 1px solid rgba($color-light, 0.3);
    }
    textarea { padding: 8px 0; }
  }
}

.confirmation {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: color.adjust($color-brand, $lightness: 10%);
  width: 100%;
  height: 500px;

  svg { width: 80px; height: 80px; }
}

.error {
  font-size: 1.4rem;
  color: rgba(red, 0.8);
}
</style>
