<script setup lang="ts">
const BLUE = '#1c3177'
const SUB = '#7f96c2'
const WEAK = '#b42318'
</script>

<template>
  <div class="ad">
    <div class="ad__title">攻撃＝頂点を引っ張る　／　守備＝辺を厚くする</div>

    <div class="ad__stage">
      <div class="ad__role">
        <div class="ad__role-name">攻撃側</div>
        <div class="ad__role-desc">頂点の引っ張り x</div>
      </div>

      <svg viewBox="0 0 320 150" class="ad__svg" aria-hidden="true">
        <!-- ← pull left, pull right → -->
        <g class="ad__pull ad__pull--l">
          <line x1="52" y1="75" x2="28" y2="75" :stroke="BLUE" stroke-width="2.4" stroke-linecap="round" />
          <polygon points="16,75 30,67 30,83" :fill="BLUE" />
        </g>
        <g class="ad__pull ad__pull--r">
          <line x1="268" y1="75" x2="292" y2="75" :stroke="BLUE" stroke-width="2.4" stroke-linecap="round" />
          <polygon points="304,75 290,67 290,83" :fill="BLUE" />
        </g>

        <!-- left cluster + half bridges -->
        <g class="ad__side ad__side--l">
          <line x1="70" y1="48" x2="110" y2="48" :stroke="SUB" stroke-width="2" />
          <line x1="70" y1="48" x2="90" y2="95" :stroke="SUB" stroke-width="2" />
          <line x1="110" y1="48" x2="90" y2="95" :stroke="SUB" stroke-width="2" />
          <line class="ad__weak-half" x1="110" y1="48" x2="152" y2="48" :stroke="WEAK" stroke-linecap="round" />
          <line class="ad__strong-half" x1="90" y1="95" x2="152" y2="95" :stroke="BLUE" stroke-linecap="round" />
          <circle cx="70" cy="48" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
          <circle cx="110" cy="48" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
          <circle cx="90" cy="95" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
        </g>

        <!-- right cluster + half bridges -->
        <g class="ad__side ad__side--r">
          <line x1="210" y1="48" x2="250" y2="48" :stroke="SUB" stroke-width="2" />
          <line x1="210" y1="48" x2="230" y2="95" :stroke="SUB" stroke-width="2" />
          <line x1="250" y1="48" x2="230" y2="95" :stroke="SUB" stroke-width="2" />
          <line class="ad__weak-half" x1="168" y1="48" x2="210" y2="48" :stroke="WEAK" stroke-linecap="round" />
          <line class="ad__strong-half" x1="168" y1="95" x2="230" y2="95" :stroke="BLUE" stroke-linecap="round" />
          <circle cx="210" cy="48" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
          <circle cx="250" cy="48" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
          <circle cx="230" cy="95" r="8" fill="#fff" :stroke="BLUE" stroke-width="2" />
        </g>

        <g class="ad__cut">
          <line x1="150" y1="38" x2="168" y2="56" :stroke="WEAK" stroke-width="2.4" />
          <line x1="150" y1="56" x2="168" y2="38" :stroke="WEAK" stroke-width="2.4" />
        </g>
      </svg>

      <div class="ad__role">
        <div class="ad__role-name">守備側</div>
        <div class="ad__role-desc">辺強度の配分 w</div>
      </div>
    </div>

    <div class="ad__steps">
      <span class="ad__step ad__step--1">1. 攻撃が頂点を左右へ引っ張る</span>
      <span class="ad__step ad__step--2">2. 細い辺に応力が集中</span>
      <span class="ad__step ad__step--3">3. 守備は危ない辺を厚くしたい</span>
    </div>
  </div>
</template>

