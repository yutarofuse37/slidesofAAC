<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const BLUE = '#1c3177'
const SUB = '#7f96c2'

const { $clicks } = useSlideContext()
/** 0: 座標 → 1: 両領域 → 2: パス例（A₂） → 3: 密／直径例（A₁） */
const step = computed(() => Math.min(Math.max(Number($clicks.value ?? 0), 0), 3))
</script>

<template>
  <div class="cr" :data-step="step">
    <div class="cr__legend">
      <span class="cr__chip cr__chip--a2">A₂ が速い</span>
      <span class="cr__chip cr__chip--a1">A₁ が速い</span>
    </div>

    <div class="cr__plot">
      <svg viewBox="0 0 320 270" class="cr__svg" aria-hidden="true">
        <defs>
          <pattern id="crHatch" width="8" height="8" patternUnits="userSpaceOnUse" patternTransform="rotate(35)">
            <line x1="0" y1="0" x2="0" y2="8" :stroke="BLUE" stroke-width="2.2" />
          </pattern>
          <marker id="crX" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto">
            <path d="M0,0 L5,3 L0,6 Z" :fill="BLUE" />
          </marker>
          <marker id="crY" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto">
            <path d="M0,0 L5,3 L0,6 Z" :fill="BLUE" />
          </marker>
        </defs>

        <!-- A1: soft base + hatch, A2: solid sub-blue -->
        <polygon class="cr__a1-base" points="70,45 280,45 280,230 165,230 70,140" fill="#e8eef8" />
        <polygon class="cr__a1" points="70,45 280,45 280,230 165,230 70,140" fill="url(#crHatch)" />
        <polygon class="cr__a2" points="70,230 165,230 70,140" :fill="SUB" />

        <line x1="70" y1="230" x2="290" y2="230" :stroke="BLUE" stroke-width="1.8" marker-end="url(#crX)" />
        <line x1="70" y1="230" x2="70" y2="30" :stroke="BLUE" stroke-width="1.8" marker-end="url(#crY)" />
        <line class="cr__boundary" x1="70" y1="140" x2="165" y2="230" :stroke="BLUE" stroke-width="2.6" />

        <!-- example points -->
        <g class="cr__pt cr__pt--path">
          <circle cx="95" cy="205" r="7" fill="#fff" :stroke="BLUE" stroke-width="2.2" />
          <circle cx="95" cy="205" r="3.2" :fill="BLUE" />
        </g>
        <g class="cr__pt cr__pt--dense">
          <circle cx="250" cy="95" r="7" fill="#fff" :stroke="BLUE" stroke-width="2.2" />
          <circle cx="250" cy="95" r="3.2" :fill="BLUE" />
        </g>
      </svg>

      <div class="cr__overlay">
        <span class="cr__tick cr__tick--x1">1</span>
        <span class="cr__tick cr__tick--x15">3/2</span>
        <span class="cr__tick cr__tick--x2">2</span>
        <span class="cr__tick cr__tick--ym3">-3</span>
        <span class="cr__tick cr__tick--ym2">-2</span>
        <span class="cr__tick cr__tick--ym1">-1</span>
        <span class="cr__axis cr__axis--x">x = logₙ m</span>
        <span class="cr__axis cr__axis--y">y = logₙ OPT</span>
        <span class="cr__eq">境界 y = -2x</span>
        <span class="cr__lab cr__lab--a1">A₁</span>
        <span class="cr__lab cr__lab--a2">A₂</span>
        <span class="cr__ptlab cr__ptlab--path">パス</span>
        <span class="cr__ptlab cr__ptlab--dense">密</span>
        <span class="cr__focus cr__focus--a2" />
        <span class="cr__focus cr__focus--a1" />
      </div>
    </div>

    <!-- concrete example miniatures -->
    <div class="cr__examples">
      <div class="cr__ex cr__ex--path">
        <div class="cr__ex-title">例：パス Pₙ</div>
        <svg viewBox="0 0 160 48" class="cr__ex-svg" aria-hidden="true">
          <line x1="18" y1="24" x2="142" y2="24" :stroke="SUB" stroke-width="2.5" stroke-linecap="round" />
          <circle cx="18" cy="24" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
          <circle cx="56" cy="24" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
          <circle cx="94" cy="24" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
          <circle cx="132" cy="24" r="7" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
        </svg>
        <div class="cr__ex-meta">m≈n，OPT≈n⁻³ → A₂</div>
      </div>

      <div class="cr__ex cr__ex--dense">
        <div class="cr__ex-title">例：密／直径 O(1)</div>
        <svg viewBox="0 0 160 56" class="cr__ex-svg" aria-hidden="true">
          <line x1="40" y1="16" x2="120" y2="16" :stroke="BLUE" stroke-width="2" />
          <line x1="40" y1="16" x2="40" y2="44" :stroke="BLUE" stroke-width="2" />
          <line x1="120" y1="16" x2="120" y2="44" :stroke="BLUE" stroke-width="2" />
          <line x1="40" y1="44" x2="120" y2="44" :stroke="BLUE" stroke-width="2" />
          <line x1="40" y1="16" x2="120" y2="44" :stroke="SUB" stroke-width="1.6" />
          <line x1="120" y1="16" x2="40" y2="44" :stroke="SUB" stroke-width="1.6" />
          <line x1="80" y1="8" x2="40" y2="16" :stroke="SUB" stroke-width="1.6" />
          <line x1="80" y1="8" x2="120" y2="16" :stroke="SUB" stroke-width="1.6" />
          <circle cx="40" cy="16" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
          <circle cx="120" cy="16" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
          <circle cx="40" cy="44" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
          <circle cx="120" cy="44" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
          <circle cx="80" cy="8" r="6" fill="#fff" :stroke="BLUE" stroke-width="1.6" />
        </svg>
        <div class="cr__ex-meta">OPT≳n⁻²，密なら A₁</div>
      </div>
    </div>

    <div class="cr__hint">クリックで左側の説明と一緒に進みます</div>
  </div>
