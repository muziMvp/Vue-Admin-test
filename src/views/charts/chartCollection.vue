<template>
  <div class="chart-container">
    <!-- <chart height="100%" width="100%" /> -->
    <div class="chart-main">
      <div class="item-d lf-chart"></div>
      <div class="item-d mid-chart"></div>
      <div class="item-d rt-chart"></div>
    </div>
  </div>
</template>

<script>
// import Chart from '@/components/Charts/mixChart'

export default {
  name: 'ChartCollection',
  // components: { Chart }
}
</script>

<style scoped>
.chart-container{
  position: relative;
  width: 100%;
  height: calc(100vh - 84px);
  border:1px solid red;
}
.chart-main {
    display: flex;
    width:100%;
    height: 100%;
}
.item-d {
  flex: 1;
  /* width:100%;
  height: 100%; */
  margin-left:10px;
}
.lf-chart {
  background:blanchedalmond
}
.mid-chart {
  background:orange
}
.rt-chart {
  background-color: aqua;
}
</style>

