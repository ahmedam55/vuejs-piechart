<template>
  <svg class="pie">
    <circle
      v-for="item in dataObjects"
      v-bind:style="{strokeDasharray: `${item.relativeSize} ${circleLength}`, strokeDashoffset: item.offset}"
      r="25%"
      cx="50%"
      cy="50%"
    />
  </svg>
</template>

<script>
  export default {
    name: 'PieChart',
    mounted() {
      setTimeout(() => { this.hasMounted = true }, 0);
      setInterval(() => { this.shuffle(this.data) }, 1000)
    },
    data() {
      return {
        data: [10,20,40,50],
        circleLength: 371.9451599121094,
        hasMounted: false
      }
    },
    methods: {
      shuffle(data) {
        let dataCopy = data.slice();
        let temp;
        let index;
        let randomIndex;

        for (index = 0; index < dataCopy.length; index++) {
          randomIndex = Math.floor(Math.random() * index);

          temp = dataCopy[index];
          dataCopy[index] = dataCopy[randomIndex];
          dataCopy[randomIndex] = temp;
        }

        this.data = dataCopy;
      }
    },
    computed: {
        dataTotal() {
          return this.data.reduce((previous, current) => previous + current);
        },
        dataObjects() {
          let startingPoint = 0;

          return this.data.map(item => {
            let relativeSize = ((item / this.dataTotal) * this.circleLength);

            let dataObject =  { relativeSize: this.hasMounted? relativeSize: 0 , offset: -startingPoint };

            startingPoint += relativeSize;

            return dataObject;
          })
        }
    }
  }
</script>

<style lang="scss">
.pie circle {
  fill: none;
  stroke-width: 32;
  transition: stroke-dasharray 0.3s ease-in-out,stroke-dashoffset 0.3s ease-in-out;
}

$colors: red, yellow, cyan, blue, green, black, white, gray, purple;

@for $i from 1 through length($colors) {
  .pie circle:nth-child(#{$i}) {
    stroke: nth($colors, $i);
  }
}
</style>
