<template>
  <div id="app">
    <h1>D3 Viz</h1>
    <svg width="800" height="600" id="viz"></svg>
  </div>
</template>

<script>
const d3 = require('d3')

export default{
  name: 'App',
  components: {},
  mounted() {
    let numbers = [1110,210,45,650,600,1000];
    // **** D3 steps ****
    // select the visual env: Svg
    const svg = d3.select('#viz');

    // join my data
    const rects = svg.selectAll('rect')
      .data(numbers)
      .join('rect');

    // update: join()
    const scaleLen = d3.scaleLinear()
      .domain([0,d3.max(numbers)])
      .range([0, 600])

    const scalePos = (d, i) => {
      return 20 * i;
    }

    rects
      .attr('x', 20)
      .attr('height', 19)
      .attr('y', scalePos)
      .attr('width', scaleLen)
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
