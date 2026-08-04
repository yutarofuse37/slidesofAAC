<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const BLUE = '#1c3177'
const SUB = '#7f96c2'
const SOFT = '#cfdbed'

const { $clicks } = useSlideContext()
/** 0: 座標 → 1: 両領域 → 2: パス（A₂） → 3: 密（A₁） */
const step = computed(() => Math.min(Math.max(Number($clicks.value ?? 0), 0), 3))
</script>

<template>
  <div class="cr" :data-step="step">
    <div class="cr__legend">
      <span class="cr__chip cr__chip--a2"><i />A₂ が速い</span>
      <span class="cr__chip cr__chip--a1"><i />A₁ が速い</span>
    </div>

    <svg viewBox="0 0 300 240" class="cr__svg" aria-hidden="true">
      <defs>
        <marker id="crX" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto">
          <path d="M0,0 L5,3 L0,6 Z" :fill="BLUE" />
        </marker>
        <marker id="crY" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto">
          <path d="M0,0 L5,3 L0,6 Z" :fill="BLUE" />
        </marker>
      </defs>

      <!-- regions: solid colors only -->
      <polygon class="cr__a1" points="56,36 270,36 270,200 148,200 56,118" :fill="SOFT" />
      <polygon class="cr__a2" points="56,200 148,200 56,118" :fill="BLUE" />

      <!-- axes -->
      <line x1="56" y1="200" x2="278" y2="200" :stroke="BLUE" stroke-width="1.8" marker-end="url(#crX)" />
      <line x1="56" y1="200" x2="56" y2="24" :stroke="BLUE" stroke-width="1.8" marker-end="url(#crY)" />
      <line class="cr__boundary" x1="56" y1="118" x2="148" y2="200" stroke="#fff" stroke-width="3.5" />
      <line class="cr__boundary" x1="56" y1="118" x2="148" y2="200" :stroke="BLUE" stroke-width="2" />

      <!-- ticks -->
      <text x="56" y="216" text-anchor="middle" class="cr__t">1</text>
      <text x="148" y="216" text-anchor="middle" class="cr__t">3/2</text>
      <text x="270" y="216" text-anchor="middle" class="cr__t">2</text>
      <text x="46" y="204" text-anchor="end" class="cr__t">-3</text>
      <text x="46" y="122" text-anchor="end" class="cr__t">-2</text>
      <text x="46" y="40" text-anchor="end" class="cr__t">-1</text>
      <text x="278" y="216" text-anchor="end" class="cr__ax">x=logₙm</text>
      <text x="8" y="18" class="cr__ax">y=logₙOPT</text>

      <!-- region labels -->
      <text class="cr__lab cr__lab--a1" x="190" y="110" text-anchor="middle">A₁</text>
      <text class="cr__lab cr__lab--a2" x="90" y="175" text-anchor="middle">A₂</text>

      <!-- example points (one at a time) -->
      <g class="cr__pt cr__pt--path">
        <circle cx="82" cy="178" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
        <circle cx="82" cy="178" r="3.5" fill="#b42318" />
      </g>
      <g class="cr__pt cr__pt--dense">
        <circle cx="230" cy="78" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
        <circle cx="230" cy="78" r="3.5" fill="#b42318" />
      </g>
    </svg>

    <!-- single example panel: only active one shown -->
    <div class="cr__panel cr__panel--path">
      <div class="cr__panel-head">具体例：パス Pₙ</div>
      <svg viewBox="0 0 180 40" class="cr__mini" aria-hidden="true">
        <line x1="16" y1="20" x2="164" y2="20" :stroke="SUB" stroke-width="3" stroke-linecap="round" />
        <circle cx="16" cy="20" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="64" cy="20" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="112" cy="20" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="160" cy="20" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
      </svg>
      <div class="cr__panel-meta">m≈n，OPT≈n⁻³ → 図の赤点は A₂ 側</div>
    </div>

    <div class="cr__panel cr__panel--dense">
      <div class="cr__panel-head">具体例：密／直径 O(1)</div>
      <svg viewBox="0 0 180 52" class="cr__mini" aria-hidden="true">
        <line x1="50" y1="14" x2="130" y2="14" :stroke="BLUE" stroke-width="2.2" />
        <line x1="50" y1="14" x2="50" y2="40" :stroke="BLUE" stroke-width="2.2" />
        <line x1="130" y1="14" x2="130" y2="40" :stroke="BLUE" stroke-width="2.2" />
        <line x1="50" y1="40" x2="130" y2="40" :stroke="BLUE" stroke-width="2.2" />
        <line x1="50" y1="14" x2="130" y2="40" :stroke="SUB" stroke-width="1.6" />
        <line x1="130" y1="14" x2="50" y2="40" :stroke="SUB" stroke-width="1.6" />
        <line x1="90" y1="6" x2="50" y2="14" :stroke="SUB" stroke-width="1.6" />
        <line x1="90" y1="6" x2="130" y2="14" :stroke="SUB" stroke-width="1.6" />
        <circle cx="50" cy="14" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
        <circle cx="130" cy="14" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
        <circle cx="50" cy="40" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
        <circle cx="130" cy="40" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
        <circle cx="90" cy="6" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
      </svg>
      <div class="cr__panel-meta">OPT≳n⁻²，密なら → 図の赤点は A₁ 側</div>
    </div>
  </div>
