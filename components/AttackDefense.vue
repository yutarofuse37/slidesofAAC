<script setup lang="ts">
const BLUE = '#1c3177'
const SUB = '#7f96c2'
const WEAK = '#b42318'
</script>

<template>
  <div class="ad">
    <div class="ad__title">守備が w を配分 → 攻撃が引っ張り x を選ぶ</div>

    <svg viewBox="0 0 340 175" class="ad__svg" aria-hidden="true">
      <!-- roles -->
      <text x="36" y="18" text-anchor="middle" class="ad__role">守備</text>
      <text x="36" y="32" text-anchor="middle" class="ad__role-sub">配分 w</text>
      <text x="304" y="18" text-anchor="middle" class="ad__role">攻撃</text>
      <text x="304" y="32" text-anchor="middle" class="ad__role-sub">引っ張り x</text>

      <!-- defender: weight tokens moving onto edges -->
      <g class="ad__tokens">
        <circle class="ad__tok ad__tok--1" cx="36" cy="70" r="5" :fill="BLUE" />
        <circle class="ad__tok ad__tok--2" cx="36" cy="90" r="5" :fill="BLUE" />
        <circle class="ad__tok ad__tok--3" cx="36" cy="110" r="5" :fill="SUB" />
      </g>

      <!-- attacker: pull handles -->
      <g class="ad__pull ad__pull--l">
        <line x1="92" y1="78" x2="68" y2="78" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="60,78 72,72 72,84" :fill="BLUE" />
      </g>
      <g class="ad__pull ad__pull--r">
        <line x1="248" y1="78" x2="272" y2="78" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="280,78 268,72 268,84" :fill="BLUE" />
      </g>

      <!-- graph: left / right halves -->
      <g class="ad__half ad__half--l">
        <line class="ad__e ad__e--side" x1="110" y1="58" x2="145" y2="42" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="110" y1="58" x2="128" y2="100" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="145" y1="42" x2="128" y2="100" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--bridge-weak" x1="145" y1="42" x2="170" y2="48" :stroke="WEAK" stroke-linecap="round" />
        <line class="ad__e ad__e--bridge-strong" x1="128" y1="100" x2="170" y2="105" :stroke="BLUE" stroke-linecap="round" />
        <circle cx="110" cy="58" r="8" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="145" cy="42" r="8" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="128" cy="100" r="8" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
      </g>

      <g class="ad__half ad__half--r">
        <line class="ad__e ad__e--side" x1="195" y1="42" x2="230" y2="58" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="230" y1="58" x2="212" y2="100" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--side" x1="195" y1="42" x2="212" y2="100" :stroke="SUB" stroke-linecap="round" />
        <line class="ad__e ad__e--bridge-weak" x1="170" y1="48" x2="195" y2="42" :stroke="WEAK" stroke-linecap="round" />
        <line class="ad__e ad__e--bridge-strong" x1="170" y1="105" x2="212" y2="100" :stroke="BLUE" stroke-linecap="round" />
        <circle cx="195" cy="42" r="8" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="230" cy="58" r="8" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="212" cy="100" r="8" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
      </g>

      <!-- cost callout -->
      <g class="ad__cost">
        <rect x="118" y="128" width="104" height="22" rx="8" fill="#fff5f4" stroke="#f3c1bc" />
        <text x="170" y="143" text-anchor="middle" class="ad__cost-text">C が小さい（攻撃有利）</text>
      </g>
      <g class="ad__cost-good">
        <rect x="118" y="128" width="104" height="22" rx="8" fill="#e4ebf6" stroke="#cfdbed" />
        <text x="170" y="143" text-anchor="middle" class="ad__cost-good-text">太い辺で C が上がる</text>
      </g>

      <g class="ad__cut">
        <line x1="160" y1="38" x2="176" y2="54" :stroke="WEAK" stroke-width="2.2" />
        <line x1="160" y1="54" x2="176" y2="38" :stroke="WEAK" stroke-width="2.2" />
      </g>
    </svg>

    <div class="ad__steps">
      <span class="ad__step ad__step--1"><i>1</i>守備：辺に強度を配分</span>
      <span class="ad__step ad__step--2"><i>2</i>太さ = 配分の大きさ</span>
      <span class="ad__step ad__step--3"><i>3</i>攻撃：引っ張り方を選ぶ</span>
      <span class="ad__step ad__step--4"><i>4</i>細い辺が伸びると C 小</span>
      <span class="ad__step ad__step--5"><i>5</i>守備は危ない辺を厚く</span>
    </div>
  </div>
</template>

