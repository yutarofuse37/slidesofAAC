<script setup lang="ts">
const BLUE = '#1c3177'
const SUB = '#7f96c2'
const SOFT = '#cfdbed'
const WEAK = '#b42318'
</script>

<template>
  <div class="ad">
    <div class="ad__title">攻撃＝頂点を引っ張る　／　守備＝辺を厚くする</div>

    <svg viewBox="0 0 360 210" class="ad__svg" aria-hidden="true">
      <!-- ground -->
      <ellipse cx="180" cy="188" rx="118" ry="8" fill="#e4ebf6" opacity="0.7" />

      <!-- Attacker character (left) -->
      <g class="ad__char ad__char--atk">
        <ellipse cx="42" cy="168" rx="16" ry="4" fill="#cfdbed" />
        <!-- body -->
        <rect x="32" y="132" width="20" height="28" rx="10" :fill="BLUE" />
        <!-- head -->
        <circle cx="42" cy="118" r="16" fill="#fff" :stroke="BLUE" stroke-width="2" />
        <!-- eyes -->
        <circle cx="36" cy="116" r="2.2" :fill="BLUE" />
        <circle cx="48" cy="116" r="2.2" :fill="BLUE" />
        <!-- blush -->
        <circle cx="32" cy="122" r="2.5" fill="#f3c1bc" opacity="0.7" />
        <circle cx="52" cy="122" r="2.5" fill="#f3c1bc" opacity="0.7" />
        <!-- smile -->
        <path d="M37 124 Q42 128 47 124" fill="none" :stroke="BLUE" stroke-width="1.4" stroke-linecap="round" />
        <!-- arms pulling -->
        <g class="ad__arm ad__arm--atk">
          <path d="M50 140 Q68 138 78 128" fill="none" :stroke="BLUE" stroke-width="3" stroke-linecap="round" />
          <circle cx="78" cy="128" r="3.5" :fill="BLUE" />
        </g>
        <text x="42" y="198" text-anchor="middle" class="ad__label">攻撃</text>
      </g>

      <!-- Defender character (right) -->
      <g class="ad__char ad__char--def">
        <ellipse cx="318" cy="168" rx="16" ry="4" fill="#cfdbed" />
        <rect x="308" y="132" width="20" height="28" rx="10" :fill="SUB" />
        <circle cx="318" cy="118" r="16" fill="#fff" :stroke="SUB" stroke-width="2" />
        <circle cx="312" cy="116" r="2.2" :fill="BLUE" />
        <circle cx="324" cy="116" r="2.2" :fill="BLUE" />
        <circle cx="308" cy="122" r="2.5" fill="#f3c1bc" opacity="0.7" />
        <circle cx="328" cy="122" r="2.5" fill="#f3c1bc" opacity="0.7" />
        <path d="M313 124 Q318 128 323 124" fill="none" :stroke="BLUE" stroke-width="1.4" stroke-linecap="round" />
        <!-- arm reinforcing -->
        <g class="ad__arm ad__arm--def">
          <path d="M310 140 Q292 136 282 128" fill="none" :stroke="SUB" stroke-width="3" stroke-linecap="round" />
          <circle cx="282" cy="128" r="3.5" :fill="SUB" />
        </g>
        <text x="318" y="198" text-anchor="middle" class="ad__label">守備</text>
      </g>

      <!-- Graph -->
      <g class="ad__graph">
        <!-- left half -->
        <g class="ad__half ad__half--l">
          <line x1="110" y1="70" x2="148" y2="52" :stroke="SOFT" stroke-width="2.2" stroke-linecap="round" />
          <line x1="110" y1="70" x2="130" y2="108" :stroke="SOFT" stroke-width="2.2" stroke-linecap="round" />
          <line x1="148" y1="52" x2="130" y2="108" :stroke="SOFT" stroke-width="2.2" stroke-linecap="round" />
          <line class="ad__weak" x1="148" y1="52" x2="176" y2="58" :stroke="WEAK" stroke-linecap="round" />
          <line class="ad__strong" x1="130" y1="108" x2="176" y2="112" :stroke="BLUE" stroke-linecap="round" />
          <!-- cute nodes -->
          <g class="ad__node" transform="translate(110,70)">
            <circle r="11" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
            <circle cx="-3.5" cy="-1" r="1.6" :fill="BLUE" />
            <circle cx="3.5" cy="-1" r="1.6" :fill="BLUE" />
            <path d="M-3 4 Q0 6.5 3 4" fill="none" :stroke="BLUE" stroke-width="1.1" stroke-linecap="round" />
          </g>
          <g class="ad__node" transform="translate(148,52)">
            <circle r="11" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
            <circle cx="-3.5" cy="-1" r="1.6" :fill="BLUE" />
            <circle cx="3.5" cy="-1" r="1.6" :fill="BLUE" />
            <path d="M-3 4 Q0 6.5 3 4" fill="none" :stroke="BLUE" stroke-width="1.1" stroke-linecap="round" />
          </g>
          <g class="ad__node" transform="translate(130,108)">
            <circle r="11" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
            <circle cx="-3.5" cy="-1" r="1.6" :fill="BLUE" />
            <circle cx="3.5" cy="-1" r="1.6" :fill="BLUE" />
            <path d="M-3 4 Q0 6.5 3 4" fill="none" :stroke="BLUE" stroke-width="1.1" stroke-linecap="round" />
          </g>
        </g>

        <!-- right half -->
        <g class="ad__half ad__half--r">
          <line x1="212" y1="52" x2="250" y2="70" :stroke="SOFT" stroke-width="2.2" stroke-linecap="round" />
          <line x1="250" y1="70" x2="230" y2="108" :stroke="SOFT" stroke-width="2.2" stroke-linecap="round" />
          <line x1="212" y1="52" x2="230" y2="108" :stroke="SOFT" stroke-width="2.2" stroke-linecap="round" />
          <line class="ad__weak" x1="184" y1="58" x2="212" y2="52" :stroke="WEAK" stroke-linecap="round" />
          <line class="ad__strong" x1="184" y1="112" x2="230" y2="108" :stroke="BLUE" stroke-linecap="round" />
          <g class="ad__node" transform="translate(212,52)">
            <circle r="11" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
            <circle cx="-3.5" cy="-1" r="1.6" :fill="BLUE" />
            <circle cx="3.5" cy="-1" r="1.6" :fill="BLUE" />
            <path d="M-3 4 Q0 6.5 3 4" fill="none" :stroke="BLUE" stroke-width="1.1" stroke-linecap="round" />
          </g>
          <g class="ad__node" transform="translate(250,70)">
            <circle r="11" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
            <circle cx="-3.5" cy="-1" r="1.6" :fill="BLUE" />
            <circle cx="3.5" cy="-1" r="1.6" :fill="BLUE" />
            <path d="M-3 4 Q0 6.5 3 4" fill="none" :stroke="BLUE" stroke-width="1.1" stroke-linecap="round" />
          </g>
          <g class="ad__node" transform="translate(230,108)">
            <circle r="11" fill="#fff" :stroke="BLUE" stroke-width="1.8" />
            <circle cx="-3.5" cy="-1" r="1.6" :fill="BLUE" />
            <circle cx="3.5" cy="-1" r="1.6" :fill="BLUE" />
            <path d="M-3 4 Q0 6.5 3 4" fill="none" :stroke="BLUE" stroke-width="1.1" stroke-linecap="round" />
          </g>
        </g>

        <!-- cut mark on weak bridge -->
        <g class="ad__cut">
          <line x1="170" y1="48" x2="186" y2="64" :stroke="WEAK" stroke-width="2.2" stroke-linecap="round" />
          <line x1="170" y1="64" x2="186" y2="48" :stroke="WEAK" stroke-width="2.2" stroke-linecap="round" />
        </g>

        <!-- sparkles when defended -->
        <g class="ad__spark">
          <circle cx="180" cy="100" r="2" :fill="BLUE" />
          <circle cx="168" cy="120" r="1.5" :fill="SUB" />
          <circle cx="192" cy="118" r="1.5" :fill="SUB" />
        </g>
      </g>

      <!-- pull arrows above graph -->
      <g class="ad__pull ad__pull--l">
        <line x1="128" y1="28" x2="98" y2="28" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="90,28 102,22 102,34" :fill="BLUE" />
        <text x="110" y="20" text-anchor="middle" class="ad__tiny">引く</text>
      </g>
      <g class="ad__pull ad__pull--r">
        <line x1="232" y1="28" x2="262" y2="28" :stroke="BLUE" stroke-width="2.2" stroke-linecap="round" />
        <polygon points="270,28 258,22 258,34" :fill="BLUE" />
        <text x="250" y="20" text-anchor="middle" class="ad__tiny">引く</text>
      </g>

      <!-- reinforce badge -->
      <g class="ad__badge">
        <rect x="148" y="128" width="64" height="18" rx="9" fill="#e4ebf6" :stroke="BLUE" stroke-width="1.2" />
        <text x="180" y="140" text-anchor="middle" class="ad__badge-text">厚くする!</text>
      </g>
    </svg>

    <div class="ad__steps">
      <span class="ad__step ad__step--1"><i>1</i>守備が辺に配分</span>
      <span class="ad__step ad__step--2"><i>2</i>攻撃が左右へ引く</span>
      <span class="ad__step ad__step--3"><i>3</i>細い辺に応力</span>
      <span class="ad__step ad__step--4"><i>4</i>切れそうになる</span>
      <span class="ad__step ad__step--5"><i>5</i>危ない辺を厚く</span>
    </div>
  </div>