<style scoped>
.ad {
  width: 100%;
  max-width: 32rem;
  margin: 0 auto;
  color: #1c3177;
  font-size: 0.8rem;
  line-height: 1.35;
}
.ad__title {
  text-align: center;
  font-size: 0.78rem;
  font-weight: 600;
  color: #475569;
  margin-bottom: 0.35rem;
}
.ad__stage {
  display: grid;
  grid-template-columns: 0.75fr 1.7fr 0.75fr;
  gap: 0.2rem;
  align-items: center;
}
.ad__role { text-align: center; }
.ad__role-name {
  font-size: 0.92rem;
  font-weight: 700;
  color: #1c3177;
}
.ad__role-desc {
  font-size: 0.72rem;
  color: #7f96c2;
  margin-top: 0.15rem;
}
.ad__svg {
  width: 100%;
  height: auto;
  display: block;
  overflow: visible;
}
.ad__steps {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.45rem 0.75rem;
  margin-top: 0.45rem;
}
.ad__step {
  font-size: 0.72rem;
  color: #475569;
  background: #f9fbff;
  border: 1px solid #cfdbed;
  border-radius: 999px;
  padding: 0.15rem 0.55rem;
}

.ad__pull--l { animation: pull-l 3.2s ease-in-out infinite; }
.ad__pull--r { animation: pull-r 3.2s ease-in-out infinite; }
.ad__side--l { animation: side-l 3.2s ease-in-out infinite; }
.ad__side--r { animation: side-r 3.2s ease-in-out infinite; }
.ad__weak-half {
  stroke-width: 1.5;
  animation: weak 3.2s ease-in-out infinite;
}
.ad__strong-half {
  stroke-width: 4.5;
  animation: strong 3.2s ease-in-out infinite;
}
.ad__cut { animation: cut 3.2s ease-in-out infinite; }
.ad__step--1 { animation: step1 3.2s ease-in-out infinite; }
.ad__step--2 { animation: step2 3.2s ease-in-out infinite; }
.ad__step--3 { animation: step3 3.2s ease-in-out infinite; }

@keyframes pull-l {
  0%, 12% { transform: translateX(0); opacity: 0.35; }
  32%, 52% { transform: translateX(-7px); opacity: 1; }
  100% { transform: translateX(0); opacity: 0.35; }
}
@keyframes pull-r {
  0%, 12% { transform: translateX(0); opacity: 0.35; }
  32%, 52% { transform: translateX(7px); opacity: 1; }
  100% { transform: translateX(0); opacity: 0.35; }
}
@keyframes side-l {
  0%, 12% { transform: translateX(0); }
  32%, 52% { transform: translateX(-10px); }
  100% { transform: translateX(0); }
}
@keyframes side-r {
  0%, 12% { transform: translateX(0); }
  32%, 52% { transform: translateX(10px); }
  100% { transform: translateX(0); }
}
@keyframes weak {
  0%, 18% { stroke-width: 1.5; opacity: 0.9; stroke-dasharray: none; }
  35%, 52% { stroke-width: 1; opacity: 0.2; stroke-dasharray: 4 4; }
  100% { stroke-width: 1.5; opacity: 0.9; stroke-dasharray: none; }
}
@keyframes strong {
  0%, 45% { stroke-width: 4; }
  65%, 88% { stroke-width: 6.5; }
  100% { stroke-width: 4; }
}
@keyframes cut {
  0%, 28% { opacity: 0; transform: scale(0.7); }
  38%, 52% { opacity: 1; transform: scale(1); }
  70%, 100% { opacity: 0; transform: scale(0.7); }
}
@keyframes step1 {
  0%, 12% { border-color: #cfdbed; color: #475569; background: #f9fbff; }
  18%, 40% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  55%, 100% { border-color: #cfdbed; color: #475569; background: #f9fbff; }
}
@keyframes step2 {
  0%, 28% { border-color: #cfdbed; color: #475569; background: #f9fbff; }
  35%, 52% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  70%, 100% { border-color: #cfdbed; color: #475569; background: #f9fbff; }
}
@keyframes step3 {
  0%, 55% { border-color: #cfdbed; color: #475569; background: #f9fbff; }
  65%, 88% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  100% { border-color: #cfdbed; color: #475569; background: #f9fbff; }
}
</style>
