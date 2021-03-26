<template>
  <div id="app">
    <h1>D3 Viz</h1>
    <button></button>
    <svg width="800" height="600" id="viz"></svg>
  </div>
</template>

<script>
const d3 = require('d3')

export default{
  name: 'App',
  components: {},
  data(){
    return {
      numbers: [1110,210,45,650,600,90,213,345]
    }
  },
  mounted() {
    this.refreshChart(this.numbers)
  },

  watch: {
    numbers(newVal)  {
      this.refreshChart(newVal);
    }
  },

  methods: {
    refreshChart(listOfNums) {
      // **** D3 steps ****
      // select the visual env: Svg
      const svg = d3.select('#viz');

      const scaleLen = d3.scaleLinear()
          .domain([0,d3.max(listOfNums)])
          .range([0, 600])

      const lAxis = d3.axisTop(scaleLen);

      const scalePos = d3.scaleBand()
          .domain(d3.range(listOfNums.length))
          .range([0,200])
          .round({x: true})
          .paddingInner(0.05)
          .paddingOuter(0.05)
      // range up on graph
      svg.append('g')
          .attr('class', 'lAxis')
          .attr('transform', 'translate(20, 20)')
          .call(lAxis);

      const gs = svg.selectAll('g.bars')// g = elements <g> // .bars = class <g class="bars">
          .data(listOfNums)
          .join('g').attr('class', 'bars');

      gs.attr('transform', (d,i) => `translate(20, ${30 + scalePos(i)} )`)
      // append a <rect> on every <g>
      gs.selectAll('rect')
          .data(d => [d]) // d is the value of one data
          .join('rect')
          .attr('fill', '#bfa766')
          .attr('height', scalePos.bandwidth())
          .attr('width', scaleLen)

      gs.selectAll('text')
          .data(d =>  [d])
          .join('text')
          .text((d) => d)
          .attr('x', scaleLen)
          .attr('y', scalePos.bandwidth() - 5);
    }
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
