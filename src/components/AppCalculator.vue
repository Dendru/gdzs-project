<template>
  <div>
    <h2>Калькулятор ГДЗС</h2>
    <div>
      <p>
        Время включения в СИЗОД:
        <input
          type="time"
          class="timeInput"
          v-model="timeOnInclusion"
          required
        />
      </p>
      <p class="inputs">
        Давление газодымозащитников при включении T<sub>включ</sub>:
        <input
          type="number"
          min="260"
          max="300"
          placeholder="280"
          v-model.number="pressure1"
          required
        />
        <input
          type="number"
          min="260"
          max="300"
          placeholder="270"
          v-model.number="pressure2"
          required
        />
        <input
          type="number"
          min="260"
          max="300"
          placeholder="290"
          v-model.number="pressure3"
          required
        />
      </p>
      <p>
        Объем баллона V<sub>бал.</sub>:
        <input
          type="number"
          placeholder="6.8"
          v-model="cylinderVolume"
          required
        />
      </p>
    </div>
    <div>
      <p>
        <b>1. Рассчитать максимальное падение давления</b> - давление которое
        звено может максимально израсходовать при следовании к очагу и работе на
        месте пожара. Берется минимальное давление из переданных
        газодымозащитниками.
      </p>
      <h3>P<sub>макс.пад.</sub> = P<sub>мин.давл.включ.</sub> ÷ 3</h3>
      <h3>
        P<sub>макс.пад.</sub> = {{ minPressure }} ÷ 3 =
        {{ maxPressureDrop }} атм
      </h3>

      <p>
        <b
          >2. Рассчитать контрольное давление выходы из непригодной для дыхания
          среды</b
        >
      </p>
      <h3>
        P<sub>контр.давл.вых.</sub> = P<sub>мин.давл.включ.</sub> − P<sub
          >макс.пад.</sub
        >
      </h3>
      <h3>
        P<sub>контр.давл.вых.</sub> = {{ minPressure }} −
        {{ maxPressureDrop }} = {{ outletPressure }} атм
      </h3>

      <p>
        <b
          >3. Расчет промежутка времени с момента включения в СИЗОД до подачи
          команды постовым поста безопасности ГДЗС на возвращение звена ГДЗС из
          непригодной для дыхания среды</b
        >
      </p>
      <h3>ΔT = P<sub>макс.пад.</sub> ∙ V<sub>бал.</sub> ÷ 45</h3>
      <h3>
        ΔT = {{ maxPressureDrop }} ∙ {{ cylinderVolume }} ÷ 45 =
        {{ deltaTime }} мин
      </h3>

      <p>
        <b
          >4. Время подачи команды постовым поста безопасности ГДЗС на
          возвращение звена ГДЗС из непригодной для дыхания среды</b
        >
      </p>
      <h3>T<sub>вых</sub> = T<sub>включ</sub> + ΔT</h3>
      <h3>
        T<sub>вых</sub> = {{ timeOnInclusion }} + {{ deltaTime }} =
        {{ exitTime }}
      </h3>

      <p><b>5. Общее время работы</b></p>
      <h3>
        T<sub>общ.</sub> = P<sub>мин.давл.включ.</sub> ∙ V<sub>бал.</sub> ÷ 45
      </h3>
      <h3>
        T<sub>общ.</sub> = {{ minPressure }} ∙ {{ cylinderVolume }} ÷ 45 =
        {{ totalWorkingTime }} мин
      </h3>

      <p><b>6. Ожидаемое время возвращения звена</b></p>
      <h3>T<sub>возвр.</sub> = T<sub>включ</sub> + T<sub>общ.</sub></h3>
      <h3>
        T<sub>возвр.</sub> = {{ timeOnInclusion }} + {{ totalWorkingTime }} =
        {{ timeToReturn }}
      </h3>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      pressure1: null,
      pressure2: null,
      pressure3: null,
      cylinderVolume: 6.8,
      timeOnInclusion: null
    }
  },
  methods: {
    addMinutesToTime (timeString, minutesToAdd) {
      const [hours, minutes] = timeString.split(':').map(Number)
      const date = new Date()
      date.setHours(hours)
      date.setMinutes(minutes)
      date.setSeconds(0)

      date.setMinutes(date.getMinutes() + minutesToAdd)
      const result = `${date.getHours().toString().padStart(2, '0')}:${date
        .getMinutes()
        .toString()
        .padStart(2, '0')}`
      return result
    }
  },
  computed: {
    minPressure () {
      return Math.min(this.pressure1, this.pressure2, this.pressure3)
    },
    maxPressureDrop () {
      return Math.ceil(this.minPressure / 3)
    },
    outletPressure () {
      return this.minPressure - this.maxPressureDrop
    },
    deltaTime () {
      return Math.floor((this.maxPressureDrop * this.cylinderVolume) / 45)
    },
    totalWorkingTime () {
      return Math.floor((this.minPressure * this.cylinderVolume) / 45)
    },
    exitTime () {
      if (!this.timeOnInclusion || !this.deltaTime) return ''
      return this.addMinutesToTime(this.timeOnInclusion, this.deltaTime)
    },
    timeToReturn () {
      if (!this.timeOnInclusion || !this.totalWorkingTime) return ''
      return this.addMinutesToTime(this.timeOnInclusion, this.totalWorkingTime)
    }
  }
}
</script>

<style scoped>
.timeInput {
  margin-left: 5px;
  width: 70px;
}
input {
  width: 40px;
  margin-left: 8px;
}
p {
  text-align: left;
}
b {
  font-weight: 450;
}
</style>
