<template>
  <div class="bg-[#EFF1F5] dark:bg-[#3F455D] max-w-sm rounded-2xl">
    <div class="flex justify-between p-8">
      <button>
        <IconArrowLeft class="w-6 h-6" />
      </button>

      <h1 class="text-lg font-semibold">Empréstimos</h1>

      <button>
        <IconMore class="w-6 h-6" />
      </button>
    </div>

    <section class="m-8 bg-white dark:bg-[#363B4F] px-4 py-3.5 border border-[#29BBCF] rounded-lg">
      <h1 class="text-xs text-center text-[#555555] dark:text-[#ADB1B8]">Maximum Funding</h1>
      <h2 class="mt-1.5 text-3xl font-semibold text-center text-[#3F455D] dark:text-white">
        Rp {{ maximumFundingFormatted }}
      </h2>
      <hr class="bg-[#9BA1AA] border-none h-px opacity-30 mt-4 mb-4" />
      <div class="flex justify-between items-center">
        <div>
          <h1 class="text-sm">Honda ADV 150 CBS</h1>
          <span class="text-xs text-[#9299B5]">2022</span>
        </div>

        <button>
          <IconFilter class="w-5 h-5 text-[#ADB1B8]" />
        </button>
      </div>
    </section>

    <section class="mt-6 mx-8 bg-white dark:bg-[#363B4F] px-4 py-3.5 rounded-lg">
      <h1 class="text-xs text-[#555555] dark:text-[#ADB1B8]">Loan Amount</h1>
      <h2 class="text-lg font-semibold mt-1">Rp {{ amountFormatted }}</h2>
      <input
        type="range"
        class="w-full accent-[#29BBCF]"
        :value="state.amount"
        @input="updateAmount"
        :min="minimumAmount"
        :max="maximumAmount"
      />
      <div class="flex justify-between items-center">
        <span class="text-xs text-[#9299B5]">{{ minimumAmountFormatted }}</span>
        <span class="text-xs text-[#9299B5]">{{ maximumAmountFormatted }}</span>
      </div>
    </section>

    <section class="mt-6 mx-8 bg-white dark:bg-[#363B4F] px-4 py-3.5 rounded-lg">
      <h1 class="text-xs text-[#555555] dark:text-[#ADB1B8]">Loan Period</h1>
      <h2 class="text-lg font-semibold mt-1">{{ state.period }} Months</h2>
      <input
        type="range"
        class="w-full accent-[#29BBCF]"
        :value="state.period"
        @input="updatePeriod"
        :min="minimumPeriod"
        :max="maximumPeriod"
      />
      <div class="flex justify-between items-center">
        <span class="text-xs text-[#9299B5]">{{ minimumPeriod }} Months</span>
        <span class="text-xs text-[#9299B5]">{{ maximumPeriod }} Months</span>
      </div>
    </section>

    <div class="py-6 px-8">
      <h1 class="text-xs">Estimated monthly installments</h1>
      <h2 class="mt-0.5 text-3xl font-semibold">Rp {{ monthlyPayment }}</h2>
      <p class="mt-3.5 text-xs text-[#555555] dark:text-[#ADB1B8]">
        Installment fees may change according to the results of the verification of the physical
        condition of the vehicle at the branch office.
      </p>
      <button class="mt-8 rounded-lg p-3 bg-[#29BCCF] hover:bg-[#1FA5B8] w-full" @click="submit">
        Appy Loan
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import IconArrowLeft from './icons/ArrowLeft.vue'
import IconMore from './icons/More.vue'
import IconFilter from './icons/Filter.vue'
import { computed, reactive } from 'vue'

const maximumFunding = 17484500

const minimumAmount = 1000000
const maximumAmount = maximumFunding

const minimumPeriod = 6
const maximumPeriod = 18

// Props
defineProps<{
  title?: string
}>()

// States
const state = reactive({
  amount: minimumAmount,
  period: minimumPeriod
})

// Methods
const updateAmount = (event: Event) => {
  const target = event.target as HTMLInputElement
  state.amount = Number(target.value)
}

const updatePeriod = (event: Event) => {
  const target = event.target as HTMLInputElement
  state.period = Number(target.value)
}

const submit = () => {
  console.log('SUBMITTED')
}

const formatNumber = (number: number) => {
  return Intl.NumberFormat('pt-BR', {
    minimumFractionDigits: 0
  }).format(number)
}

// Computeds
const amountFormatted = computed(() => {
  return formatNumber(state.amount)
})

const minimumAmountFormatted = computed(() => {
  return formatNumber(minimumAmount)
})

const maximumAmountFormatted = computed(() => {
  return formatNumber(maximumAmount)
})

const maximumFundingFormatted = computed(() => {
  return formatNumber(maximumFunding)
})

const monthlyPayment = computed(() => {
  return formatNumber(Number((state.amount / state.period).toPrecision(2)))
})
</script>
