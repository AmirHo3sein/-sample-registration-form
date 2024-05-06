<!-- Wizard.vue -->
<script setup>
import {computed, ref} from 'vue';
import BaseInput from "@/components/BaseInput.vue";

const step = ref('username');
const username = ref('');
const email = ref('');
const usernameFeedback = ref('');
const emailFeedback = ref('');
const stepDetail = ref('')
const nextStep = () => {
  if (step.value === 'username') {
    if (username.value === 'valid_username') {
      step.value = 'email'
    } else {
      usernameFeedback.value = 'Invalid Username.'
    }
  } else if (step.value === 'email') {
    if (email.value === 'user@local.test') {
      stepDetail.value = 'Step: review'
      usernameFeedback.value = 'Username: valid_username'
      emailFeedback.value = 'Email: user@local.test'
      step.value = 'review'
    } else {
      emailFeedback.value = 'Invalid email address.'
    }
  }

};
const isFirstStep = computed(() => step.value === 'username')
const isSecondStep = computed(() => step.value === 'email')
const isLastStep = computed(() => step.value === 'review')

const prevStep = () => {
  if (step.value === 'email') {
    step.value = 'username';
  } else if (step.value === 'review') {
    step.value = 'email';
  }
  emailFeedback.value = '';
  usernameFeedback.value = '';
};
</script>

<template>
  <div>
    <!-- Your wizard steps go here -->
    <div class="flex flex-col items-center mt-12">
      <div>
        <div class="mb-4 w-60 sm:w-96">
          <div class="overflow-hidden rounded-full bg-gray-200">
            <div class="h-2  rounded-full bg-blue-500 transition-all duration-300 "
                 :class="{'w-1/3':isFirstStep,'w-2/3':isSecondStep,'w-full':isLastStep}"></div>
          </div>

          <ol class="mt-4 grid grid-cols-3 text-sm font-medium text-gray-500">
            <li class="flex items-center justify-start text-blue-600 sm:gap-1.5"
                :class="{'text-blue-600':isSecondStep || isLastStep}">
              <span class="hidden sm:inline"> Username </span>

              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-5 h-5">
                <path fill-rule="evenodd"
                      d="M7.5 6a4.5 4.5 0 1 1 9 0 4.5 4.5 0 0 1-9 0ZM3.751 20.105a8.25 8.25 0 0 1 16.498 0 .75.75 0 0 1-.437.695A18.683 18.683 0 0 1 12 22.5c-2.786 0-5.433-.608-7.812-1.7a.75.75 0 0 1-.437-.695Z"
                      clip-rule="evenodd"/>
              </svg>

            </li>

            <li class="flex items-center justify-center  sm:gap-1.5" :class="{'text-blue-600':isSecondStep || isLastStep}">
              <span class="hidden sm:inline"> Email </span>
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-5 h-5">
                <path d="M1.5 8.67v8.58a3 3 0 0 0 3 3h15a3 3 0 0 0 3-3V8.67l-8.928 5.493a3 3 0 0 1-3.144 0L1.5 8.67Z"/>
                <path
                    d="M22.5 6.908V6.75a3 3 0 0 0-3-3h-15a3 3 0 0 0-3 3v.158l9.714 5.978a1.5 1.5 0 0 0 1.572 0L22.5 6.908Z"/>
              </svg>

            </li>

            <li class="flex items-center justify-end sm:gap-1.5" :class="{'text-blue-600':isLastStep}">
              <span class="hidden sm:inline"> Review </span>

              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-5 h-5">
                <path fill-rule="evenodd"
                      d="M4.5 3.75a3 3 0 0 0-3 3v10.5a3 3 0 0 0 3 3h15a3 3 0 0 0 3-3V6.75a3 3 0 0 0-3-3h-15Zm4.125 3a2.25 2.25 0 1 0 0 4.5 2.25 2.25 0 0 0 0-4.5Zm-3.873 8.703a4.126 4.126 0 0 1 7.746 0 .75.75 0 0 1-.351.92 7.47 7.47 0 0 1-3.522.877 7.47 7.47 0 0 1-3.522-.877.75.75 0 0 1-.351-.92ZM15 8.25a.75.75 0 0 0 0 1.5h3.75a.75.75 0 0 0 0-1.5H15ZM14.25 12a.75.75 0 0 1 .75-.75h3.75a.75.75 0 0 1 0 1.5H15a.75.75 0 0 1-.75-.75Zm.75 2.25a.75.75 0 0 0 0 1.5h3.75a.75.75 0 0 0 0-1.5H15Z"
                      clip-rule="evenodd"/>
              </svg>

            </li>
          </ol>
        </div>
      </div>

      <div class="relative w-60 " v-show="isFirstStep">
        <BaseInput type="text" v-model="username" name="username" id="username" label="Username:">
          <span>{{ usernameFeedback }}</span>
        </BaseInput>

      </div>
      <div class="relative w-60" v-show="isSecondStep">
        <BaseInput type="text" v-model="email" name="email" id="email" label="Email:">

          <span>{{ emailFeedback }}</span>
        </BaseInput>

      </div>
      <div class="relative w-60" v-show="isLastStep">

        <p>{{ usernameFeedback }}</p>
        <p>{{ emailFeedback }}</p>
        <p>{{ stepDetail }}</p>

      </div>

      <div class="flex justify-between gap-4 mt-4 flex-row-reverse">
        <button
            @click="nextStep" id="btn-next" :disabled="step ==='review'"
            class="align-middle select-none font-sans font-bold text-center uppercase transition-all
          disabled:opacity-50 disabled:shadow-none disabled:pointer-events-none text-xs py-3 px-6
          rounded-lg border border-gray-900 text-gray-900 hover:opacity-75 focus:ring
          focus:ring-gray-300 active:opacity-[0.85] flex items-center gap-3"
            type="button">
          Next
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"
               class="w-5 h-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M17.25 8.25L21 12m0 0l-3.75 3.75M21 12H3"></path>
          </svg>
        </button>

        <button
            @click="prevStep" id="btn-prev" :disabled="step==='username'"
            class="align-middle select-none font-sans font-bold text-center uppercase transition-all
          disabled:opacity-50 disabled:shadow-none disabled:pointer-events-none text-xs py-3 px-6
          rounded-lg border border-gray-900 text-gray-900 hover:opacity-75 focus:ring
          focus:ring-gray-300 active:opacity-[0.85] flex items-center gap-3"
            type="button">
          Prev
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
               stroke-width="2" stroke="currentColor"
               class="w-5 h-5 rotate-180">
            <path stroke-linecap="round" stroke-linejoin="round" d="M17.25 8.25L21 12m0 0l-3.75 3.75M21 12H3"></path>
          </svg>
        </button>
      </div>


    </div>


  </div>
</template>

