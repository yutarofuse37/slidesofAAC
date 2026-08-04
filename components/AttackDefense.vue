<script setup lang="ts">
const BLUE = '#1c3177'
const SUB = '#7f96c2'
const WEAK = '#b42318'
</script>

<template>
  <div class="ad">
    <div class="ad__title">守備が全辺に w を配分 → 攻撃が引っ張り x を選ぶ</div>

    <svg viewBox="0 0 360 200" class="ad__svg" aria-hidden="true">
      <g class="ad__turn ad__turn--def">
        <rect x="8" y="8" width="72" height="22" rx="11" fill="#e4ebf6" :stroke="BLUE" stroke-width="1.5" />
        <text x="44" y="23" text-anchor="middle" class="ad__turn-text">守備ターン</text>
      </g>
      <g class="ad__turn ad__turn--atk">
        <rect x="280" y="8" width="72" height="22" rx="11" fill="#fff5f4" stroke="#f3c1bc" stroke-width="1.5" />
        <text x="316" y="23" text-anchor="middle" class="ad__turn-text-atk">攻撃ターン</text>
      </g>

      <!-- Defender -->
      <g class="ad__player ad__player--def">
        <ellipse cx="44" cy="168" rx="18" ry="5" fill="#cfdbed" />
        <rect x="32" y="128" width="24" height="32" rx="12" :fill="SUB" />
        <circle cx="44" cy="112" r="17" fill="#fff" :stroke="SUB" stroke-width="2.2" />
        <circle cx="38" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="50" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="34" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <circle cx="54" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <path d="M39 118 Q44 122 49 118" fill="none" :stroke="BLUE" stroke-width="1.4" stroke-linecap="round" />
        <g class="ad__hand ad__hand--def">
          <path d="M54 140 Q72 136 88 120" fill="none" :stroke="SUB" stroke-width="3.2" stroke-linecap="round" />
          <circle cx="88" cy="120" r="4" :fill="BLUE" />
        </g>
        <text x="44" y="188" text-anchor="middle" class="ad__name">守備</text>
        <text x="44" y="198" text-anchor="middle" class="ad__name-sub">全辺へ配分</text>
      </g>

      <!-- Attacker -->
      <g class="ad__player ad__player--atk">
        <ellipse cx="316" cy="168" rx="18" ry="5" fill="#cfdbed" />
        <rect x="304" y="128" width="24" height="32" rx="12" :fill="BLUE" />
        <circle cx="316" cy="112" r="17" fill="#fff" :stroke="BLUE" stroke-width="2.2" />
        <circle cx="310" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="322" cy="110" r="2.2" :fill="BLUE" />
        <circle cx="306" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <circle cx="326" cy="116" r="2.4" fill="#f3c1bc" opacity="0.75" />
        <path d="M311 118 Q316 122 321 118" fill="none" :stroke="BLUE" stroke-width="1.4" stroke-linecap="round" />
        <g class="ad__hand ad__hand--atk">
          <path d="M306 140 Q288 136 272 120" fill="none" :stroke="BLUE" stroke-width="3.2" stroke-linecap="round" />
          <circle cx="272" cy="120" r="4" :fill="BLUE" />
        </g>
        <text x="316" y="188" text-anchor="middle" class="ad__name">攻撃</text>
        <text x="316" y="198" text-anchor="middle" class="ad__name-sub">x を選ぶ</text>
      </g>

      <g class="ad__pull ad__pull--l">
        <line x1="118" y1="48" x2="96" y2="48" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="88,48 100,42 100,54" :fill="BLUE" />
      </g>
      <g class="ad__pull ad__pull--r">
        <line x1="242" y1="48" x2="264" y2="48" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="272,48 260,42 260,54" :fill="BLUE" />
      </g>

      <!-- weight tokens scatter onto every edge -->
      <g class="ad__tokens">
        <circle class="ad__tok ad__tok--a" cx="44" cy="120" r="3.5" :fill="BLUE" />
        <circle class="ad__tok ad__tok--b" cx="44" cy="128" r="3.5" :fill="BLUE" />
        <circle class="ad__tok ad__tok--c" cx="44" cy="136" r="3.5" :fill="SUB" />
        <circle class="ad__tok ad__tok--d" cx="52" cy="124" r="3.5" :fill="BLUE" />
        <circle class="ad__tok ad__tok--e" cx="52" cy="132" r="3.5" :fill="SUB" />
        <circle class="ad__tok ad__tok--f" cx="36" cy="132" r="3.5" :fill="SUB" />
        <circle class="ad__tok ad__tok--g" cx="48" cy="140" r="3.5" :fill="BLUE" />
        <circle class="ad__tok ad__tok--h" cx="40" cy="140" r="3.5" :fill="SUB" />
      </g>

      <!-- graph: all edges start dashed, then solid with thickness = weight -->
      <g class="ad__half ad__half--l">
        <line class="ad__e ad__e--mid" x1="120" y1="78" x2="152" y2="60" stroke-linecap="round" />
        <line class="ad__e ad__e--mid" x1="120" y1="78" x2="138" y2="112" stroke-linecap="round" />
        <line class="ad__e ad__e--mid" x1="152" y1="60" x2="138" y2="112" stroke-linecap="round" />
        <line class="ad__e ad__e--thin" x1="152" y1="60" x2="174" y2="66" stroke-linecap="round" />
        <line class="ad__e ad__e--thick" x1="138" y1="112" x2="174" y2="116" stroke-linecap="round" />
        <circle cx="120" cy="78" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="152" cy="60" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
        <circle cx="138" cy="112" r="7.5" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
      </g>
      <g class="ad__half ad__half--r">
        <line class="ad__e ad__e--mid" x1="208" y1="60" x2="240" y2="78" stroke-linecap="round" />
        <line class="ad__e ad__e--mid" x1="240" y1="78" x2="222" y2="112" stroke-linecap="round" />
        <line class="ad__e ad__e--mid" x1="208" y1="60" x2="222" y2="112" stroke-linecap="round" />
        <line class="ad__e ad__e--thin" x1="186" y1="66" x2="208" y2="60" stroke-linecap="round" />
        <line class="ad__e ad__e--thick" x1="186" y1="116" x2="222" y2="112" stroke-linecap="round" />
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
        <text x="180" y="152" text-anchor="middle" class="ad__cost-bad">細い辺 → C 小</text>
      </g>
      <g class="ad__fix">
        <rect x="128" y="138" width="104" height="20" rx="8" fill="#e4ebf6" stroke="#cfdbed" />
        <text x="180" y="152" text-anchor="middle" class="ad__cost-ok">危ない辺を厚く</text>
      </g>
    </svg>

    <div class="ad__steps">
      <span class="ad__step ad__step--1"><i>1</i>最初は全辺が点線</span>
      <span class="ad__step ad__step--2"><i>2</i>守備が全辺に配分</span>
      <span class="ad__step ad__step--3"><i>3</i>太さのある実線に</span>
      <span class="ad__step ad__step--4"><i>4</i>攻撃が引っ張りを選ぶ</span>
      <span class="ad__step ad__step--5"><i>5</i>細い辺が弱点</span>
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