</template>

<style scoped>
.cr {
  width: 100%;
  max-width: 19rem;
  margin: 0 auto;
  color: #1c3177;
}
.cr__legend {
  display: flex;
  justify-content: center;
  gap: 0.45rem;
  margin-bottom: 0.3rem;
}
.cr__chip {
  font-size: 0.68rem;
  font-weight: 700;
  padding: 0.12rem 0.5rem;
  border-radius: 999px;
  border: 1.5px solid #1c3177;
  color: #1c3177;
  transition: background 0.35s ease, color 0.35s ease, opacity 0.35s ease;
}
.cr__chip--a2 { background: #7f96c2; color: #fff; border-color: #7f96c2; }
.cr__chip--a1 {
  background: #f9fbff;
  background-image: repeating-linear-gradient(
    -55deg,
    #f9fbff 0 3px,
    #1c3177 3px 5px
  );
}
.cr__plot { position: relative; }
.cr__svg { width: 100%; height: auto; display: block; }
.cr__overlay { position: absolute; inset: 0; pointer-events: none; }
.cr__tick { position: absolute; font-size: 0.7rem; font-weight: 600; color: #1c3177; }
.cr__tick--x1 { left: 20%; bottom: 7%; transform: translateX(-50%); }
.cr__tick--x15 { left: 48%; bottom: 7%; transform: translateX(-50%); }
.cr__tick--x2 { left: 82%; bottom: 7%; transform: translateX(-50%); }
.cr__tick--ym3 { left: 12%; bottom: 15%; transform: translateX(-100%); }
.cr__tick--ym2 { left: 12%; top: 48%; transform: translate(-100%, -50%); }
.cr__tick--ym1 { left: 12%; top: 16%; transform: translate(-100%, -50%); }
.cr__axis { position: absolute; font-size: 0.68rem; font-weight: 600; color: #1c3177; }
.cr__axis--x { right: 2%; bottom: 1%; }
.cr__axis--y { left: 1%; top: 1%; }
.cr__eq {
  position: absolute;
  left: 48%;
  top: 58%;
  font-size: 0.68rem;
  color: #1c3177;
  font-weight: 700;
}
.cr__lab {
  position: absolute;
  font-weight: 800;
  transition: opacity 0.35s ease, transform 0.35s ease;
}
.cr__lab--a1 { left: 62%; top: 34%; font-size: 1.15rem; color: #1c3177; }
.cr__lab--a2 { left: 26%; top: 72%; font-size: 1.05rem; color: #1c3177; }
.cr__ptlab {
  position: absolute;
  font-size: 0.68rem;
  font-weight: 700;
  color: #1c3177;
  opacity: 0;
  transition: opacity 0.35s ease;
}
.cr__ptlab--path { left: 22%; top: 78%; }
.cr__ptlab--dense { left: 78%; top: 26%; }
.cr__pt { opacity: 0; transition: opacity 0.35s ease; }
.cr__focus {
  position: absolute;
  border: 2.5px solid #1c3177;
  border-radius: 0.4rem;
  opacity: 0;
  transition: opacity 0.35s ease;
}
.cr__focus--a2 { left: 18%; top: 48%; width: 28%; height: 36%; }
.cr__focus--a1 { left: 28%; top: 12%; width: 58%; height: 70%; }
.cr__a1,
.cr__a1-base,
.cr__a2,
.cr__boundary {
  transition: opacity 0.35s ease;
}
.cr__a1 { opacity: 0; }
.cr__a1-base { opacity: 0; }
.cr__a2 { opacity: 0; }

.cr__examples {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.4rem;
  margin-top: 0.4rem;
}
.cr__ex {
  border: 1.5px solid #cfdbed;
  border-radius: 0.45rem;
  padding: 0.3rem 0.35rem 0.35rem;
  background: #f9fbff;
  opacity: 0.35;
  transition: opacity 0.35s ease, border-color 0.35s ease, box-shadow 0.35s ease;
}
.cr__ex-title {
  font-size: 0.65rem;
  font-weight: 700;
  color: #1c3177;
  text-align: center;
}
.cr__ex-svg { width: 100%; height: auto; display: block; margin-top: 0.1rem; }
.cr__ex-meta {
  margin-top: 0.1rem;
  font-size: 0.6rem;
  font-weight: 600;
  color: #1c3177;
  text-align: center;
  line-height: 1.25;
}
.cr__hint {
  text-align: center;
  margin-top: 0.3rem;
  font-size: 0.62rem;
  color: #7f96c2;
}

/* step 0: axes only */
.cr[data-step="0"] .cr__chip { opacity: 0.45; }

/* step 1: both regions */
.cr[data-step="1"] .cr__a1-base { opacity: 1; }
.cr[data-step="1"] .cr__a1 { opacity: 0.55; }
.cr[data-step="1"] .cr__a2 { opacity: 1; }
.cr[data-step="1"] .cr__chip { opacity: 1; }

/* step 2: path / A2 */
.cr[data-step="2"] .cr__a1-base { opacity: 0.35; }
.cr[data-step="2"] .cr__a1 { opacity: 0.2; }
.cr[data-step="2"] .cr__a2 { opacity: 1; }
.cr[data-step="2"] .cr__lab--a2 { transform: scale(1.12); }
.cr[data-step="2"] .cr__lab--a1 { opacity: 0.4; }
.cr[data-step="2"] .cr__chip--a1 { opacity: 0.4; }
.cr[data-step="2"] .cr__chip--a2 { opacity: 1; box-shadow: 0 0 0 2px #1c3177; }
.cr[data-step="2"] .cr__pt--path,
.cr[data-step="2"] .cr__ptlab--path,
.cr[data-step="2"] .cr__focus--a2 { opacity: 1; }
.cr[data-step="2"] .cr__ex--path {
  opacity: 1;
  border-color: #1c3177;
  box-shadow: 0 0 0 1px #1c3177 inset;
}
.cr[data-step="2"] .cr__ex--dense { opacity: 0.35; }

/* step 3: dense / A1 */
.cr[data-step="3"] .cr__a1-base { opacity: 1; }
.cr[data-step="3"] .cr__a1 { opacity: 0.75; }
.cr[data-step="3"] .cr__a2 { opacity: 0.35; }
.cr[data-step="3"] .cr__lab--a1 { transform: scale(1.12); }
.cr[data-step="3"] .cr__lab--a2 { opacity: 0.4; }
.cr[data-step="3"] .cr__chip--a2 { opacity: 0.4; }
.cr[data-step="3"] .cr__chip--a1 { opacity: 1; box-shadow: 0 0 0 2px #1c3177; }
.cr[data-step="3"] .cr__pt--dense,
.cr[data-step="3"] .cr__ptlab--dense,
.cr[data-step="3"] .cr__focus--a1 { opacity: 1; }
.cr[data-step="3"] .cr__ex--dense {
  opacity: 1;
  border-color: #1c3177;
  box-shadow: 0 0 0 1px #1c3177 inset;
}
.cr[data-step="3"] .cr__ex--path { opacity: 0.35; }
</style>
