<script setup lang="ts">
const BLUE = '#1c3177'
const SUB = '#7f96c2'
</script>

<template>
  <div class="cr">
    <div class="cr__legend">
      <span class="cr__chip cr__chip--a2">A₂ が速い</span>
      <span class="cr__chip cr__chip--a1">A₁ が速い</span>
    </div>
    <div class="cr__plot">
      <svg viewBox="0 0 320 270" class="cr__svg" aria-hidden="true">
        <polygon class="cr__a1" points="70,45 280,45 280,230 165,230 70,140" fill="#e4ebf6" />
        <polygon class="cr__a2" points="70,230 165,230 70,140" fill="#cfdbed" />
        <line x1="70" y1="230" x2="290" y2="230" :stroke="BLUE" stroke-width="1.5" marker-end="url(#crX)" />
        <line x1="70" y1="230" x2="70" y2="30" :stroke="BLUE" stroke-width="1.5" marker-end="url(#crY)" />
        <line class="cr__boundary" x1="70" y1="140" x2="165" y2="230" :stroke="BLUE" stroke-width="2.2" />
        <circle cx="70" cy="45" r="3" :fill="BLUE" />
        <circle cx="280" cy="45" r="3" :fill="BLUE" />
        <circle cx="70" cy="140" r="3" :fill="BLUE" />
        <circle cx="70" cy="230" r="3" :fill="BLUE" />
        <circle cx="165" cy="230" r="3" :fill="BLUE" />
        <circle cx="280" cy="230" r="3" :fill="BLUE" />
        <defs>
          <marker id="crX" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto"><path d="M0,0 L5,3 L0,6 Z" :fill="BLUE" /></marker>
          <marker id="crY" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto"><path d="M0,0 L5,3 L0,6 Z" :fill="BLUE" /></marker>
        </defs>
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
        <span class="cr__eq">境界 y=-2x</span>
        <span class="cr__lab cr__lab--a1">A₁</span>
        <span class="cr__lab cr__lab--a2">A₂</span>
        <span class="cr__scan" />
      </div>
    </div>
    <div class="cr__caption">OPT が大きい／密なら A₁、パスなど OPT が小さいと A₂</div>
  </div>
</template>

<style scoped>
.cr {
  width: 100%;
  max-width: 18.5rem;
  margin: 0 auto;
  color: #1c3177;
  font-size: 0.78rem;
  line-height: 1.3;
}
.cr__legend {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 0.35rem;
}
.cr__chip {
  font-size: 0.7rem;
  font-weight: 700;
  padding: 0.12rem 0.45rem;
  border-radius: 999px;
  border: 1px solid #cfdbed;
}
.cr__chip--a2 {
  background: #cfdbed;
  color: #1c3177;
  animation: chip-a2 4s ease-in-out infinite;
}
.cr__chip--a1 {
  background: #e4ebf6;
  color: #1c3177;
  animation: chip-a1 4s ease-in-out infinite;
}
.cr__plot { position: relative; }
.cr__svg { width: 100%; height: auto; display: block; }
.cr__overlay { position: absolute; inset: 0; pointer-events: none; }
.cr__tick { position: absolute; font-size: 0.7rem; color: #1c3177; }
.cr__tick--x1 { left: 20%; bottom: 7%; transform: translateX(-50%); }
.cr__tick--x15 { left: 48%; bottom: 7%; transform: translateX(-50%); }
.cr__tick--x2 { left: 82%; bottom: 7%; transform: translateX(-50%); }
.cr__tick--ym3 { left: 12%; bottom: 15%; transform: translateX(-100%); }
.cr__tick--ym2 { left: 12%; top: 48%; transform: translate(-100%, -50%); }
.cr__tick--ym1 { left: 12%; top: 16%; transform: translate(-100%, -50%); }
.cr__axis { position: absolute; font-size: 0.68rem; color: #475569; }
.cr__axis--x { right: 3%; bottom: 1%; }
.cr__axis--y { left: 1%; top: 1%; }
.cr__eq {
  position: absolute;
  left: 46%;
  top: 58%;
  font-size: 0.7rem;
  color: #1c3177;
  font-weight: 600;
}
.cr__lab {
  position: absolute;
  font-weight: 800;
}
.cr__lab--a1 {
  left: 62%;
  top: 36%;
  font-size: 1.1rem;
  color: #1c3177;
  animation: lab-a1 4s ease-in-out infinite;
}
.cr__lab--a2 {
  left: 28%;
  top: 72%;
  font-size: 1rem;
  color: #5f7ab6;
  animation: lab-a2 4s ease-in-out infinite;
}
.cr__scan {
  position: absolute;
  border: 2px solid #1c3177;
  border-radius: 0.35rem;
  animation: scan 4s ease-in-out infinite;
}
.cr__caption {
  text-align: center;
  margin-top: 0.4rem;
  font-size: 0.72rem;
  color: #475569;
}
.cr__a1 { animation: fill-a1 4s ease-in-out infinite; }
.cr__a2 { animation: fill-a2 4s ease-in-out infinite; }
.cr__boundary { animation: boundary 4s ease-in-out infinite; }

@keyframes fill-a2 {
  0%, 40% { fill: #a5b7d6; }
  55%, 100% { fill: #cfdbed; }
}
@keyframes fill-a1 {
  0%, 40% { fill: #e4ebf6; }
  55%, 100% { fill: #bac9e2; }
}
@keyframes lab-a2 {
  0%, 40% { opacity: 1; transform: scale(1.08); }
  55%, 100% { opacity: 0.45; transform: scale(1); }
}
@keyframes lab-a1 {
  0%, 40% { opacity: 0.45; transform: scale(1); }
  55%, 100% { opacity: 1; transform: scale(1.08); }
}
@keyframes chip-a2 {
  0%, 40% { box-shadow: 0 0 0 2px #1c3177 inset; }
  55%, 100% { box-shadow: none; }
}
@keyframes chip-a1 {
  0%, 40% { box-shadow: none; }
  55%, 100% { box-shadow: 0 0 0 2px #1c3177 inset; }
}
@keyframes scan {
  0%, 40% {
    left: 18%; top: 48%; width: 28%; height: 34%;
    opacity: 0.85;
  }
  55%, 100% {
    left: 28%; top: 14%; width: 58%; height: 68%;
    opacity: 0.85;
  }
}
@keyframes boundary {
  0%, 100% { stroke-width: 2; }
  50% { stroke-width: 2.8; }
}
</style>