/* base edge: dashed before allocation */
.ad__e {
  fill: none;
  stroke: #7f96c2;
  stroke-width: 1.4;
  stroke-dasharray: 4 4;
}

/* 10s loop
   0–10%   all dashed (no weight)
   10–32%  tokens fly / all edges become solid with thickness
   32–62%  attacker pull, thin bridge stressed
   62–82%  defender thickens weak edges
   82–100% reset toward dashed
*/
.ad__turn--def { animation: turn-def 10s ease-in-out infinite; }
.ad__turn--atk { animation: turn-atk 10s ease-in-out infinite; }
.ad__player--def { animation: player-def 10s ease-in-out infinite; }
.ad__player--atk { animation: player-atk 10s ease-in-out infinite; }
.ad__hand--def { animation: hand-def 10s ease-in-out infinite; transform-origin: 54px 140px; }
.ad__hand--atk { animation: hand-atk 10s ease-in-out infinite; transform-origin: 306px 140px; }
.ad__e--thin { animation: alloc-thin 10s ease-in-out infinite; }
.ad__e--mid { animation: alloc-mid 10s ease-in-out infinite; }
.ad__e--thick { animation: alloc-thick 10s ease-in-out infinite; }
.ad__pull--l { animation: pull-l 10s ease-in-out infinite; }
.ad__pull--r { animation: pull-r 10s ease-in-out infinite; }
.ad__half--l { animation: half-l 10s ease-in-out infinite; }
.ad__half--r { animation: half-r 10s ease-in-out infinite; }
.ad__cut { animation: cut 10s ease-in-out infinite; transform-origin: 178px 64px; }
.ad__cost { animation: cost 10s ease-in-out infinite; }
.ad__fix { animation: fix 10s ease-in-out infinite; }
.ad__step--1 { animation: s1 10s ease-in-out infinite; }
.ad__step--2 { animation: s2 10s ease-in-out infinite; }
.ad__step--3 { animation: s3 10s ease-in-out infinite; }
.ad__step--4 { animation: s4 10s ease-in-out infinite; }
.ad__step--5 { animation: s5 10s ease-in-out infinite; }