<style scoped>
.ad {
  width: 100%;
  max-width: 22rem;
  margin: 0 auto;
  color: #1c3177;
}
.ad__title {
  text-align: center;
  font-size: 0.72rem;
  font-weight: 600;
  color: #64748b;
  margin-bottom: 0.15rem;
}
.ad__svg {
  width: 100%;
  height: auto;
  display: block;
  overflow: visible;
}
.ad__role {
  font-size: 12px;
  font-weight: 700;
  fill: #1c3177;
  font-family: system-ui, sans-serif;
}
.ad__role-sub {
  font-size: 9px;
  fill: #7f96c2;
  font-family: system-ui, sans-serif;
}
.ad__cost-text {
  font-size: 9px;
  fill: #b42318;
  font-weight: 700;
  font-family: system-ui, sans-serif;
}
.ad__cost-good-text {
  font-size: 9px;
  fill: #1c3177;
  font-weight: 700;
  font-family: system-ui, sans-serif;
}
.ad__steps {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.28rem;
  margin-top: 0.25rem;
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

/* 9s loop
   0–18%  allocate tokens / edges thicken differently
   18–32% hold allocation visible
   32–55% attacker pulls
   55–70% weak edge stressed, C small
   70–90% defender thickens weak bridge
   90–100% reset
*/
.ad__tok--1 { animation: tok1 9s ease-in-out infinite; }
.ad__tok--2 { animation: tok2 9s ease-in-out infinite; }
.ad__tok--3 { animation: tok3 9s ease-in-out infinite; }
.ad__e--side {
  stroke-width: 1.5;
  animation: side-w 9s ease-in-out infinite;
}
.ad__e--bridge-weak {
  stroke-width: 1.4;
  animation: bridge-weak 9s ease-in-out infinite;
}
.ad__e--bridge-strong {
  stroke-width: 1.4;
  animation: bridge-strong 9s ease-in-out infinite;
}
.ad__pull--l { animation: pull-l 9s ease-in-out infinite; }
.ad__pull--r { animation: pull-r 9s ease-in-out infinite; }
.ad__half--l { animation: half-l 9s ease-in-out infinite; }
.ad__half--r { animation: half-r 9s ease-in-out infinite; }
.ad__cut { animation: cut 9s ease-in-out infinite; transform-origin: 168px 46px; }
.ad__cost { animation: cost-bad 9s ease-in-out infinite; }
.ad__cost-good { animation: cost-good 9s ease-in-out infinite; }
.ad__step--1 { animation: s1 9s ease-in-out infinite; }
.ad__step--2 { animation: s2 9s ease-in-out infinite; }
.ad__step--3 { animation: s3 9s ease-in-out infinite; }
.ad__step--4 { animation: s4 9s ease-in-out infinite; }
.ad__step--5 { animation: s5 9s ease-in-out infinite; }

/* tokens fly from defender onto bridges */
@keyframes tok1 {
  0%, 4% { transform: translate(0, 0); opacity: 0.35; }
  16%, 28% { transform: translate(134px, -22px); opacity: 1; }
  36%, 100% { transform: translate(134px, -22px); opacity: 0; }
}
@keyframes tok2 {
  0%, 6% { transform: translate(0, 0); opacity: 0.35; }
  18%, 28% { transform: translate(134px, 15px); opacity: 1; }
  36%, 100% { transform: translate(134px, 15px); opacity: 0; }
}
@keyframes tok3 {
  0%, 8% { transform: translate(0, 0); opacity: 0.25; }
  20%, 28% { transform: translate(90px, -30px); opacity: 0.7; }
  36%, 100% { transform: translate(90px, -30px); opacity: 0; }
}

@keyframes side-w {
  0%, 8% { stroke-width: 1.4; stroke: #cfdbed; }
  20%, 68% { stroke-width: 1.8; stroke: #7f96c2; }
  78%, 100% { stroke-width: 1.5; stroke: #7f96c2; }
}
@keyframes bridge-weak {
  /* little weight → stays thin; stressed then later reinforced */
  0%, 8% { stroke-width: 1.3; opacity: 0.7; stroke-dasharray: none; }
  20%, 30% { stroke-width: 1.5; opacity: 0.9; }
  40%, 55% { stroke-width: 2; opacity: 1; }
  58%, 68% { stroke-width: 1; opacity: 0.3; stroke-dasharray: 3 4; }
  78%, 90% { stroke-width: 6; opacity: 1; stroke: #1c3177; stroke-dasharray: none; }
  100% { stroke-width: 1.3; opacity: 0.7; stroke-dasharray: none; }
}
@keyframes bridge-strong {
  /* more weight early */
  0%, 8% { stroke-width: 1.3; }
  20%, 68% { stroke-width: 5.5; }
  78%, 90% { stroke-width: 4; }
  100% { stroke-width: 1.3; }
}

@keyframes pull-l {
  0%, 30% { opacity: 0; transform: translateX(0); }
  36%, 65% { opacity: 1; transform: translateX(-8px); }
  75%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes pull-r {
  0%, 30% { opacity: 0; transform: translateX(0); }
  36%, 65% { opacity: 1; transform: translateX(8px); }
  75%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes half-l {
  0%, 32% { transform: translateX(0); }
  40%, 65% { transform: translateX(-10px); }
  78%, 100% { transform: translateX(0); }
}
@keyframes half-r {
  0%, 32% { transform: translateX(0); }
  40%, 65% { transform: translateX(10px); }
  78%, 100% { transform: translateX(0); }
}
@keyframes cut {
  0%, 54% { opacity: 0; transform: scale(0.5); }
  58%, 68% { opacity: 1; transform: scale(1); }
  78%, 100% { opacity: 0; transform: scale(0.5); }
}
@keyframes cost-bad {
  0%, 54% { opacity: 0; }
  58%, 68% { opacity: 1; }
  76%, 100% { opacity: 0; }
}
@keyframes cost-good {
  0%, 74% { opacity: 0; }
  78%, 90% { opacity: 1; }
  100% { opacity: 0; }
}

@keyframes s1 {
  0%, 16% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  22%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s2 {
  0%, 14% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  18%, 30% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  38%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s3 {
  0%, 30% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  34%, 52% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  60%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s4 {
  0%, 52% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  56%, 68% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  76%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s5 {
  0%, 72% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  76%, 90% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
</style>
