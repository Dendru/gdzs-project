<template>
  <h2>Карта гидрантов</h2>
  <p>Для того, чтобы открыть карту с гидрантами перейди по ссылке ниже</p>
  <a class="map-btn" href="https://yandex.ru/maps/?bookmarks%5BpublicId%5D=cUIoRBQK" target="_blank">Открыть карту</a>
  <p class="thank">Спасибо Алексею Е. за то, что разработал эту карту</p>
</template>

<script>
export default {
  data () {
    return {

    }
  }
}
</script>

<style scoped>

.map-btn {
  display: inline-block;
  font-size: 16px;
  padding: 18px 34px;
  background: linear-gradient(135deg, #24d32d, #4c00ff);
  border: none;
  border-radius: 12px;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.1s, box-shadow 0.1s;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

.map-btn:hover {
  background: linear-gradient(135deg, #63ff33, #4c00ff);
}

.map-btn:active {
  transform: scale(0.95);
  box-shadow: 0 2px 4px rgba(0,0,0,0.2);
}
.thank{
  font-size: 13px;
}
</style>