.ad__tok--a { animation: tok-a 10s ease-in-out infinite; }
.ad__tok--b { animation: tok-b 10s ease-in-out infinite; }
.ad__tok--c { animation: tok-c 10s ease-in-out infinite; }
.ad__tok--d { animation: tok-d 10s ease-in-out infinite; }
.ad__tok--e { animation: tok-e 10s ease-in-out infinite; }
.ad__tok--f { animation: tok-f 10s ease-in-out infinite; }
.ad__tok--g { animation: tok-g 10s ease-in-out infinite; }
.ad__tok--h { animation: tok-h 10s ease-in-out infinite; }

@keyframes turn-def {
  0%, 30% { opacity: 1; }
  34%, 60% { opacity: 0.2; }
  64%, 82% { opacity: 1; }
  90%, 100% { opacity: 0.35; }
}
@keyframes turn-atk {
  0%, 30% { opacity: 0.2; }
  34%, 60% { opacity: 1; }
  64%, 100% { opacity: 0.2; }
}
@keyframes player-def {
  0%, 30% { opacity: 1; filter: none; }
  34%, 60% { opacity: 0.35; filter: grayscale(0.35); }
  64%, 82% { opacity: 1; filter: none; }
  90%, 100% { opacity: 0.55; filter: none; }
}
@keyframes player-atk {
  0%, 30% { opacity: 0.35; filter: grayscale(0.35); }
  34%, 60% { opacity: 1; filter: none; }
  64%, 100% { opacity: 0.35; filter: grayscale(0.35); }
}
@keyframes hand-def {
  0%, 8% { transform: rotate(0deg); }
  12%, 28% { transform: rotate(-16deg); }
  34%, 60% { transform: rotate(0deg); }
  68%, 80% { transform: rotate(-12deg); }
  100% { transform: rotate(0deg); }
}
@keyframes hand-atk {
  0%, 32% { transform: rotate(0deg); }
  38%, 56% { transform: rotate(14deg); }
  64%, 100% { transform: rotate(0deg); }
}

/* dashed → solid, thickness = allocation */
@keyframes alloc-thin {
  0%, 8% {
    stroke: #7f96c2;
    stroke-width: 1.3;
    stroke-dasharray: 4 4;
    opacity: 0.85;
  }
  18%, 52% {
    stroke: #b42318;
    stroke-width: 1.6;
    stroke-dasharray: none;
    opacity: 1;
  }
  56%, 60% {
    stroke: #b42318;
    stroke-width: 1.2;
    stroke-dasharray: 3 4;
    opacity: 0.35;
  }
  68%, 82% {
    stroke: #1c3177;
    stroke-width: 6.5;
    stroke-dasharray: none;
    opacity: 1;
  }
  92%, 100% {
    stroke: #7f96c2;
    stroke-width: 1.3;
    stroke-dasharray: 4 4;
    opacity: 0.85;
  }
}
@keyframes alloc-mid {
  0%, 8% {
    stroke: #7f96c2;
    stroke-width: 1.3;
    stroke-dasharray: 4 4;
    opacity: 0.85;
  }
  16%, 82% {
    stroke: #7f96c2;
    stroke-width: 2.6;
    stroke-dasharray: none;
    opacity: 1;
  }
  92%, 100% {
    stroke: #7f96c2;
    stroke-width: 1.3;
    stroke-dasharray: 4 4;
    opacity: 0.85;
  }
}
@keyframes alloc-thick {
  0%, 8% {
    stroke: #7f96c2;
    stroke-width: 1.3;
    stroke-dasharray: 4 4;
    opacity: 0.85;
  }
  14%, 82% {
    stroke: #1c3177;
    stroke-width: 5.8;
    stroke-dasharray: none;
    opacity: 1;
  }
  92%, 100% {
    stroke: #7f96c2;
    stroke-width: 1.3;
    stroke-dasharray: 4 4;
    opacity: 0.85;
  }
}

