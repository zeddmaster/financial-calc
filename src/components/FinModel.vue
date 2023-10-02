<script setup>
import {ref, computed} from 'vue'
import FinSchedule from "@/components/FinSchedule.vue";


const annualReturn = .04; // константа годового дохода (4%)
const price = ref(50000); // вложения
const months = ref(60); // срок вложений (по-дефолку 5 лет)


// --- расчеты --- //

//--шаг расчетов по месяцам--//
const getStep = (time) => {
  if (time <= 12) {
    return 1
  } else if (time > 12 && time <= 36) {
    return 4
  } else {
    return 6
  }
}

// стоимость недвижимости по step месяцев
const monthPrice = computed(() => {

  const monthPrice = []

  for (let i = 0; i <= months.value; i += getStep(months.value)) {

    const profitPercent = (annualReturn / 12 * i)
    const profit = (price.value * profitPercent)


    monthPrice.push({
      month: i,
      price: price.value + profit,
      profit,
      profitPercent
    })
  }

  return monthPrice
})


// окончательный результат
const result = computed(() => {
  if (monthPrice.value.length)
    return monthPrice.value[monthPrice.value.length - 1]
  return null;
})

</script>


<template>
  <section class="fin-model">
    <div class="fin-model__wrapper">

      <div class="form">
        <div class="field">
          <input type="number" v-model.number="price" min="10">
        </div>
        <div class="field">
          <input type="number" v-model.number="months" min="5" step="5">
        </div>

        <div class="results">
          <h4>Доходность за {{ months }} мес.</h4>
          <div class="field">
            <input type="text" :value="result.price.toFixed(2)" disabled>
          </div>
          <div class="field">
            <input type="text" :value="result.profit.toFixed(2)" disabled>
          </div>
          <div class="field">
            <input type="text" :value="(result.profitPercent * 100).toFixed(2) + '%'" disabled>
          </div>
        </div>
      </div>

      <div class="diagram">
        <pre>{{ result }}</pre>
      </div>

    </div>
    <fin-schedule :propety="monthPrice"></fin-schedule>
  </section>
</template>


<style>

.fin-model {
  border: 1px solid #efefef;
  padding: 20px;
  border-radius: 7px;
  width: 100%;
  max-width: 700px;
  margin: 0 auto;
}

.fin-model__wrapper {
  display: flex;
  gap: 15px;
}

.fin-model pre {
  padding: 15px;
  background: #efefef;
  max-height: 200px;
  overflow-y: auto;
}

</style>