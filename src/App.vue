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
    let numbers = [1110,210,45,650,600,1000,212,345];
    // **** D3 steps ****
    // select the visual env: Svg
    const svg = d3.select('#viz');
    // ***** Create BARS *****
    // join my data
    const rects = svg.selectAll('rect')
      .data(numbers)
      .join('rect');

    // update: join()
    const scaleLen = d3.scaleLinear()
      .domain([0,d3.max(numbers)])
      .range([0, 600])

    //const scalePos = (d, i) => {
      //return 30 * i + 20;
    //}

    const scalePos = d3.scaleBand()
    .domain(d3.range(numbers.length))
    .range([0,200])
    .round({x: true})
    .paddingInner(0.05)
    .paddingOuter(0.05)

    rects
      .attr('x', 20)
      .attr('height', scalePos.bandwidth())
      .attr('y', (d,i) => scalePos(i))
      .attr('width', scaleLen)
      .attr('fill', '#bfa766')

    // ***** Create LABELS *****
    const labels = svg.selectAll('text')
      .data(numbers)
      .join('text')

    labels
      .text((d) => d)
      .attr('x', scaleLen())
      .attr('y', (d, i) => scalePos(i))
      .attr('dy', scalePos.bandwidth()/2)
      .attr('dx', scaleLen)
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