/* tokens to all edge midpoints-ish */
@keyframes tok-a {
  0%, 8% { opacity: 0.4; transform: translate(0, 0); }
  18%, 28% { opacity: 1; transform: translate(92px, -42px); }
  36%, 100% { opacity: 0; transform: translate(92px, -42px); }
}
@keyframes tok-b {
  0%, 9% { opacity: 0.4; transform: translate(0, 0); }
  19%, 28% { opacity: 1; transform: translate(86px, -8px); }
  36%, 100% { opacity: 0; transform: translate(86px, -8px); }
}
@keyframes tok-c {
  0%, 10% { opacity: 0.4; transform: translate(0, 0); }
  20%, 28% { opacity: 1; transform: translate(100px, -48px); }
  36%, 100% { opacity: 0; transform: translate(100px, -48px); }
}
@keyframes tok-d {
  0%, 11% { opacity: 0.4; transform: translate(0, 0); }
  21%, 28% { opacity: 1; transform: translate(118px, -54px); }
  36%, 100% { opacity: 0; transform: translate(118px, -54px); }
}
@keyframes tok-e {
  0%, 10% { opacity: 0.4; transform: translate(0, 0); }
  20%, 28% { opacity: 1; transform: translate(126px, -16px); }
  36%, 100% { opacity: 0; transform: translate(126px, -16px); }
}
@keyframes tok-f {
  0%, 12% { opacity: 0.4; transform: translate(0, 0); }
  22%, 28% { opacity: 1; transform: translate(180px, -48px); }
  36%, 100% { opacity: 0; transform: translate(180px, -48px); }
}
@keyframes tok-g {
  0%, 11% { opacity: 0.4; transform: translate(0, 0); }
  21%, 28% { opacity: 1; transform: translate(188px, -36px); }
  36%, 100% { opacity: 0; transform: translate(188px, -36px); }
}
@keyframes tok-h {
  0%, 13% { opacity: 0.4; transform: translate(0, 0); }
  23%, 28% { opacity: 1; transform: translate(170px, -8px); }
  36%, 100% { opacity: 0; transform: translate(170px, -8px); }
}

@keyframes pull-l {
  0%, 32% { opacity: 0; transform: translateX(0); }
  38%, 56% { opacity: 1; transform: translateX(-8px); }
  66%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes pull-r {
  0%, 32% { opacity: 0; transform: translateX(0); }
  38%, 56% { opacity: 1; transform: translateX(8px); }
  66%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes half-l {
  0%, 34% { transform: translateX(0); }
  40%, 56% { transform: translateX(-10px); }
  68%, 100% { transform: translateX(0); }
}
@keyframes half-r {
  0%, 34% { transform: translateX(0); }
  40%, 56% { transform: translateX(10px); }
  68%, 100% { transform: translateX(0); }
}
@keyframes cut {
  0%, 50% { opacity: 0; transform: scale(0.5); }
  54%, 60% { opacity: 1; transform: scale(1); }
  68%, 100% { opacity: 0; transform: scale(0.5); }
}
@keyframes cost {
  0%, 50% { opacity: 0; }
  54%, 60% { opacity: 1; }
  68%, 100% { opacity: 0; }
}
@keyframes fix {
  0%, 66% { opacity: 0; }
  70%, 82% { opacity: 1; }
  90%, 100% { opacity: 0; }
}
@keyframes s1 {
  0%, 10% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  16%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s2 {
  0%, 10% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  12%, 22% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  30%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s3 {
  0%, 18% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  20%, 30% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  36%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s4 {
  0%, 32% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  36%, 52% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  60%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s5 {
  0%, 50% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  54%, 60% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  68%, 82% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  90%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
</style>
