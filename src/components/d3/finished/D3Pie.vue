<template>
  <svg v-if="paths">
    <D3Arc v-for="(item, key) in paths"
           :key="key"
           :fill-color="colors(paths.length)[key]"
           :start-angle="item.startAngle"
           :end-angle="item.endAngle"
           :pad-angle="item.padAngle"
           :inner-radius="innerRadius"
           :outer-radius="outerRadius"
           :corner-radius="cornerRadius">
      <g slot-scope="{centroid}">
        <slot v-bind="{centroid, item, key, color: colors(paths.length)[key] }">
              {{item.centroid}}
        </slot>
      </g>
    </D3Arc>
  </svg>
</template>

<script>
import Vue from 'vue'
import D3Arc from './D3Arc.vue'
import { pie } from 'd3-shape'

export default Vue.extend({
  name:       'PieChart',
  components: {
    D3Arc
  },
  props: {
    inputDatum: {
      type: Array,
      default() {
        return [1, 2, 3, 4, 15, 6, 7, 8, 9]
      }
    },
    index: {
      type:    Number,
      default: 0
    },
    startAngle: {
      type:    Number,
      default: 0
    },
    endAngle: {
      type:    Number,
      default: Math.PI * 2
    },
    padAngle: {
      type:    Number,
      default: 0
    },
    innerRadius: {
      type:    Number,
      default: 200
    },
    outerRadius: {
      type:    Number,
      default: 300
    },
    cornerRadius: {
      type:    Number,
      default: 0
    }
  },
  computed: {
    pieGenerator() {
      return pie()
        .startAngle(this.startAngle)
        .endAngle(this.endAngle)
        .padAngle(this.padAngle)

    },
    paths() {
      if (this.inputDatum) {
        return this.pieGenerator(this.inputDatum).map(v => {
          return {
            ...v,
            centroid: {
              degrees: 0
            }
          }
        })
      } else {
        return []
      }
    }
  }
})
</script>
