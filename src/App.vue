<template>
  <div class="container pt-1">
    <div class="card">
      <h2>Пожарная шпаргалка</h2>
      <div v-if="introActive">
        <h4>
          Добро пожаловать на сайт, посвященный рутине простых пожарных в
          суровых условиях работы в
          <span class="crossed">ПЧ-60</span> 60 ПЧ
        </h4>
        <button class="btn intro" @click="deactiveIntro">
          Приступить в работе
        </button>
      </div>
      <div v-if="!introActive">
        <h3>{{ currentDate }}</h3>
        <h4>Сейчас на смене {{ todayShift }} караул</h4>
        <p class="text-joke">Удачного дня! И не забудь выключить свет на фасаде</p>
        <div class="scroll-menu">
          <button
            v-for="item in buttons"
            :key="item.name"
            @click="currentComponent = item.component"
            class="btn"
            :class="{ selected: currentComponent === item.component }"
          >
            {{ item.name }}
          </button>
        </div>
        <component :is="currentComponent"></component>
      </div>
    </div>
    <footer>
      <div style="text-align: right">created by Dendru</div>
    </footer>
  </div>
</template>

<script>
import AppCalculator from './components/AppCalculator.vue'
import AppInformation from './components/AppInformation.vue'
import AppRespon from './components/AppRespon.vue'
import AppCheck1 from './components/AppCheck1.vue'
import AppAcceptance from './components/AppAcceptance.vue'
import AppMapHydrants from './components/AppMapHydrants.vue'

export default {
  components: {
    AppCalculator,
    AppInformation,
    AppRespon,
    AppCheck1,
    AppAcceptance,
    AppMapHydrants
  },
  data () {
    return {
      startDate: new Date('2025-10-22T09:15:00'),
      totalShifts: 4,
      introActive: true,
      currentComponent: '',
      buttons: [
        { name: 'Гидранты', component: 'AppMapHydrants' },
        { name: 'Обязанности', component: 'AppRespon' },
        { name: 'Уставы', component: 'AppInformation' },
        { name: 'Проверка №1', component: 'AppCheck1' },
        { name: 'Калькулятор ГДЗС', component: 'AppCalculator' },
        { name: 'Приём дежурства', component: 'AppAcceptance' }
      ]
    }
  },
  methods: {
    deactiveIntro () {
      this.introActive = false
    }
  },
  computed: {
    currentDate () {
      return new Date().toLocaleDateString('ru-RU', {
        day: 'numeric',
        month: 'long',
        year: 'numeric'
      })
    },
    todayShift () {
      const now = new Date()
      let diffTime = now - this.startDate
      const todayCutOff = new Date()
      todayCutOff.setHours(9, 15, 0, 0)

      if (now < todayCutOff) {
        diffTime -= 1000 * 60 * 60 * 24
      }

      const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24))

      return (diffDays % this.totalShifts) + 1
    }
  }
}
</script>

<style scoped>
.crossed {
  text-decoration: line-through;
  text-decoration-thickness: 3px;
}
.text-joke {
  font-size: 12px;
}
</style>
