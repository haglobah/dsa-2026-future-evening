<script setup>
import { computed } from 'vue'

const props = defineProps({
  // [{ label: string, value: number }] — drawn clockwise from 12 o'clock
  data: { type: Array, required: true },
  // one fill per datum, same order
  colors: { type: Array, required: true },
  // pie only, labels in a legend below — for several charts on one slide
  compact: { type: Boolean, default: false },
})

const W = props.compact ? 360 : 920
const H = props.compact ? 360 : 460
const CX = W / 2
const CY = H / 2
const R = props.compact ? 174 : 168
const GAP = 3 // surface-colored stroke between slices
const SURFACE = '#ffffff'
const INK = '#2b2a27'
const LABEL_SIZE = 21
const LINE_H = 27
const COUNT_SIZE = 30
const MAX_LINE_CHARS = 24

function polar(angleDeg, radius) {
  // 0° = 12 o'clock, clockwise
  const rad = (angleDeg * Math.PI) / 180
  return { x: CX + Math.sin(rad) * radius, y: CY - Math.cos(rad) * radius }
}

function arcPath(a0, a1) {
  const p0 = polar(a0, R)
  const p1 = polar(a1, R)
  const large = a1 - a0 > 180 ? 1 : 0
  return `M ${CX} ${CY} L ${p0.x} ${p0.y} A ${R} ${R} 0 ${large} 1 ${p1.x} ${p1.y} Z`
}

function wrap(text) {
  const words = text.split(' ')
  const lines = []
  let line = ''
  for (const w of words) {
    if (line && (line + ' ' + w).length > MAX_LINE_CHARS) {
      lines.push(line)
      line = w
    } else {
      line = line ? line + ' ' + w : w
    }
  }
  if (line) lines.push(line)
  return lines
}

function luminance(hex) {
  const n = parseInt(hex.slice(1), 16)
  const ch = [n >> 16 & 255, n >> 8 & 255, n & 255].map((v) => {
    v /= 255
    return v <= 0.03928 ? v / 12.92 : ((v + 0.055) / 1.055) ** 2.4
  })
  return 0.2126 * ch[0] + 0.7152 * ch[1] + 0.0722 * ch[2]
}

const slices = computed(() => {
  const total = props.data.reduce((s, d) => s + d.value, 0)
  let angle = 0
  return props.data.map((d, i) => {
    const a0 = angle
    const a1 = angle + (d.value / total) * 360
    angle = a1
    const mid = (a0 + a1) / 2
    const count = polar(mid, R * 0.68)
    const text = polar(mid, R + 44)
    const dx = Math.sin((mid * Math.PI) / 180)
    const anchor = Math.abs(dx) < 0.35 ? 'middle' : dx > 0 ? 'start' : 'end'
    const lines = wrap(d.label)
    // center the block on the anchor point, but keep top/bottom labels
    // growing away from the pie instead of into it
    const dy = -Math.cos((mid * Math.PI) / 180)
    let firstLineY
    if (anchor === 'middle' && dy < -0.6) {
      firstLineY = text.y - (lines.length - 1) * LINE_H // grow upward
    } else if (anchor === 'middle' && dy > 0.6) {
      firstLineY = text.y + LINE_H * 0.6 // grow downward
    } else {
      firstLineY = text.y - ((lines.length - 1) * LINE_H) / 2
    }
    return {
      key: i,
      path: arcPath(a0, a1),
      fill: props.colors[i],
      value: d.value,
      count,
      // 0.227 = equal-contrast crossover between white and INK on the fill
      countInk: luminance(props.colors[i]) > 0.227 ? INK : SURFACE,
      text: { x: text.x, y: firstLineY },
      anchor,
      lines,
    }
  })
})
</script>

<template>
  <div :class="compact ? 'pie-compact' : null">
  <svg
    class="pie-chart"
    :viewBox="`0 0 ${W} ${H}`"
    :style="compact ? { width: '190px', height: '190px' } : { width: '820px', height: '410px' }"
    role="img"
  >
    <path
      v-for="s in slices"
      :key="'slice-' + s.key"
      :d="s.path"
      :fill="s.fill"
      :stroke="SURFACE"
      :stroke-width="GAP"
      stroke-linejoin="round"
    />
    <g v-for="s in slices" :key="'label-' + s.key">
      <text
        :x="s.count.x"
        :y="s.count.y"
        :fill="s.countInk"
        :font-size="COUNT_SIZE"
        font-weight="700"
        text-anchor="middle"
        dominant-baseline="central"
      >
        {{ s.value }}
      </text>
      <text
        v-if="!compact"
        :x="s.text.x"
        :y="s.text.y"
        :fill="INK"
        :font-size="LABEL_SIZE"
        :text-anchor="s.anchor"
      >
        <tspan
          v-for="(line, li) in s.lines"
          :key="li"
          :x="s.text.x"
          :y="s.text.y + li * LINE_H"
        >
          {{ line }}
        </tspan>
      </text>
    </g>
  </svg>
  <ul v-if="compact" class="pie-legend">
    <li v-for="s in slices" :key="'legend-' + s.key">
      <span class="swatch" :style="{ background: s.fill }" />
      <span>{{ data[s.key].label }}</span>
    </li>
  </ul>
  </div>
</template>

<style scoped>
.pie-compact {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.8rem;
}
.pie-legend {
  list-style: none;
  margin: 0;
  padding: 0;
  font-size: 1rem;
  line-height: 1.25;
  text-align: left;
}
.pie-legend li {
  display: flex;
  align-items: baseline;
  gap: 0.5rem;
  margin: 0 0 0.4rem;
  padding: 0;
  line-height: 1.25;
}
.pie-legend li::before {
  content: none;
}
.swatch {
  flex: none;
  width: 0.8em;
  height: 0.8em;
  border-radius: 2px;
  transform: translateY(0.1em);
}
</style>
