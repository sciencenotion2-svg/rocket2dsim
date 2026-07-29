[rocket-engine-simulator.html](https://github.com/user-attachments/files/30485247/rocket-engine-simulator.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LPRE-1 // Liquid Engine Test Stand</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Big+Shoulders:wght@400;600;700&family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#12161b;
    --panel:#1a2029;
    --panel2:#20272f;
    --border:#2c3641;
    --text:#e7ebee;
    --dim:#7f8b98;
    --ox:#4fa8e0;
    --fuel:#d7a94b;
    --flame:#ff5a1f;
    --flame2:#ffcf6b;
    --good:#5ad685;
    --warn:#e6c94a;
    --bad:#e0553f;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;background:var(--bg);color:var(--text);font-family:'IBM Plex Sans',sans-serif;}
  ::selection{background:var(--flame);color:#12161b;}
  .wrap{max-width:1180px;margin:0 auto;padding:28px 20px 80px;}
  header{display:flex;justify-content:space-between;align-items:flex-end;gap:16px;flex-wrap:wrap;border-bottom:1px solid var(--border);padding-bottom:18px;margin-bottom:22px;}
  .brand{display:flex;flex-direction:column;gap:2px;}
  .brand .eyebrow{font-family:'IBM Plex Mono',monospace;font-size:11px;letter-spacing:.14em;color:var(--dim);text-transform:uppercase;}
  h1{font-family:'Big Shoulders',sans-serif;font-weight:700;font-size:clamp(28px,4vw,42px);margin:0;letter-spacing:.01em;text-transform:uppercase;line-height:1;}
  .status{display:flex;align-items:center;gap:8px;font-family:'IBM Plex Mono',monospace;font-size:12px;color:var(--dim);}
  .status .dot{width:8px;height:8px;border-radius:50%;background:var(--good);box-shadow:0 0 8px var(--good);}
  .status.unstable .dot{background:var(--warn);box-shadow:0 0 8px var(--warn);}
  .status.bad .dot{background:var(--bad);box-shadow:0 0 8px var(--bad);}
  .hero{display:grid;grid-template-columns:1.3fr 1fr;gap:20px;margin-bottom:22px;}
  @media (max-width:860px){.hero{grid-template-columns:1fr;}}
  .panel{background:var(--panel);border:1px solid var(--border);border-radius:6px;padding:18px 20px;}
  .panel h2{font-family:'IBM Plex Mono',monospace;font-size:11px;letter-spacing:.14em;text-transform:uppercase;color:var(--dim);margin:0 0 14px;}
  .diagram-panel{position:relative;padding:10px 10px 16px;overflow:hidden;}
  .diagram-panel svg{width:100%;height:auto;display:block;}
  .thrust-overlay{position:absolute;top:20px;left:24px;font-family:'IBM Plex Mono',monospace;}
  .thrust-overlay .big{font-family:'Big Shoulders',sans-serif;font-size:44px;font-weight:700;line-height:1;color:var(--flame2);text-shadow:0 0 18px rgba(255,90,31,.35);}
  .thrust-overlay .label{font-size:11px;color:var(--dim);letter-spacing:.1em;text-transform:uppercase;margin-top:2px;}
  .controls .row{margin-bottom:16px;}
  .controls label{display:flex;justify-content:space-between;font-family:'IBM Plex Mono',monospace;font-size:12px;color:var(--dim);margin-bottom:6px;}
  .controls label span.val{color:var(--text);font-weight:500;}
  select, input[type=range]{width:100%;}
  select{background:var(--panel2);color:var(--text);border:1px solid var(--border);border-radius:4px;padding:8px 10px;font-family:'IBM Plex Mono',monospace;font-size:12.5px;}
  input[type=range]{appearance:none;-webkit-appearance:none;height:4px;background:var(--border);border-radius:2px;outline:none;}
  input[type=range]::-webkit-slider-thumb{appearance:none;-webkit-appearance:none;width:16px;height:16px;border-radius:50%;background:var(--flame2);border:2px solid #12161b;cursor:pointer;box-shadow:0 0 6px rgba(255,207,107,.6);}
  input[type=range]::-moz-range-thumb{width:16px;height:16px;border-radius:50%;background:var(--flame2);border:2px solid #12161b;cursor:pointer;}
  .segmented{display:flex;gap:6px;}
  .segmented button{flex:1;background:var(--panel2);border:1px solid var(--border);color:var(--dim);font-family:'IBM Plex Mono',monospace;font-size:11.5px;padding:8px 6px;border-radius:4px;cursor:pointer;}
  .segmented button.active{background:rgba(255,90,31,.12);border-color:var(--flame);color:var(--flame2);}
  .telemetry{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-bottom:22px;}
  @media (max-width:760px){.telemetry{grid-template-columns:repeat(2,1fr);}}
  .tcell{background:var(--panel);border:1px solid var(--border);border-radius:6px;padding:14px 16px;}
  .tcell .k{font-family:'IBM Plex Mono',monospace;font-size:10.5px;color:var(--dim);letter-spacing:.1em;text-transform:uppercase;}
  .tcell .v{font-family:'Big Shoulders',sans-serif;font-size:26px;font-weight:600;margin-top:4px;color:var(--text);}
  .tcell .v span.unit{font-family:'IBM Plex Mono',monospace;font-size:12px;color:var(--dim);font-weight:400;margin-left:4px;}
  .expansion-note{font-family:'IBM Plex Mono',monospace;font-size:12px;padding:12px 16px;border-radius:6px;border:1px solid var(--border);background:var(--panel2);color:var(--dim);margin-bottom:22px;}
  .expansion-note b{color:var(--text);}
  footer{font-family:'IBM Plex Mono',monospace;font-size:11.5px;color:var(--dim);border-top:1px solid var(--border);padding-top:16px;line-height:1.7;}
  footer code{color:var(--text);}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="brand">
      <div class="eyebrow">Test Stand Console / Bipropellant</div>
      <h1>LPRE&#8209;1 Engine Simulator</h1>
    </div>
    <div class="status" id="statusBox"><div class="dot"></div><span id="statusText">NOMINAL &mdash; STEADY STATE</span></div>
  </header>

  <div class="hero">
    <div class="panel diagram-panel">
      <div class="thrust-overlay">
        <div class="big" id="thrustBig">0</div>
        <div class="label">Thrust, kN</div>
      </div>
      <svg viewBox="0 0 640 360" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="metalGrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="#3a4550"/>
            <stop offset="50%" stop-color="#232b33"/>
            <stop offset="100%" stop-color="#3a4550"/>
          </linearGradient>
          <linearGradient id="flameGrad" x1="0" y1="0" x2="1" y2="0">
            <stop offset="0%" stop-color="#fff3c4"/>
            <stop offset="18%" stop-color="#ffcf6b"/>
            <stop offset="55%" stop-color="#ff5a1f"/>
            <stop offset="100%" stop-color="#ff5a1f" stop-opacity="0"/>
          </linearGradient>
        </defs>
        <rect x="70" y="120" width="26" height="120" fill="url(#metalGrad)" stroke="#101418" stroke-width="1.5"/>
        <line x1="83" y1="126" x2="83" y2="234" stroke="#101418" stroke-width="1" stroke-dasharray="4 4"/>
        <rect x="30" y="128" width="42" height="14" fill="none" stroke="var(--ox)" stroke-width="2.5" id="oxLine"/>
        <text x="18" y="122" font-family="IBM Plex Mono" font-size="10" fill="#4fa8e0">LOX</text>
        <rect x="30" y="218" width="42" height="14" fill="none" stroke="var(--fuel)" stroke-width="2.5" id="fuelLine"/>
        <text x="12" y="245" font-family="IBM Plex Mono" font-size="10" fill="#d7a94b">FUEL</text>
        <path d="M96,120 L200,120 L200,240 L96,240 Z" fill="url(#metalGrad)" stroke="#101418" stroke-width="1.5"/>
        <path d="M200,120 L266,150 L266,210 L200,240 Z" fill="url(#metalGrad)" stroke="#101418" stroke-width="1.5"/>
        <rect x="264" y="150" width="10" height="60" fill="#171d23" stroke="#101418" stroke-width="1.5"/>
        <path id="nozzlePath" d="M274,150 L470,90 L470,270 L274,210 Z" fill="url(#metalGrad)" stroke="#101418" stroke-width="1.5"/>
        <ellipse id="chamberGlow" cx="150" cy="180" rx="55" ry="50" fill="#ff5a1f" opacity="0.55"/>
        <g id="plumeGroup">
          <path id="plumeMain" d="" fill="url(#flameGrad)" opacity="0.9"/>
          <g id="diamonds"></g>
        </g>
        <g transform="translate(520,60)">
          <circle r="34" fill="#171d23" stroke="#2c3641" stroke-width="2"/>
          <text y="-42" text-anchor="middle" font-family="IBM Plex Mono" font-size="9" fill="#7f8b98">CHAMBER Pc</text>
          <line id="pcNeedle" x1="0" y1="0" x2="0" y2="-24" stroke="#ffcf6b" stroke-width="2.5" stroke-linecap="round"/>
          <circle r="3" fill="#ffcf6b"/>
        </g>
        <g transform="translate(520,150)">
          <circle r="34" fill="#171d23" stroke="#2c3641" stroke-width="2"/>
          <text y="-42" text-anchor="middle" font-family="IBM Plex Mono" font-size="9" fill="#7f8b98">MASS FLOW</text>
          <line id="mdotNeedle" x1="0" y1="0" x2="0" y2="-24" stroke="#4fa8e0" stroke-width="2.5" stroke-linecap="round"/>
          <circle r="3" fill="#4fa8e0"/>
        </g>
        <g transform="translate(520,240)">
          <circle r="34" fill="#171d23" stroke="#2c3641" stroke-width="2"/>
          <text y="-42" text-anchor="middle" font-family="IBM Plex Mono" font-size="9" fill="#7f8b98">EXIT MACH</text>
          <line id="machNeedle" x1="0" y1="0" x2="0" y2="-24" stroke="#5ad685" stroke-width="2.5" stroke-linecap="round"/>
          <circle r="3" fill="#5ad685"/>
        </g>
        <text x="150" y="270" text-anchor="middle" font-family="IBM Plex Mono" font-size="10" fill="#7f8b98">CHAMBER</text>
        <text x="470" y="300" text-anchor="middle" font-family="IBM Plex Mono" font-size="10" fill="#7f8b98">NOZZLE EXIT</text>
      </svg>
    </div>

    <div class="panel controls">
      <h2>Engine Configuration</h2>
      <div class="row">
        <label>Propellant combination</label>
        <select id="propSelect">
          <option value="loxrp1">LOX / RP&#8209;1 (kerosene)</option>
          <option value="loxlh2">LOX / LH&#8322; (hydrogen)</option>
          <option value="n2o4udmh">N&#8322;O&#8324; / UDMH (storable)</option>
        </select>
      </div>
      <div class="row">
        <label>Chamber pressure Pc <span class="val" id="pcVal">8.0 MPa</span></label>
        <input type="range" id="pcSlider" min="2" max="22" step="0.1" value="8">
      </div>
      <div class="row">
        <label>Mixture ratio O/F <span class="val" id="ofVal">2.56</span></label>
        <input type="range" id="ofSlider" min="1" max="7" step="0.02" value="2.56">
      </div>
      <div class="row">
        <label>Throat diameter <span class="val" id="throatVal">80 mm</span></label>
        <input type="range" id="throatSlider" min="20" max="300" step="1" value="80">
      </div>
      <div class="row">
        <label>Expansion ratio &epsilon; = Ae/At <span class="val" id="epsVal">12.0</span></label>
        <input type="range" id="epsSlider" min="2" max="80" step="0.5" value="12">
      </div>
      <div class="row">
        <label>Ambient environment</label>
        <div class="segmented" id="ambientSeg">
          <button data-p="101325" class="active">Sea level</button>
          <button data-p="20000">High altitude</button>
          <button data-p="0">Vacuum</button>
        </div>
      </div>
    </div>
  </div>

  <div class="telemetry">
    <div class="tcell"><div class="k">Specific Impulse</div><div class="v" id="ispOut">0<span class="unit">s</span></div></div>
    <div class="tcell"><div class="k">Exhaust Velocity Ve</div><div class="v" id="veOut">0<span class="unit">m/s</span></div></div>
    <div class="tcell"><div class="k">Mass Flow Rate</div><div class="v" id="mdotOut">0<span class="unit">kg/s</span></div></div>
    <div class="tcell"><div class="k">Characteristic Vel. c*</div><div class="v" id="cstarOut">0<span class="unit">m/s</span></div></div>
    <div class="tcell"><div class="k">Exit Pressure Pe</div><div class="v" id="peOut">0<span class="unit">kPa</span></div></div>
    <div class="tcell"><div class="k">Exit Mach Number</div><div class="v" id="machOut">0<span class="unit">M</span></div></div>
    <div class="tcell"><div class="k">Chamber Temp Tc</div><div class="v" id="tcOut">0<span class="unit">K</span></div></div>
    <div class="tcell"><div class="k">Throat Area At</div><div class="v" id="atOut">0<span class="unit">cm&sup2;</span></div></div>
  </div>

  <div class="expansion-note" id="expansionNote">Calculating expansion regime&hellip;</div>

  <footer>
    Model: 1D isentropic nozzle flow with fixed propellant properties (k, T<sub>c</sub>, molar mass, c*) per combination &mdash; not a combustion-chemistry solver.
    <br>
    F = &#7745;&middot;Ve + (Pe&minus;Pa)&middot;Ae &nbsp;|&nbsp; Isp = F / (&#7745;&middot;g&#8320;) &nbsp;|&nbsp; &#7745; = Pc&middot;At / c* &nbsp;|&nbsp; Ve from isentropic energy balance.
    <br>
    Educational approximation &mdash; not for hardware design use.
  </footer>
</div>

<script>
const G0 = 9.80665;
const RU = 8314.5;

const PROPS = {
  loxrp1:    { name:"LOX/RP-1",  k:1.22, Tc:3670, M:21.9, cstar:1715, ofOptimal:2.56, ofRange:[1.8,3.4] },
  loxlh2:    { name:"LOX/LH2",   k:1.20, Tc:3540, M:13.0, cstar:2380, ofOptimal:5.5,  ofRange:[3.5,7.0] },
  n2o4udmh: { name:"N2O4/UDMH", k:1.24, Tc:3200, M:24.0, cstar:1730, ofOptimal:2.6,  ofRange:[1.6,3.2] }
};

let propSelect, pcSlider, ofSlider, throatSlider, epsSlider, ambientSeg;
let Pa = 101325;

function solveExitMach(eps, k){
  function areaRatio(M){
    const term = (2/(k+1)) * (1 + (k-1)/2 * M*M);
    return (1/M) * Math.pow(term, (k+1)/(2*(k-1)));
  }
  let lo = 1.0001, hi = 30;
  while(areaRatio(hi) < eps && hi < 1000){ hi *= 1.5; }
  for(let i=0;i<80;i++){
    const mid = (lo+hi)/2;
    if(Math.abs(hi - lo) < 1e-6) break;
    if(areaRatio(mid) < eps) lo = mid; else hi = mid;
  }
  return (lo+hi)/2;
}

function fmt(n, d=0){
  return n.toLocaleString(undefined, {minimumFractionDigits:d, maximumFractionDigits:d});
}

function update(){
  if(!propSelect || !pcSlider || !ofSlider || !throatSlider || !epsSlider || !ambientSeg) return;

  const prop = PROPS[propSelect.value];
  const Pc = parseFloat(pcSlider.value) * 1e6;
  const OF = parseFloat(ofSlider.value);
  const throatD = parseFloat(throatSlider.value) / 1000;
  const eps = parseFloat(epsSlider.value);

  document.getElementById('pcVal').textContent = pcSlider.value + ' MPa';
  document.getElementById('ofVal').textContent = OF.toFixed(2);
  document.getElementById('throatVal').textContent = throatSlider.value + ' mm';
  document.getElementById('epsVal').textContent = eps.toFixed(1);

  const k = prop.k;
  const Tc = prop.Tc;
  const Rspecific = RU / prop.M;
  const cstar = prop.cstar;

  const At = Math.PI * Math.pow(throatD/2, 2);
  const Ae = At * eps;

  const Me = solveExitMach(eps, k);
  const PeOverPc = Math.pow(1 + (k-1)/2 * Me*Me, -k/(k-1));
  const Pe = PeOverPc * Pc;

  const Ve = Math.sqrt( (2*k/(k-1)) * Rspecific * Tc * (1 - Math.pow(PeOverPc, (k-1)/k)) );

  const mdot = (Pc * At) / cstar;
  const F = mdot * Ve + (Pe - Pa) * Ae;
  const Isp = F / (mdot * G0);

  document.getElementById('ispOut').innerHTML = fmt(Isp) + '<span class="unit">s</span>';
  document.getElementById('veOut').innerHTML = fmt(Ve) + '<span class="unit">m/s</span>';
  document.getElementById('mdotOut').innerHTML = fmt(mdot,2) + '<span class="unit">kg/s</span>';
  document.getElementById('cstarOut').innerHTML = fmt(cstar) + '<span class="unit">m/s</span>';
  document.getElementById('peOut').innerHTML = fmt(Pe/1000) + '<span class="unit">kPa</span>';
  document.getElementById('machOut').innerHTML = Me.toFixed(2) + '<span class="unit">M</span>';
  document.getElementById('tcOut').innerHTML = fmt(Tc) + '<span class="unit">K</span>';
  document.getElementById('atOut').innerHTML = fmt(At*10000,1) + '<span class="unit">cm&sup2;</span>';

  document.getElementById('thrustBig').textContent = fmt(F/1000, F/1000<100?1:0);

  const ratio = Pe / Pa;
  let regime, statusClass, statusText;
  if(Pa < 1000){
    regime = "Vacuum operation &mdash; plume expands freely, no shock structure.";
    statusClass=""; statusText="NOMINAL — VACUUM";
  } else if(ratio > 1.15){
    regime = "<b>Under-expanded:</b> Pe (" + fmt(Pe/1000) + " kPa) exceeds Pa &mdash; exhaust keeps expanding outside the nozzle, forming visible Mach diamonds.";
    statusClass=""; statusText="NOMINAL — UNDEREXPANDED";
  } else if(ratio < 0.4){
    regime = "<b>Severely over-expanded:</b> Pe is far below Pa &mdash; flow separation risk inside the nozzle. Consider a smaller expansion ratio.";
    statusClass="bad"; statusText="CAUTION — FLOW SEPARATION RISK";
  } else if(ratio < 0.85){
    regime = "<b>Over-expanded:</b> Pe (" + fmt(Pe/1000) + " kPa) is below Pa &mdash; oblique shocks compress the plume just outside the nozzle.";
    statusClass="unstable"; statusText="NOMINAL — OVEREXPANDED";
  } else {
    regime = "<b>Near-optimal expansion:</b> Pe &asymp; Pa &mdash; smooth, parallel exhaust plume at peak efficiency for this altitude.";
    statusClass=""; statusText="NOMINAL — OPTIMAL EXPANSION";
  }
  document.getElementById('expansionNote').innerHTML = regime;
  const statusBox = document.getElementById('statusBox');
  statusBox.className = 'status ' + statusClass;
  document.getElementById('statusText').textContent = statusText;

  const glow = document.getElementById('chamberGlow');
  const pcNorm = Math.min(1, Pc/22e6);
  glow.setAttribute('opacity', 0.35 + pcNorm*0.5);
  glow.setAttribute('rx', 48 + pcNorm*14);

  function setNeedle(id, frac){
    const angle = -110 + Math.max(0,Math.min(1,frac)) * 220;
    document.getElementById(id).setAttribute('transform', `rotate(${angle})`);
  }
  setNeedle('pcNeedle', Pc/22e6);
  setNeedle('mdotNeedle', Math.min(1, mdot/400));
  setNeedle('machNeedle', Math.min(1, Me/8));

  const plumeLenBase = Math.min(160, 90 + Math.min(140, F/6000));
  const flare = ratio > 1 ? Math.min(30, (ratio-1)*20) : 0;
  const neckIn = ratio < 0.85 ? Math.min(15, (0.85-ratio)*30) : 0;

  const x0 = 470, yTop = 90, yBot = 270;
  const xEnd = Math.min(630, x0 + plumeLenBase);
  const path = `M${x0},${yTop}
    C ${x0+40},${yTop-flare+neckIn} ${xEnd-30},${180-10-flare*0.6+neckIn} ${xEnd},180
    C ${xEnd-30},${180+10+flare*0.6-neckIn} ${x0+40},${yBot+flare-neckIn} ${x0},${yBot} Z`;
  document.getElementById('plumeMain').setAttribute('d', path);

  const dGroup = document.getElementById('diamonds');
  dGroup.innerHTML = '';
  if(ratio > 1.15){
    const n = Math.min(4, Math.round((ratio-1)*3)+1);
    for(let i=0;i<n;i++){
      const dx = x0 + 25 + i*20;
      if (dx < xEnd - 15) {
        dGroup.innerHTML += `<line x1="${dx}" y1="${170-8}" x2="${dx+10}" y2="180" stroke="#fff3c4" stroke-width="1.5" opacity="0.7"/>
          <line x1="${dx+10}" y1="180" x2="${dx}" y2="${190+8}" stroke="#fff3c4" stroke-width="1.5" opacity="0.7"/>`;
      }
    }
  }
}

function init(){
  propSelect = document.getElementById('propSelect');
  pcSlider = document.getElementById('pcSlider');
  ofSlider = document.getElementById('ofSlider');
  throatSlider = document.getElementById('throatSlider');
  epsSlider = document.getElementById('epsSlider');
  ambientSeg = document.getElementById('ambientSeg');

  if(!propSelect || !pcSlider || !ofSlider || !throatSlider || !epsSlider || !ambientSeg) return;

  ambientSeg.querySelectorAll('button').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      ambientSeg.querySelectorAll('button').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      Pa = parseFloat(btn.dataset.p);
      update();
    });
  });

  [propSelect, pcSlider, ofSlider, throatSlider, epsSlider].forEach(el=>{
    el.addEventListener('input', update);
  });

  propSelect.addEventListener('change', ()=>{
    const prop = PROPS[propSelect.value];
    ofSlider.min = prop.ofRange[0];
    ofSlider.max = prop.ofRange[1];
    ofSlider.value = prop.ofOptimal;
    update();
  });

  propSelect.dispatchEvent(new Event('change'));
  update();
}

if(document.readyState === 'loading'){
  document.addEventListener('DOMContentLoaded', init);
} else {
  init();
}
</script>
</body>
</html>
