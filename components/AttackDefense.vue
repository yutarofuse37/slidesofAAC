<script setup lang="ts">
const BLUE = '#1c3177'
const SUB = '#7f96c2'
const WEAK = '#b42318'
</script>

<template>
  <div class="ad">
    <div class="ad__title">守備が w を配分 → 攻撃が引っ張り x を選ぶ</div>

    <svg viewBox="0 0 360 200" class="ad__svg" aria-hidden="true">
      <!-- turn banners -->
      <g class="ad__turn ad__turn--def">
        <rect x="8" y="8" width="72" height="22" rx="11" fill="#e4ebf6" :stroke="BLUE" stroke-width="1.5" />
        <text x="44" y="23" text-anchor="middle" class="ad__turn-text">守備ターン</text>
      </g>
      <g class="ad__turn ad__turn--atk">
        <rect x="280" y="8" width="72" height="22" rx="11" fill="#fff5f4" stroke="#f3c1bc" stroke-width="1.5" />
        <text x="316" y="23" text-anchor="middle" class="ad__turn-text-atk">攻撃ターン</text>
      </g>

      <!-- Defender player (left) -->
      <g class="ad__player ad__player--def">
        <ellipse cx="44" cy="168" rx="18" ry="5" fill="#cfdbed" />
        <rect x="32" y="128" width="24" height="32" rx="12" :fill="SUB" />
        <circle cx="44" cy="112" r="17" fill="#fff" :stroke="SUB" stroke-width="2.2" />
        <circle cx="38" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="50" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="34" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <circle cx="54" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <path d="M39 118 Q44 122 49 118" fill="none" :stroke="BLUE" stroke-width="1.4" stroke-linecap="round" />
        <!-- hand with weight token -->
        <g class="ad__hand ad__hand--def">
          <path d="M54 140 Q72 136 88 120" fill="none" :stroke="SUB" stroke-width="3.2" stroke-linecap="round" />
          <circle cx="88" cy="120" r="4" :fill="BLUE" />
        </g>
        <text x="44" y="188" text-anchor="middle" class="ad__name">守備</text>
        <text x="44" y="198" text-anchor="middle" class="ad__name-sub">w を配分</text>
      </g>

      <!-- Attacker player (right) -->
      <g class="ad__player ad__player--atk">
        <ellipse cx="316" cy="168" rx="18" ry="5" fill="#cfdbed" />
        <rect x="304" y="128" width="24" height="32" rx="12" :fill="BLUE" />
        <circle cx="316" cy="112" r="17" fill="#fff" :stroke="BLUE" stroke-width="2.2" />
        <circle cx="310" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="322" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="306" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <circle cx="326" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <path d="M311 118 Q316 122 321 118" fill="none" :stroke="BLUE" stroke-width="1.4" stroke-linecap="round" />
        <!-- hand pulling -->
        <g class="ad__hand ad__hand--atk">
          <path d="M306 140 Q288 136 272 120" fill="none" :stroke="BLUE" stroke-width="3.2" stroke-linecap="round" />
          <circle cx="272" cy="120" r="4" :fill="BLUE" />
        </g>
        <text x="316" y="188" text-anchor="middle" class="ad__name">攻撃</text>
        <text x="316" y="198" text-anchor="middle" class="ad__name-sub">x を選ぶ</text>
      </g>

      <!-- pull arrows -->
      <g class="ad__pull ad__pull--l">
        <line x1="118" y1="48" x2="96" y2="48" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="88,48 100,42 100,54" :fill="BLUE" />
      </g>
      <g class="ad__pull ad__pull--r">
        <line x1="242" y1="48" x2="264" y2="48" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="272,48 260,42 260,54" :fill="BLUE" />
      </g>

      <!-- graph -->
      <g class="ad__half ad__half--l">
        <line class="ad__e ad__e--side" x1="120" y1="78" x2="152" y2="60" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="120" y1="78" x2="138" y2="112" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="152" y1="60" x2="138" y2="112" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--weak" x1="152" y1="60" x2="174" y2="66" :stroke="WEAK" stroke-linecap="round" />
        <line class="ad__e ad__e--strong" x1="138" y1="112" x2="174" y2="116" :stroke="BLUE" stroke-linecap="round" />
        <circle cx="120" cy="78" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="152" cy="60" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="138" cy="112" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
      </g>
      <g class="ad__half ad__half--r">
        <line class="ad__e ad__e--side" x1="208" y1="60" x2="240" y2="78" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="240" y1="78" x2="222" y2="112" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="208" y1="60" x2="222" y2="112" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--weak" x1="186" y1="66" x2="208" y2="60" :stroke="WEAK" stroke-linecap="round" />
        <line class="ad__e ad__e--strong" x1="186" y1="116" x2="222" y2="112" :stroke="BLUE" stroke-linecap="round" />
        <circle cx="208" cy="60" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="240" cy="78" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="222" cy="112" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
      </g>

      <g class="ad__cut">
        <line x1="170" y1="56" x2="186" y2="72" :stroke="WEAK" stroke-width="2.2" />
        <line x1="170" y1="72" x2="186" y2="56" :stroke="WEAK" stroke-width="2.2" />
      </g>

      <g class="ad__cost">
        <rect x="128" y="138" width="104" height="20" rx="8" fill="#fff5f4" stroke="#f3c1bc" />
        <text x="180" y="152" text-anchor="middle" class="ad__cost-bad">C 小（攻撃有利）</text>
      </g>
      <g class="ad__fix">
        <rect x="128" y="138" width="104" height="20" rx="8" fill="#e4ebf6" stroke="#cfdbed" />
        <text x="180" y="152" text-anchor="middle" class="ad__cost-ok">危ない辺を厚く!</text>
      </g>
    </svg>

    <div class="ad__steps">
      <span class="ad__step ad__step--1"><i>1</i>守備：辺に強度を配分</span>
      <span class="ad__step ad__step--2"><i>2</i>太さ = 配分の大きさ</span>
      <span class="ad__step ad__step--3"><i>3</i>攻撃：引っ張り方を選ぶ</span>
      <span class="ad__step ad__step--4"><i>4</i>細い辺が伸びると C 小</span>
      <span class="ad__step ad__step--5"><i>5</i>守備：危ない辺を厚く</span>
    </div>
  </div>