</template>

<style scoped>
.ad {
  width: 100%;
  max-width: 22rem;
  margin: 0 auto;
  color: #1c3177;
  font-size: 0.78rem;
  line-height: 1.3;
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
.ad__label {
  font-size: 11px;
  fill: #1c3177;
  font-weight: 700;
  font-family: system-ui, sans-serif;
}
.ad__tiny {
  font-size: 9px;
  fill: #7f96c2;
  font-family: system-ui, sans-serif;
}
.ad__badge-text {
  font-size: 10px;
  fill: #1c3177;
  font-weight: 700;
  font-family: system-ui, sans-serif;
}
.ad__steps {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.3rem;
  margin-top: 0.2rem;
}
.ad__step {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  font-size: 0.65rem;
  color: #64748b;
  background: #f9fbff;
  border: 1px solid #cfdbed;
  border-radius: 999px;
  padding: 0.12rem 0.45rem 0.12rem 0.2rem;
}
.ad__step i {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1rem;
  height: 1rem;
  border-radius: 999px;
  background: #e4ebf6;
  color: #1c3177;
  font-style: normal;
  font-weight: 700;
  font-size: 0.6rem;
}

/* ===== 8s story loop =====
   0–12%   idle / defend allocate
   12–32%  attack pulls
   32–48%  stress on weak
   48–62%  about to snap
   62–86%  defend thickens
   86–100% hold / reset
*/
.ad__char--atk { animation: char-atk 8s ease-in-out infinite; }
.ad__char--def { animation: char-def 8s ease-in-out infinite; }
.ad__arm--atk { animation: arm-atk 8s ease-in-out infinite; transform-origin: 50px 140px; }
.ad__arm--def { animation: arm-def 8s ease-in-out infinite; transform-origin: 310px 140px; }
.ad__half--l { animation: half-l 8s ease-in-out infinite; }
.ad__half--r { animation: half-r 8s ease-in-out infinite; }
.ad__pull--l { animation: pull-l 8s ease-in-out infinite; }
.ad__pull--r { animation: pull-r 8s ease-in-out infinite; }
.ad__weak {
  stroke-width: 1.6;
  animation: weak 8s ease-in-out infinite;
}
.ad__strong {
  stroke-width: 2.4;
  animation: strong 8s ease-in-out infinite;
}
.ad__cut { animation: cut 8s ease-in-out infinite; transform-origin: 178px 56px; }
.ad__spark { animation: spark 8s ease-in-out infinite; }
.ad__badge { animation: badge 8s ease-in-out infinite; transform-origin: 180px 137px; }
.ad__step--1 { animation: s1 8s ease-in-out infinite; }
.ad__step--2 { animation: s2 8s ease-in-out infinite; }
.ad__step--3 { animation: s3 8s ease-in-out infinite; }
.ad__step--4 { animation: s4 8s ease-in-out infinite; }
.ad__step--5 { animation: s5 8s ease-in-out infinite; }

@keyframes char-atk {
  0%, 10% { transform: translateX(0); }
  18%, 58% { transform: translateX(-5px); }
  70%, 100% { transform: translateX(0); }
}
@keyframes char-def {
  0%, 58% { transform: translateX(0); }
  68%, 86% { transform: translateX(4px) scale(1.04); }
  100% { transform: translateX(0) scale(1); }
}
@keyframes arm-atk {
  0%, 10% { transform: rotate(0deg); }
  18%, 55% { transform: rotate(-12deg); }
  70%, 100% { transform: rotate(0deg); }
}
@keyframes arm-def {
  0%, 60% { transform: rotate(0deg); }
  68%, 86% { transform: rotate(10deg); }
  100% { transform: rotate(0deg); }
}
@keyframes half-l {
  0%, 12% { transform: translateX(0); }
  22%, 58% { transform: translateX(-11px); }
  72%, 100% { transform: translateX(0); }
}
@keyframes half-r {
  0%, 12% { transform: translateX(0); }
  22%, 58% { transform: translateX(11px); }
  72%, 100% { transform: translateX(0); }
}
@keyframes pull-l {
  0%, 10% { opacity: 0; transform: translateX(0); }
  16%, 55% { opacity: 1; transform: translateX(-8px); }
  68%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes pull-r {
  0%, 10% { opacity: 0; transform: translateX(0); }
  16%, 55% { opacity: 1; transform: translateX(8px); }
  68%, 100% { opacity: 0; transform: translateX(0); }
}
@keyframes weak {
  0%, 20% { stroke-width: 1.6; opacity: 0.85; stroke-dasharray: none; }
  32%, 45% { stroke-width: 2.2; opacity: 1; }
  50%, 60% { stroke-width: 1; opacity: 0.25; stroke-dasharray: 3 4; }
  72%, 100% { stroke-width: 1.6; opacity: 0.85; stroke-dasharray: none; }
}
@keyframes strong {
  0%, 58% { stroke-width: 2.4; }
  68%, 88% { stroke-width: 7; }
  100% { stroke-width: 2.4; }
}
@keyframes cut {
  0%, 46% { opacity: 0; transform: scale(0.5); }
  52%, 60% { opacity: 1; transform: scale(1); }
  70%, 100% { opacity: 0; transform: scale(0.5); }
}
@keyframes spark {
  0%, 66% { opacity: 0; transform: scale(0.5); }
  72%, 84% { opacity: 1; transform: scale(1.2); }
  100% { opacity: 0; transform: scale(0.5); }
}
@keyframes badge {
  0%, 64% { opacity: 0; transform: scale(0.7) translateY(4px); }
  70%, 86% { opacity: 1; transform: scale(1) translateY(0); }
  100% { opacity: 0; transform: scale(0.7) translateY(4px); }
}
@keyframes s1 {
  0%, 12% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  18%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s2 {
  0%, 12% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  16%, 30% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  40%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s3 {
  0%, 28% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  32%, 46% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  55%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s4 {
  0%, 46% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  50%, 60% { border-color: #b42318; color: #b42318; background: #fff5f4; }
  70%, 100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
@keyframes s5 {
  0%, 62% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
  68%, 86% { border-color: #1c3177; color: #1c3177; background: #e4ebf6; }
  100% { border-color: #cfdbed; color: #64748b; background: #f9fbff; }
}
</style>
