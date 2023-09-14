<script setup lang="ts">
import { reactive, ref } from 'vue';
import { isAfter, format } from 'date-fns'
import { ptBR as locale } from 'date-fns/locale'

const nextHoliday = reactive(ref<any>());

async function getHolidays() {
  const httpResponse = await fetch('https://brasilapi.com.br/api/feriados/v1/2023')
  return await httpResponse.json()
}

const currentDate = new Date()
getHolidays().then((responseHolidays) => {

  const nextHolidays = responseHolidays.filter((holiday: any) => {
    return isAfter(new Date(holiday.date), currentDate)
  })


  nextHoliday.value = nextHolidays[2]
  console.log(new Date(nextHoliday.value.date))

})
const images: any = {
  "Finados": "https://images.unsplash.com/photo-1541284875349-799cdedb0d84?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2574&q=80",
  "Nossa Senhora Aparecida": "https://images.squarespace-cdn.com/content/v1/63e64be0831faf1c806eacdb/1682713519528-2U5BBZAV5U4HMVE6G3D1/Design+sem+nome+%288%29.jpg",
  "Proclamação da República": "https://al.se.leg.br/wp-content/uploads/2022/11/proclamacao_da_republica.jpg",
  "Natal": "https://plus.unsplash.com/premium_photo-1678229915729-7e75d14a6b00?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2670&q=80",
}

</script>

<template>
  <main class="flex items-center justify-center h-screen bg-no-repeat bg-cover bg-center"
    :style="{ 'background': 'linear-gradient(rgba(0,0,0,0.8), rgba(0,0,0,0.95)), url(' + images[nextHoliday.name] + ')' }">
    <div class="flex flex-col items-center justify-center">
      <h1 class="text-xg text-gray-200 font-normal mb-4">Qual o próximo feriado?</h1>
      <h2 class="text-4xl text-white">
        {{ nextHoliday?.name }}
      </h2>
      <h3 class="text-center text-xl mt-6 text-white">
        <span class="font-bold text-2xl">
          {{ nextHoliday?.date.replace(/\-/g, '/').split('/').reverse().join('/') }}

        </span>
      </h3>
    </div>
  </main>
</template>