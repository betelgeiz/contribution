<template>
  <div id="app">
    <div class="words">
      <span>Август</span>
      <span>Сентябрь</span>
      <span>Октябрь</span>
      <span>Ноябрь</span>
      <span>Декабрь</span>
      <span>Январь</span>
      <span>Февраль</span>
      <span>Март</span>
      <span>Апрель</span>
      <span>Май</span>
      <span>Июнь</span>
      <span>Июль</span>
    </div>
    <ContributionGraph />
  </div>
</template>

<script>
import ContributionGraph from "@/components/ContributionGraph.vue";

export default {
  components: {
    ContributionGraph,
  },
};
</script>

<style scoped>
.words {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.words span {
  flex-basis: calc(8.33% - 5px); /* Задайте желаемую ширину для каждого элемента span */
}
</style>