</template>

<style scoped>
.ad {
  width: 100%;
  max-width: 23rem;
  margin: 0 auto;
  color: #1c3177;
}
.ad__title {
  text-align: center;
  font-size: 0.72rem;
  font-weight: 600;
  color: #64748b;
  margin-bottom: 0.1rem;
}
.ad__svg {
  width: 100%;
  height: auto;
  display: block;
  overflow: visible;
}
.ad__turn-text {
  font-size: 10px;
  font-weight: 700;
  fill: #1c3177;
  font-family: system-ui, sans-serif;
}
.ad__turn-text-atk {
  font-size: 10px;
  font-weight: 700;
  fill: #b42318;
  font-family: system-ui, sans-serif;
}
.ad__name {
  font-size: 11px;
  font-weight: 700;
  fill: #1c3177;
  font-family: system-ui, sans-serif;
}
.ad__name-sub {
  font-size: 8px;
  fill: #7f96c2;
  font-family: system-ui, sans-serif;
}
.ad__cost-bad {
  font-size: 10px;
  font-weight: 700;
  fill: #b42318;
  font-family: system-ui, sans-serif;
}
.ad__cost-ok {
  font-size: 10px;
  font-weight: 700;
  fill: #1c3177;
  font-family: system-ui, sans-serif;
}
.ad__steps {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.28rem;
  margin-top: 0.2rem;
}
.ad__step {
  display: inline-flex;
  align-items: center;
  gap: 0.22rem;
  font-size: 0.64rem;
  color: #64748b;
  background: #f9fbff;
  border: 1px solid #cfdbed;
  border-radius: 999px;
  padding: 0.1rem 0.42rem 0.1rem 0.18rem;
}
.ad__step i {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 0.95rem;
  height: 0.95rem;
  border-radius: 999px;
  background: #e4ebf6;
  color: #1c3177;
  font-style: normal;
  font-weight: 700;
  font-size: 0.58rem;
}

/* 9s:
   0–28%  defender turn (allocate / thickness)
   28–68% attacker turn (pull / cheap cut)
   68–90% defender turn again (thicken weak)
   90–100% settle
*/
.ad__turn--def { animation: turn-def 9s ease-in-out infinite; }
.ad__turn--atk { animation: turn-atk 9s ease-in-out infinite; }
.ad__player--def { animation: player-def 9s ease-in-out infinite; transform-origin: 44px 140px; }
.ad__player--atk { animation: player-atk 9s ease-in-out infinite; transform-origin: 316px 140px; }
.ad__hand--def { animation: hand-def 9s ease-in-out infinite; transform-origin: 54px 140px; }
.ad__hand--atk { animation: hand-atk 9s ease-in-out infinite; transform-origin: 306px 140px; }
.ad__e--side {
  stroke-width: 1.5;
  animation: side-w 9s ease-in-out infinite;
}
.ad__e--weak {
  stroke-width: 1.4;
  animation: weak-w 9s ease-in-out infinite;
}
.ad__e--strong {
  stroke-width: 1.4;
  animation: strong-w 9s ease-in-out infinite;
}
.ad__pull--l { animation: pull-l 9s ease-in-out infinite; }
.ad__pull--r { animation: pull-r 9s ease-in-out infinite; }
.ad__half--l { animation: half-l 9s ease-in-out infinite; }
.ad__half--r { animation: half-r 9s ease-in-out infinite; }
.ad__cut { animation: cut 9s ease-in-out infinite; transform-origin: 178px 64px; }
.ad__cost { animation: cost 9s ease-in-out infinite; }
.ad__fix { animation: fix 9s ease-in-out infinite; }
.ad__step--1 { animation: s1 9s ease-in-out infinite; }
.ad__step--2 { animation: s2 9s ease-in-out infinite; }
.ad__step--3 { animation: s3 9s ease-in-out infinite; }
.ad__step--4 { animation: s4 9s ease-in-out infinite; }
.ad__step--5 { animation: s5 9s ease-in-out infinite; }

