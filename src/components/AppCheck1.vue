<template>
  <div>
    <h2>Проверка №1</h2>
    <div>
      <ol>
        <template v-for="(item, index) in checkList" :key="index">
          <li v-if="typeof item === 'string'">
            <p>{{ item }}</p>
          </li>
          <li v-else-if="item && typeof item === 'object'">
            <p>{{ item.title }}</p>
            <ul v-if="item.subList && item.subList.length" class="custom-list">
              <li v-for="(sub, subIdx) in item.subList" :key="subIdx">
                <p>{{ sub }}</p>
              </li>
            </ul>
          </li>
        </template>
      </ol>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      checkList: [
        'Визуальный осмотр маски и аппарата',
        'Проверка маски на герметичность (1000 Па, 1 мин, падение не более 100 Па)',
        'Срабатывание клапана выхода (420 – 600 Па)',
        'Избыточное давление создаваемое легочный автоматом (250 – 450 Па)',
        'Герметичность легочного автомата (750 ± 100 Па)',
        'Редуцированное давление (манометр 0,5 – 0,9 Мпа)',
        {
          title:
            'Герметичность аппарата (падение не более 10 атм. за 1 минуту):',
          subList: ['без подключения к маске', 'с подключением к маске']
        },
        'Звуковой сигнал (60 – 50 атм.)',
        'Визуальный осмотр спасательного устройства',
        'Проверка давление воздуха в баллоне',
        'Запись в журнале проверок № 1'
      ]
    }
  }
}
</script>

<style scoped>
ol {
  margin-left: -20px;
}
li {
  text-align: left;
}
.custom-list {
  list-style-type: disc;
  padding-left: 10px;
}
</style>
