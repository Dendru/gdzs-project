<template>
  <div class="container pt-1">
    <div class="card">
      <h1>Пожарная шпаргалка</h1>
      <div v-if="introActive">
        <h4>
          Добро пожаловать на сайт, посвященный рутине простых пожарных в
          суровых условиях работы в ПЧ-60
        </h4>
        <button class="btn intro" @click="deactiveIntro">
          Приступить в работе
        </button>
      </div>
      <div v-if="!introActive">
        <button
        v-for="item in buttons"
        :key="item.name"
        @click="currentComponent = item.component"
        class="btn"
        :class="{selected: currentComponent === item.component}"
        >{{ item.name }}</button>
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

export default {
  components: {
    AppCalculator,
    AppInformation,
    AppRespon,
    AppCheck1,
    AppAcceptance
  },
  data () {
    return {
      introActive: true,
      currentComponent: '',
      buttons: [
        { name: 'Уставы', component: 'AppInformation' },
        { name: 'Проверка №1', component: 'AppCheck1' },
        { name: 'Калькулятор ГДЗС', component: 'AppCalculator' },
        { name: 'Обязанности', component: 'AppRespon' },
        { name: 'Приём дежурства', component: 'AppAcceptance' }
      ]
    }
  },
  methods: {
    deactiveIntro () {
      this.introActive = false
    }
  }
}
</script>

<style>
</style>