</template>

<style scoped>
.cr {
  width: 100%;
  max-width: 18rem;
  margin: 0 auto;
  color: #1c3177;
}
.cr__legend {
  display: flex;
  justify-content: center;
  gap: 0.55rem;
  margin-bottom: 0.35rem;
}
.cr__chip {
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  font-size: 0.7rem;
  font-weight: 700;
  color: #1c3177;
}
.cr__chip i {
  display: inline-block;
  width: 0.85rem;
  height: 0.85rem;
  border-radius: 0.15rem;
  border: 1px solid #1c3177;
}
.cr__chip--a2 i { background: #1c3177; }
.cr__chip--a1 i { background: #cfdbed; }

.cr__svg {
  width: 100%;
  height: auto;
  display: block;
}
.cr__t {
  font-size: 11px;
  fill: #1c3177;
  font-family: system-ui, sans-serif;
  font-weight: 600;
}
.cr__ax {
  font-size: 10px;
  fill: #1c3177;
  font-family: system-ui, sans-serif;
  font-weight: 600;
}
.cr__lab {
  font-size: 22px;
  font-weight: 800;
  font-family: system-ui, sans-serif;
  transition: opacity 0.3s ease;
}
.cr__lab--a1 { fill: #1c3177; }
.cr__lab--a2 { fill: #fff; }
.cr__a1,
.cr__a2,
.cr__pt,
.cr__panel {
  transition: opacity 0.3s ease;
}
.cr__a1,
.cr__a2 {
  opacity: 0;
}
.cr__pt {
  opacity: 0;
}
.cr__panel {
  display: none;
  margin-top: 0.55rem;
  padding: 0.45rem 0.55rem;
  border: 2px solid #1c3177;
  border-radius: 0.5rem;
  background: #f9fbff;
}
.cr__panel-head {
  font-size: 0.75rem;
  font-weight: 700;
  color: #1c3177;
  text-align: center;
}
.cr__mini {
  width: 100%;
  height: auto;
  display: block;
  margin-top: 0.2rem;
}
.cr__panel-meta {
  margin-top: 0.2rem;
  font-size: 0.68rem;
  font-weight: 600;
  color: #1c3177;
  text-align: center;
}

/* step 0: axes only */
.cr[data-step="0"] .cr__lab { opacity: 0; }

/* step 1: both regions */
.cr[data-step="1"] .cr__a1,
.cr[data-step="1"] .cr__a2 { opacity: 1; }

/* step 2: emphasize A2 + path example */
.cr[data-step="2"] .cr__a1 { opacity: 0.25; }
.cr[data-step="2"] .cr__a2 { opacity: 1; }
.cr[data-step="2"] .cr__lab--a1 { opacity: 0.3; }
.cr[data-step="2"] .cr__pt--path { opacity: 1; }
.cr[data-step="2"] .cr__panel--path { display: block; }
.cr[data-step="2"] .cr__chip--a1 { opacity: 0.4; }

/* step 3: emphasize A1 + dense example */
.cr[data-step="3"] .cr__a1 { opacity: 1; }
.cr[data-step="3"] .cr__a2 { opacity: 0.25; }
.cr[data-step="3"] .cr__lab--a2 { opacity: 0.35; }
.cr[data-step="3"] .cr__pt--dense { opacity: 1; }
.cr[data-step="3"] .cr__panel--dense { display: block; }
.cr[data-step="3"] .cr__chip--a2 { opacity: 0.4; }
</style>