@keyframes turn-def {
  0%, 26% { opacity: 1; transform: scale(1); }
  30%, 66% { opacity: 0.2; transform: scale(0.96); }
  70%, 90% { opacity: 1; transform: scale(1); }
  100% { opacity: 0.35; transform: scale(0.96); }
}
@keyframes turn-atk {
  0%, 26% { opacity: 0.2; transform: scale(0.96); }
  30%, 66% { opacity: 1; transform: scale(1); }
  70%, 100% { opacity: 0.2; transform: scale(0.96); }
}
@keyframes player-def {
  0%, 26% { opacity: 1; filter: none; }
  30%, 66% { opacity: 0.35; filter: grayscale(0.4); }
  70%, 90% { opacity: 1; filter: none; }
  100% { opacity: 0.55; filter: none; }
}
@keyframes player-atk {
  0%, 26% { opacity: 0.35; filter: grayscale(0.4); }
  30%, 66% { opacity: 1; filter: none; }
  70%, 100% { opacity: 0.35; filter: grayscale(0.4); }
}
@keyframes hand-def {
  0%, 6% { transform: rotate(0deg); }
  12%, 24% { transform: rotate(-18deg); }
  30%, 66% { transform: rotate(0deg); }
  74%, 86% { transform: rotate(-14deg); }
  100% { transform: rotate(0deg); }
}
@keyframes hand-atk {
  0%, 28% { transform: rotate(0deg); }
  34%, 62% { transform: rotate(16deg); }
  70%, 100% { transform: rotate(0deg); }
}
@keyframes side-w {
  0%, 8% { stroke-width: 1.4; }
  18%, 100% { stroke-width: 1.8; }
}
@keyframes weak-w {
  0%, 8% { stroke-width: 1.3; opacity: 0.8; stroke-dasharray: none; }
  18%, 28% { stroke-width: 1.5; opacity: 0.9; }
  36%, 52% { stroke-width: 2; opacity: 1; }
  56%, 66% { stroke-width: 1; opacity: 0.28; stroke-dasharray: 3 4; }
  74%, 90% { stroke-width: 6.5; opacity: 1; stroke: #1c3177; stroke-dasharray: none; }
  100% { stroke-width: 1.3; opacity: 0.8; stroke-dasharray: none; }
}
@keyframes strong-w {
  0%, 8% { stroke-width: 1.3; }
  18%, 66% { stroke-width: 5.5; }
  74%, 90% { stroke-width: 4; }
  100% { stroke-width: 1.3; }
}
@keyframes pull-l {
  0%, 28% { opacity: 0; transform: translateX(0); }
  34%, 62% { opacity: 1; transform: translateX(-8px); }
  72%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes pull-r {
  0%, 28% { opacity: 0; transform: translateX(0); }
  34%, 62% { opacity: 1; transform: translateX(8px); }
  72%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes half-l {
  0%, 30% { transform: translateX(0); }
  38%, 62% { transform: translateX(-10px); }
  74%, 100% { transform: translateX(0); }
}
@keyframes half-r {
  0%, 30% { transform: translateX(0); }
  38%, 62% { transform: translateX(10px); }
  74%, 100% { transform: translateX(0); }
}
@keyframes cut {
  0%, 52% { opacity: 0; transform: scale(0.5); }
  56%, 66% { opacity: 1; transform: scale(1); }
  74%, 100% { opacity: 0; transform: scale(0.5); }
}
@keyframes cost {
  0%, 52% { opacity: 0; }
  56%, 66% { opacity: 1; }
  74%, 100% { opacity: 0; }
}
@keyframes fix {
  0%, 72% { opacity: 0; }
  76%, 90% { opacity: 1; }
  100% { opacity: 0; }
}
@keyframes s1 {
  0%, 14% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  20%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s2 {
  0%, 12% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  16%, 26% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  32%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s3 {
  0%, 28% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  32%, 50% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  58%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s4 {
  0%, 50% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  54%, 66% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  74%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s5 {
  0%, 70% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  74%, 90% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
</style>
