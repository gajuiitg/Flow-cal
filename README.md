<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Orifice Flow Calculator</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
  :root{
    --bg:#f5f7fa;
    --card:#ffffff;
    --border:#e1e6ec;
    --text:#1f2937;
    --muted:#6b7280;
    --accent:#2563eb;
    --accent-soft:#eaf1ff;
    --good:#16a34a;
    --warn:#d97706;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    font-family:'Segoe UI',Roboto,Arial,sans-serif;
    background:var(--bg);
    color:var(--text);
    padding:20px;
  }
  .wrap{max-width:900px;margin:0 auto;}
  header{
    display:flex;
    justify-content:space-between;
    align-items:baseline;
    margin-bottom:16px;
    flex-wrap:wrap;
    gap:6px;
  }
  h1{font-size:1.3rem;margin:0;font-weight:600;}
  .sub{color:var(--muted);font-size:0.82rem;}
  .card{
    background:var(--card);
    border:1px solid var(--border);
    border-radius:10px;
    padding:16px 18px;
    margin-bottom:14px;
  }
  .card h2{
    font-size:0.85rem;
    text-transform:uppercase;
    letter-spacing:.04em;
    color:var(--muted);
    margin:0 0 12px 0;
    font-weight:600;
  }
  .grid{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(200px,1fr));
    gap:12px 16px;
  }
  .field label{
    display:block;
    font-size:0.78rem;
    color:var(--muted);
    margin-bottom:4px;
  }
  .row{display:flex;gap:6px;}
  input[type=number], select{
    width:100%;
    padding:7px 8px;
    border:1px solid var(--border);
    border-radius:6px;
    font-size:0.92rem;
    background:#fbfcfe;
    color:var(--text);
  }
  input[disabled]{background:#f0f2f5;color:var(--muted);}
  select.unit{max-width:110px;flex:0 0 auto;}
  input:focus, select:focus{outline:none;border-color:var(--accent);background:#fff;}
  .toggle-row{display:flex;gap:8px;margin-bottom:12px;flex-wrap:wrap;}
  .toggle-row button{
    border:1px solid var(--border);
    background:#fff;
    padding:6px 14px;
    border-radius:20px;
    font-size:0.82rem;
    cursor:pointer;
    color:var(--muted);
  }
  .toggle-row button.active{
    background:var(--accent);
    border-color:var(--accent);
    color:#fff;
  }
  .results{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
    gap:10px;
  }
  .result-box{
    background:var(--accent-soft);
    border-radius:8px;
    padding:12px 14px;
  }
  .result-box.primary{background:#e7f7ee;}
  .result-box.primary .value{color:var(--good);}
  .result-box .label{font-size:0.75rem;color:var(--muted);}
  .result-box .value{font-size:1.15rem;font-weight:700;color:var(--accent);margin-top:2px;}
  .result-box .value span{font-size:0.72rem;font-weight:500;color:var(--muted);}
  .diag{
    font-size:0.8rem;
    color:var(--muted);
    display:flex;
    flex-wrap:wrap;
    gap:14px;
    margin-top:10px;
    padding-top:10px;
    border-top:1px dashed var(--border);
  }
  .diag b{color:var(--text);}
  .warning{
    background:#fff7ed;
    border:1px solid #fed7aa;
    color:var(--warn);
    padding:8px 12px;
    border-radius:6px;
    font-size:0.8rem;
    margin-top:10px;
  }
  footer{
    text-align:center;
    color:var(--muted);
    font-size:0.72rem;
    margin-top:18px;
  }
  .note{font-size:0.72rem;color:var(--muted);margin-top:6px;}
  .diagram-wrap{background:#fbfcfe;border:1px solid var(--border);border-radius:8px;padding:10px 14px 6px;margin-bottom:14px;}
  .diagram-wrap svg{width:100%;height:auto;display:block;max-width:640px;margin:0 auto;}
  .diagram-cap{font-size:0.72rem;color:var(--muted);text-align:center;margin-top:4px;}
  .diagram-hidden{display:none;}
  .meter-section{display:none;}
  .meter-section.active{display:block;}
  .print-header{display:none;}
  .print-btn{
    border:1px solid var(--accent);
    background:var(--accent);
    color:#fff;
    padding:7px 16px;
    border-radius:8px;
    font-size:0.82rem;
    cursor:pointer;
  }
  @media print{
    @page{ size:A4; margin:9mm; }
    body{background:#fff;padding:0;font-size:9px;line-height:1.25;}
    .no-print, .toggle-row, header{display:none !important;}
    .print-header{display:block !important;margin-bottom:8px;padding-bottom:4px;border-bottom:1.5px solid #000;}
    .print-header .cat{font-size:12px;font-weight:700;}
    .print-header .ts{font-size:8px;color:#555;margin-top:1px;}
    .wrap{max-width:100%;}
    .card{border:1px solid #ccc;break-inside:avoid;box-shadow:none;padding:8px 10px;margin-bottom:8px;border-radius:4px;}
    .card h2{font-size:10px;margin:0 0 6px 0;}
    .grid{gap:6px 10px;}
    .field label{font-size:9px;margin-bottom:2px;}
    input[type=number], select{font-size:9px;padding:3px 5px;border-radius:4px;}
    .note{font-size:8px;line-height:1.2;margin-top:3px;}
    .results{gap:6px;}
    .result-box{padding:6px 8px;border-radius:5px;}
    .result-box .label{font-size:8px;}
    .result-box .value{font-size:11px;}
    .result-box .value span{font-size:8px;}
    .diag{font-size:8px;gap:8px;margin-top:6px;padding-top:6px;}
    .warning{font-size:8px;padding:5px 8px;}
    table{font-size:8px;}
    table th, table td{padding:2px 4px !important;}
    .diagram-wrap{break-inside:avoid;background:#fff;padding:6px 8px 3px;margin-bottom:8px;}
    .diagram-wrap svg{max-width:340px;}
    .diagram-cap{font-size:7.5px;margin-top:2px;}
    footer{font-size:7.5px;margin-top:8px;}
  }
</style>
</head>
<body>
<div class="wrap">
  <div class="print-header" id="printHeader"></div>

  <header>
    <div>
      <h1 id="pageTitle">Flow Meter Calculator</h1>
      <div class="sub" id="pageSub">Square-edge concentric orifice — ISO 5167-2 simplified form</div>
    </div>
    <div class="sub" id="betaTag">β = —</div>
  </header>

  <div class="card no-print">
    <div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px;">
      <h2 style="margin:0;">Flow Meter Category</h2>
      <button class="print-btn" onclick="window.print()">🖨️ Print this calculation</button>
    </div>
    <div class="toggle-row" id="categoryToggle" style="margin-top:10px;">
      <button data-cat="dp" class="active">Differential Pressure (Orifice/Venturi/Nozzle)</button>
      <button data-cat="coriolis">Mass Flow Meter (Coriolis/Thermal)</button>
      <button data-cat="pitot">Pitot Tube</button>
      <button data-cat="vmt">Vortex / Magnetic / Turbine</button>
      <button data-cat="oc">Open Channel (Weir/Flume)</button>
    </div>
    <div class="note">Only the selected category's calculation is shown on screen and included when you print — everything else stays out of the printout.</div>
  </div>

  <section class="meter-section active" id="sec-dp">
  <div class="card">
    <h2>Meter Type</h2>
    <div class="toggle-row" id="meterToggle">
      <button data-meter="orifice" class="active">Orifice Plate</button>
      <button data-meter="venturi">Classical Venturi Tube</button>
      <button data-meter="nozzle">Flow Nozzle</button>
    </div>

    <div class="diagram-wrap" id="diagram-orifice">
      <svg viewBox="0 0 640 190" xmlns="http://www.w3.org/2000/svg">
        <line x1="20" y1="55" x2="620" y2="55" stroke="#1f2937" stroke-width="2"/>
        <line x1="20" y1="135" x2="620" y2="135" stroke="#1f2937" stroke-width="2"/>
        <text x="30" y="45" font-size="13" fill="#2563eb">Flow →</text>
        <line x1="60" y1="55" x2="60" y2="135" stroke="#6b7280" stroke-width="1" stroke-dasharray="4,3"/>
        <text x="66" y="98" font-size="13" fill="#6b7280">D</text>
        <line x1="320" y1="55" x2="320" y2="85" stroke="#1f2937" stroke-width="5"/>
        <line x1="320" y1="105" x2="320" y2="135" stroke="#1f2937" stroke-width="5"/>
        <text x="330" y="98" font-size="13" fill="#1f2937">d</text>
        <line x1="280" y1="55" x2="280" y2="20" stroke="#d97706" stroke-width="1.5"/>
        <line x1="360" y1="55" x2="360" y2="20" stroke="#d97706" stroke-width="1.5"/>
        <line x1="280" y1="20" x2="360" y2="20" stroke="#d97706" stroke-width="1.5"/>
        <text x="295" y="14" font-size="13" fill="#d97706">ΔP</text>
        <text x="540" y="98" font-size="14" font-weight="700" fill="#16a34a">Q →</text>
      </svg>
      <div class="diagram-cap">Thin plate with a sharp-edged bore d in a pipe of diameter D. ΔP is read across flange/corner taps straddling the plate. Input: D, d, ΔP, ρ, Cd — Output: Q / ṁ.</div>
    </div>
    <div class="diagram-wrap diagram-hidden" id="diagram-venturi">
      <svg viewBox="0 0 640 190" xmlns="http://www.w3.org/2000/svg">
        <path d="M20,40 L230,40 L300,75 L340,75 L620,40" stroke="#1f2937" stroke-width="2" fill="none"/>
        <path d="M20,150 L230,150 L300,115 L340,115 L620,150" stroke="#1f2937" stroke-width="2" fill="none"/>
        <text x="30" y="30" font-size="13" fill="#2563eb">Flow →</text>
        <line x1="60" y1="40" x2="60" y2="150" stroke="#6b7280" stroke-width="1" stroke-dasharray="4,3"/>
        <text x="66" y="98" font-size="13" fill="#6b7280">D</text>
        <line x1="320" y1="75" x2="320" y2="115" stroke="#6b7280" stroke-width="1" stroke-dasharray="4,3"/>
        <text x="326" y="98" font-size="13" fill="#1f2937">d</text>
        <line x1="150" y1="40" x2="150" y2="15" stroke="#d97706" stroke-width="1.5"/>
        <line x1="320" y1="75" x2="320" y2="15" stroke="#d97706" stroke-width="1.5"/>
        <line x1="150" y1="15" x2="320" y2="15" stroke="#d97706" stroke-width="1.5"/>
        <text x="180" y="10" font-size="12" fill="#d97706">ΔP = P1 − P2</text>
        <text x="540" y="98" font-size="14" font-weight="700" fill="#16a34a">Q →</text>
      </svg>
      <div class="diagram-cap">Smooth converging throat d, then a gradual diffuser recovers most of the pressure back toward D. P1 upstream, P2 at the throat. Input: D, d, ΔP, ρ, Cd — Output: Q / ṁ + low permanent loss.</div>
    </div>
    <div class="diagram-wrap diagram-hidden" id="diagram-nozzle">
      <svg viewBox="0 0 640 190" xmlns="http://www.w3.org/2000/svg">
        <path d="M20,40 L260,40 L330,72 L400,72" stroke="#1f2937" stroke-width="2" fill="none"/>
        <path d="M20,150 L260,150 L330,118 L400,118" stroke="#1f2937" stroke-width="2" fill="none"/>
        <line x1="400" y1="72" x2="620" y2="40" stroke="#1f2937" stroke-width="2" stroke-dasharray="5,4"/>
        <line x1="400" y1="118" x2="620" y2="150" stroke="#1f2937" stroke-width="2" stroke-dasharray="5,4"/>
        <text x="30" y="30" font-size="13" fill="#2563eb">Flow →</text>
        <text x="410" y="100" font-size="10" fill="#dc2626">sudden re-expansion, no diffuser</text>
        <line x1="60" y1="40" x2="60" y2="150" stroke="#6b7280" stroke-width="1" stroke-dasharray="4,3"/>
        <text x="66" y="98" font-size="13" fill="#6b7280">D</text>
        <line x1="365" y1="72" x2="365" y2="118" stroke="#6b7280" stroke-width="1" stroke-dasharray="4,3"/>
        <text x="371" y="98" font-size="13" fill="#1f2937">d</text>
        <line x1="150" y1="40" x2="150" y2="15" stroke="#d97706" stroke-width="1.5"/>
        <line x1="365" y1="72" x2="365" y2="15" stroke="#d97706" stroke-width="1.5"/>
        <line x1="150" y1="15" x2="365" y2="15" stroke="#d97706" stroke-width="1.5"/>
        <text x="200" y="10" font-size="12" fill="#d97706">ΔP = P1 − P2</text>
        <text x="540" y="98" font-size="14" font-weight="700" fill="#16a34a">Q →</text>
      </svg>
      <div class="diagram-cap">Smooth elliptical/conical converging section to throat d, but no diffuser — flow re-expands abruptly downstream. Good at high velocity/temperature (e.g. steam). Input: D, d, ΔP, ρ, Cd — Output: Q / ṁ.</div>
    </div>
    <h2 style="margin-top:14px;">Calculation Mode</h2>
    <div class="toggle-row" id="modeToggle">
      <button data-mode="flow" class="active">Flow rate (from meter size)</button>
      <button data-mode="size">Meter size (from flow rate)</button>
    </div>
    <h2 style="margin-top:14px;">Fluid</h2>
    <div class="toggle-row" id="fluidToggle">
      <button data-fluid="liquid" class="active">Liquid</button>
      <button data-fluid="gas">Gas / Vapour</button>
    </div>
    <div class="grid">
      <div class="field">
        <label>Density at operating conditions</label>
        <div class="row">
          <input type="number" id="rho" value="850" step="any">
          <select class="unit" id="rhoUnit">
            <option value="1">kg/m³</option>
            <option value="0.001">g/cm³</option>
          </select>
        </div>
      </div>
      <div class="field">
        <label>Dynamic viscosity, μ</label>
        <div class="row">
          <input type="number" id="mu" value="1.0" step="any">
          <select class="unit" id="muUnit">
            <option value="0.001">cP (mPa·s)</option>
            <option value="1">Pa·s</option>
          </select>
        </div>
      </div>
      <div class="field" id="kappaField" style="display:none;">
        <label>Isentropic exponent, κ (Cp/Cv)</label>
        <input type="number" id="kappa" value="1.30" step="0.01">
      </div>
      <div class="field" id="tempField" style="display:none;">
        <label>Operating temperature</label>
        <div class="row">
          <input type="number" id="temp" value="40" step="any">
          <select class="unit" id="tempUnit">
            <option value="C">°C</option>
            <option value="K">K</option>
          </select>
        </div>
      </div>
    </div>
  </div>

  <div class="card">
    <h2>Geometry</h2>
    <div class="grid">
      <div class="field">
        <label>Pipe internal diameter, D</label>
        <div class="row">
          <input type="number" id="D" value="150" step="any">
          <select class="unit" id="Dunit">
            <option value="1">mm</option>
            <option value="25.4">inch</option>
          </select>
        </div>
      </div>
      <div class="field" id="dField">
        <label id="dLabel">Orifice bore diameter, d</label>
        <div class="row">
          <input type="number" id="d" value="75" step="any">
          <select class="unit" id="dunit">
            <option value="1">mm</option>
            <option value="25.4">inch</option>
          </select>
        </div>
      </div>
      <div class="field" id="targetFlowField" style="display:none;">
        <label>Required flow rate</label>
        <div class="row">
          <input type="number" id="targetFlow" value="50000" step="any">
          <select class="unit" id="targetFlowUnit">
            <option value="kgph">kg/hr</option>
            <option value="m3ph">m³/hr (actual)</option>
            <option value="sm3ph">Sm³/hr</option>
          </select>
        </div>
      </div>
      <div class="field" id="constructionField" style="display:none;">
        <label>Convergent section type</label>
        <select id="construction">
          <option value="machined">Machined</option>
          <option value="ascast">As-cast</option>
          <option value="roughwelded">Rough-welded sheet-metal</option>
        </select>
      </div>
      <div class="field" id="nozzleTypeField" style="display:none;">
        <label>Nozzle type</label>
        <select id="nozzleType">
          <option value="isa1932">ISA 1932 nozzle</option>
          <option value="longradius">Long radius nozzle</option>
        </select>
      </div>
      <div class="field">
        <label id="cdLabel">Discharge coefficient, Cd</label>
        <input type="number" id="Cd" value="0.61" step="0.001">
        <div class="note" id="cdNote">Typical square-edge orifice (corner / D-D/2 taps): 0.60–0.62</div>
      </div>
    </div>
  </div>

  <div class="card">
    <h2>Pressure</h2>
    <div class="grid">
      <div class="field">
        <label>Differential pressure, ΔP</label>
        <div class="row">
          <input type="number" id="dp" value="2500" step="any">
          <select class="unit" id="dpUnit">
            <option value="mmWC">mmWC</option>
            <option value="mbar">mbar</option>
            <option value="kPa">kPa</option>
            <option value="kgcm2">kg/cm²</option>
            <option value="bar">bar</option>
          </select>
        </div>
      </div>
      <div class="field" id="p1Field" style="display:none;">
        <label>Upstream line pressure, P1</label>
        <div class="row">
          <input type="number" id="p1" value="4.0" step="any">
          <select class="unit" id="p1Unit">
            <option value="kgcm2g" selected>kg/cm²(g)</option>
            <option value="barg">bar(g)</option>
            <option value="kPag">kPa(g)</option>
            <option value="kgcm2a">kg/cm²(a)</option>
            <option value="bara">bar(a)</option>
            <option value="kPaa">kPa(a)</option>
          </select>
        </div>
      </div>
    </div>
    <div class="note">For gas service, P1 is used only for the expansibility factor (Y) and Sm³/hr normalization. Actual density above should already reflect P1/T1. Gauge values are converted to absolute using 1 atm = 1.01325 bar(a).</div>
  </div>

  <div class="card">
    <h2>Results</h2>
    <div class="results" id="results"></div>
    <div class="diag" id="diag"></div>
    <div id="assessBox"></div>
    <div id="warningBox"></div>
  </div>

  <div class="card">
    <h2>Orifice Selection Guide</h2>
    <table style="width:100%;border-collapse:collapse;font-size:0.82rem;margin-bottom:12px;">
      <thead>
        <tr style="text-align:left;color:var(--muted);border-bottom:1px solid var(--border);">
          <th style="padding:4px 6px;">β = d/D</th>
          <th style="padding:4px 6px;">Guidance</th>
        </tr>
      </thead>
      <tbody>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;">&lt; 0.20</td>
          <td style="padding:4px 6px;">Avoid if possible — high permanent pressure loss and high jet velocity through the bore for the pressure gained; only used when a very high ΔP is genuinely wanted (e.g. deliberate throttling).</td>
        </tr>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;color:var(--good);font-weight:600;">0.20 – 0.65</td>
          <td style="padding:4px 6px;"><b>Preferred working range.</b> Best balance of measurement accuracy, permanent pressure loss, and manufacturing tolerance. Aim for β ≈ 0.4–0.6 at normal flow when there's no other constraint.</td>
        </tr>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;">0.65 – 0.75</td>
          <td style="padding:4px 6px;">Usable, but Cd/expansibility uncertainty increases and bore machining tolerance becomes more critical. Prefer only when ΔP available is limited (e.g. low driving pressure).</td>
        </tr>
        <tr>
          <td style="padding:4px 6px;">&gt; 0.75</td>
          <td style="padding:4px 6px;">Outside ISO 5167 validated range — do not use for fiscal/custody metering; re-check line size or ΔP instead.</td>
        </tr>
      </tbody>
    </table>
    <div class="grid" style="font-size:0.82rem;color:var(--text);">
      <div>
        <b>Differential pressure (ΔP)</b><br>
        <span class="note">Pick the design ΔP to suit the DP transmitter span and desired turndown. Since flow ∝ √ΔP, a 4:1 flow turndown needs a 16:1 span turndown. Typical design ΔP: 2000–2500 mmWC (≈0.2–0.25 kg/cm²) for a 100% flow at ~80–90% of transmitter span, leaving headroom for surges.</span>
      </div>
      <div>
        <b>Permanent pressure loss</b><br>
        <span class="note">Approx. PPL ≈ ΔP × (1 − β²). Lower β wastes more driving pressure — relevant where pumping/compression cost matters.</span>
      </div>
      <div>
        <b>Reynolds number</b><br>
        <span class="note">Keep Re<sub>D</sub> above ~5,000–10,000 at minimum flow so Cd stays stable per ISO 5167; below this the discharge coefficient correlation is no longer valid. Re<sub>D</sub> and Re<sub>d</sub> are now computed live below from your entered viscosity — check them at minimum flow, not just at the design point.</span>
      </div>
      <div>
        <b>Straight-run requirements</b><br>
        <span class="note">Provide adequate straight, unobstructed pipe upstream (typically 10D–44D depending on upstream fitting type and β) and downstream (typically 4D–8D) of the orifice plate per ISO 5167 / plant P&ID standard.</span>
      </div>
      <div>
        <b>Plate & edge</b><br>
        <span class="note">Square-edged plate, edge sharpness radius ≤ 0.0004d, plate thickness ≤ 0.05D (bevel the downstream face at 45° if thicker), bore measured and averaged at 4 diameters minimum.</span>
      </div>
      <div>
        <b>Rangeability check</b><br>
        <span class="note">Verify the selected d also gives an acceptable ΔP (and Re) at minimum expected flow, not just at normal/design flow — a plate sized only for the high case can under-range at turndown.</span>
      </div>
    </div>
  </div>

  <div class="card">
    <h2>Orifice vs Venturi vs Nozzle — Quick Comparison</h2>
    <table style="width:100%;border-collapse:collapse;font-size:0.8rem;">
      <thead>
        <tr style="text-align:left;color:var(--muted);border-bottom:1px solid var(--border);">
          <th style="padding:4px 6px;">Aspect</th>
          <th style="padding:4px 6px;">Orifice Plate</th>
          <th style="padding:4px 6px;">Venturi Tube</th>
          <th style="padding:4px 6px;">Flow Nozzle</th>
        </tr>
      </thead>
      <tbody>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;"><b>Cd (typical)</b></td>
          <td style="padding:4px 6px;">0.60 – 0.62</td>
          <td style="padding:4px 6px;">0.995 machined / 0.984 as-cast / 0.985 rough-welded</td>
          <td style="padding:4px 6px;">0.98 ISA 1932 / 0.99 long radius</td>
        </tr>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;"><b>β range</b></td>
          <td style="padding:4px 6px;">0.20 – 0.75</td>
          <td style="padding:4px 6px;">≈0.30 – 0.75</td>
          <td style="padding:4px 6px;">≈0.20 – 0.80 (usable at higher β/velocity)</td>
        </tr>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;"><b>Permanent pressure loss</b></td>
          <td style="padding:4px 6px;">High: ≈(1−β²)×ΔP, often 40–90%</td>
          <td style="padding:4px 6px;">Low: ≈10–20% — diffuser recovers most</td>
          <td style="padding:4px 6px;">Similar order to orifice — no diffuser</td>
        </tr>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;"><b>Straight run needed</b></td>
          <td style="padding:4px 6px;">Longer: 10D–44D upstream</td>
          <td style="padding:4px 6px;">Shorter: 5D–20D upstream</td>
          <td style="padding:4px 6px;">Similar to orifice, slightly shorter</td>
        </tr>
        <tr style="border-bottom:1px solid var(--border);">
          <td style="padding:4px 6px;"><b>Best suited for</b></td>
          <td style="padding:4px 6px;">General clean-service, cost-sensitive metering</td>
          <td style="padding:4px 6px;">Where low permanent loss or erosive/dirty service matters</td>
          <td style="padding:4px 6px;">High-velocity / high-temperature service (steam), where cavitation risk rules out an orifice</td>
        </tr>
        <tr>
          <td style="padding:4px 6px;"><b>Cost & footprint</b></td>
          <td style="padding:4px 6px;">Low cost, compact</td>
          <td style="padding:4px 6px;">Higher cost, longer laying length</td>
          <td style="padding:4px 6px;">Moderate cost, shorter than Venturi</td>
        </tr>
      </tbody>
    </table>
    <div class="note" style="margin-top:8px;">Nozzle Cd is only weakly Reynolds-dependent at high Re (unlike orifice) — the values above are reasonable defaults for typical process Re, but ISO 5167-3 gives an exact Re-dependent correlation if higher accuracy is needed.</div>
  </div>
  </section>

  <section class="meter-section" id="sec-coriolis">
  <div class="card">
    <h2>Mass Flow Meter (Coriolis / Thermal) — Sizing Check</h2>
    <div class="note" style="margin-bottom:10px;">Coriolis meters derive mass flow from a factory-calibrated flow calibration factor unique to each tube; thermal meters are calibrated to a specific gas's thermal properties. Neither has a generic first-principles equation like ISO 5167 — <b>final model, bore, accuracy, turndown, and pressure drop must come from the vendor's own sizing software</b> using your process data sheet. This section only gives a quick, vendor-independent velocity check to sanity-check a candidate bore before going to the vendor tool.</div>
    <div class="diagram-wrap">
      <svg viewBox="0 0 640 200" xmlns="http://www.w3.org/2000/svg">
        <line x1="20" y1="100" x2="200" y2="100" stroke="#1f2937" stroke-width="4"/>
        <path d="M200,100 C 260,20 380,20 440,100" stroke="#1f2937" stroke-width="4" fill="none"/>
        <path d="M200,100 C 260,180 380,180 440,100" stroke="#1f2937" stroke-width="4" fill="none"/>
        <line x1="440" y1="100" x2="620" y2="100" stroke="#1f2937" stroke-width="4"/>
        <text x="30" y="85" font-size="13" fill="#2563eb">Flow →</text>
        <text x="255" y="15" font-size="11" fill="#d97706">↕ driven vibration</text>
        <text x="255" y="198" font-size="11" fill="#d97706">↕ driven vibration</text>
        <text x="280" y="105" font-size="11" fill="#1f2937">tube twist ∝ ṁ</text>
        <text x="500" y="85" font-size="14" font-weight="700" fill="#16a34a">ṁ →</text>
      </svg>
      <div class="diagram-cap">Tube(s) are vibrated at their natural frequency; mass flow twists/phase-shifts the tube ends. That twist is measured directly as mass flow — no ΔP or density term needed. Input: none beyond flow itself — Output: ṁ directly, from the factory-calibrated tube.</div>
    </div>
    <div class="grid">
      <div class="field">
        <label>Mass flow rate</label>
        <div class="row">
          <input type="number" id="mfmFlow" value="10000" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">kg/hr</span>
        </div>
      </div>
      <div class="field">
        <label>Fluid density</label>
        <div class="row">
          <input type="number" id="mfmRho" value="850" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">kg/m³</span>
        </div>
      </div>
      <div class="field">
        <label>Candidate meter bore, D</label>
        <div class="row">
          <input type="number" id="mfmBore" value="50" step="any" list="mfmBoreList">
          <datalist id="mfmBoreList">
            <option value="15"><option value="25"><option value="40"><option value="50">
            <option value="80"><option value="100"><option value="150"><option value="200">
          </datalist>
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">mm</span>
        </div>
      </div>
    </div>
    <div class="results" id="mfmResults" style="margin-top:12px;"></div>
    <div class="note" style="margin-top:8px;">Rule-of-thumb liquid velocity guideline (varies by vendor/model): &lt;0.3 m/s often too low for stable signal; 1–7 m/s is the typical comfortable range for most Coriolis meters; &gt;7–10 m/s starts to push pressure drop and erosion — confirm against the vendor's actual capacity table for the model in question.</div>
  </div>
  </section>

  <section class="meter-section" id="sec-pitot">
  <div class="card">
    <h2>Pitot Tube / Averaging Pitot — Flow Calculation</h2>
    <div class="note" style="margin-bottom:10px;">Governing physics is standard (v = Cp·√(2ΔP/ρ)), but the flow coefficient Cp is empirically derived for each specific probe geometry and blockage ratio — for custody-grade accuracy, use the vendor's certified Cp and traverse method.</div>
    <div class="toggle-row" id="pitotToggle">
      <button data-pitot="standard" class="active">Standard Pitot (single point)</button>
      <button data-pitot="averaging">Averaging Pitot (Annubar-type)</button>
    </div>

    <div class="diagram-wrap" id="diagram-pitot-standard">
      <svg viewBox="0 0 640 190" xmlns="http://www.w3.org/2000/svg">
        <line x1="20" y1="30" x2="620" y2="30" stroke="#1f2937" stroke-width="2"/>
        <line x1="20" y1="170" x2="620" y2="170" stroke="#1f2937" stroke-width="2"/>
        <text x="30" y="20" font-size="13" fill="#2563eb">Flow →</text>
        <path d="M320,30 L320,90 L360,90" stroke="#1f2937" stroke-width="4" fill="none"/>
        <circle cx="360" cy="90" r="4" fill="#fff" stroke="#1f2937" stroke-width="2"/>
        <text x="368" y="94" font-size="11" fill="#1f2937">impact port (facing upstream)</text>
        <line x1="260" y1="30" x2="260" y2="10" stroke="#d97706" stroke-width="1.5"/>
        <text x="230" y="8" font-size="11" fill="#d97706">static tap (pipe wall)</text>
        <text x="540" y="98" font-size="14" font-weight="700" fill="#16a34a">v → Q</text>
      </svg>
      <div class="diagram-cap">Single point probe reads local velocity at one point (often near centerline) — apply a profile correction, or traverse multiple points, to get the true pipe average. Input: D, ΔP, ρ, Cp, profile factor — Output: v(local), v(avg), Q.</div>
    </div>
    <div class="diagram-wrap diagram-hidden" id="diagram-pitot-averaging">
      <svg viewBox="0 0 640 190" xmlns="http://www.w3.org/2000/svg">
        <line x1="20" y1="30" x2="620" y2="30" stroke="#1f2937" stroke-width="2"/>
        <line x1="20" y1="170" x2="620" y2="170" stroke="#1f2937" stroke-width="2"/>
        <text x="30" y="20" font-size="13" fill="#2563eb">Flow →</text>
        <line x1="320" y1="30" x2="320" y2="170" stroke="#1f2937" stroke-width="6"/>
        <circle cx="320" cy="55" r="3" fill="#fff" stroke="#1f2937"/>
        <circle cx="320" cy="85" r="3" fill="#fff" stroke="#1f2937"/>
        <circle cx="320" cy="115" r="3" fill="#fff" stroke="#1f2937"/>
        <circle cx="320" cy="145" r="3" fill="#fff" stroke="#1f2937"/>
        <text x="335" y="90" font-size="11" fill="#1f2937">multiple ports (facing upstream),</text>
        <text x="335" y="104" font-size="11" fill="#1f2937">internally averaged</text>
        <line x1="320" y1="10" x2="480" y2="10" stroke="#d97706" stroke-width="1.5"/>
        <line x1="320" y1="10" x2="320" y2="30" stroke="#d97706" stroke-width="1.5" stroke-dasharray="3,2"/>
        <line x1="480" y1="10" x2="480" y2="30" stroke="#d97706" stroke-width="1.5" stroke-dasharray="3,2"/>
        <text x="345" y="6" font-size="12" fill="#d97706">ΔP (impact − static)</text>
        <text x="540" y="98" font-size="14" font-weight="700" fill="#16a34a">v → Q</text>
      </svg>
      <div class="diagram-cap">A rod spans the pipe with multiple sensing ports, giving a flow-representative average velocity directly from one ΔP reading — no separate traverse needed. Input: D, ΔP, ρ, Cp — Output: v(avg), Q.</div>
    </div>
    <div class="grid">
      <div class="field">
        <label>Pipe internal diameter, D</label>
        <div class="row">
          <input type="number" id="pitotD" value="150" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">mm</span>
        </div>
      </div>
      <div class="field">
        <label>Differential / impact pressure, ΔP</label>
        <div class="row">
          <input type="number" id="pitotDp" value="500" step="any">
          <select class="unit" id="pitotDpUnit">
            <option value="mmWC">mmWC</option>
            <option value="mbar">mbar</option>
            <option value="kPa">kPa</option>
          </select>
        </div>
      </div>
      <div class="field">
        <label>Fluid density</label>
        <div class="row">
          <input type="number" id="pitotRho" value="1.2" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">kg/m³</span>
        </div>
      </div>
      <div class="field">
        <label>Flow coefficient, Cp</label>
        <input type="number" id="pitotCp" value="1.0" step="0.01">
        <div class="note" id="pitotCpNote">Standard pitot (theoretical/calibrated): ≈0.98–1.0</div>
      </div>
      <div class="field" id="pitotAvgField">
        <label>Point-to-average velocity factor</label>
        <input type="number" id="pitotAvgFactor" value="0.85" step="0.01">
        <div class="note">Single-point pitot reads local (often near-centerline) velocity — apply a profile correction to get the pipe average (≈0.82–0.87 typical for fully-developed turbulent flow; use a full traverse for accuracy).</div>
      </div>
    </div>
    <div class="results" id="pitotResults" style="margin-top:12px;"></div>
  </div>
  </section>

  <section class="meter-section" id="sec-vmt">
  <div class="card">
    <h2>Vortex / Magnetic / Turbine — Sizing &amp; Feasibility Check</h2>
    <div class="note" style="margin-bottom:10px;">These meters need a vendor K-factor or calibration constant for exact accuracy — this section checks velocity, Reynolds number, and basic feasibility against generic published guidelines only.</div>
    <div class="toggle-row" id="vmtToggle">
      <button data-vmt="vortex" class="active">Vortex Shedding</button>
      <button data-vmt="magnetic">Magnetic (Mag) Meter</button>
      <button data-vmt="turbine">Turbine Meter</button>
    </div>

    <div class="diagram-wrap" id="diagram-vortex">
      <svg viewBox="0 0 640 190" xmlns="http://www.w3.org/2000/svg">
        <line x1="20" y1="40" x2="620" y2="40" stroke="#1f2937" stroke-width="2"/>
        <line x1="20" y1="150" x2="620" y2="150" stroke="#1f2937" stroke-width="2"/>
        <text x="30" y="30" font-size="13" fill="#2563eb">Flow →</text>
        <rect x="300" y="70" width="16" height="50" fill="#1f2937"/>
        <text x="270" y="65" font-size="11" fill="#1f2937">bluff body</text>
        <circle cx="360" cy="75" r="8" fill="none" stroke="#2563eb" stroke-width="1.5"/>
        <circle cx="400" cy="115" r="10" fill="none" stroke="#2563eb" stroke-width="1.5"/>
        <circle cx="440" cy="75" r="12" fill="none" stroke="#2563eb" stroke-width="1.5"/>
        <circle cx="480" cy="115" r="13" fill="none" stroke="#2563eb" stroke-width="1.5"/>
        <text x="345" y="145" font-size="11" fill="#2563eb">alternating vortices, frequency f</text>
        <text x="325" y="60" font-size="10" fill="#6b7280">sensor</text>
        <text x="540" y="98" font-size="14" font-weight="700" fill="#16a34a">f → v → Q</text>
      </svg>
      <div class="diagram-cap">A bluff body sheds alternating vortices; shedding frequency f is proportional to velocity (f = St·v/w). Input: D, Q (or v), viscosity, bluff-body width, St — Output: est. frequency, Re, feasibility check.</div>
    </div>
    <div class="diagram-wrap diagram-hidden" id="diagram-magnetic">
      <svg viewBox="0 0 300 300" xmlns="http://www.w3.org/2000/svg">
        <circle cx="150" cy="150" r="110" fill="none" stroke="#1f2937" stroke-width="3"/>
        <line x1="150" y1="20" x2="150" y2="70" stroke="#2563eb" stroke-width="3"/>
        <text x="118" y="15" font-size="12" fill="#2563eb">B (coil)</text>
        <line x1="150" y1="230" x2="150" y2="280" stroke="#2563eb" stroke-width="3"/>
        <text x="118" y="298" font-size="12" fill="#2563eb">B (coil)</text>
        <circle cx="42" cy="150" r="6" fill="#d97706"/>
        <text x="0" y="140" font-size="11" fill="#d97706">electrode</text>
        <circle cx="258" cy="150" r="6" fill="#d97706"/>
        <text x="195" y="140" font-size="11" fill="#d97706">electrode</text>
        <text x="95" y="158" font-size="13" fill="#1f2937">v (flow, out of page)</text>
        <text x="80" y="180" font-size="12" font-weight="700" fill="#16a34a">E ∝ B·D·v → Q</text>
      </svg>
      <div class="diagram-cap">End-on view: coils create a magnetic field B across the pipe; the moving conductive fluid induces a voltage E at the electrodes, proportional to velocity (Faraday's law). Input: D, Q (or v), fluid conductivity — Output: velocity check, conductivity feasibility.</div>
    </div>
    <div class="diagram-wrap diagram-hidden" id="diagram-turbine">
      <svg viewBox="0 0 640 190" xmlns="http://www.w3.org/2000/svg">
        <line x1="20" y1="40" x2="620" y2="40" stroke="#1f2937" stroke-width="2"/>
        <line x1="20" y1="150" x2="620" y2="150" stroke="#1f2937" stroke-width="2"/>
        <text x="30" y="30" font-size="13" fill="#2563eb">Flow →</text>
        <circle cx="340" cy="95" r="45" fill="none" stroke="#1f2937" stroke-width="2"/>
        <line x1="340" y1="95" x2="340" y2="50" stroke="#1f2937" stroke-width="2"/>
        <line x1="340" y1="95" x2="378" y2="120" stroke="#1f2937" stroke-width="2"/>
        <line x1="340" y1="95" x2="302" y2="120" stroke="#1f2937" stroke-width="2"/>
        <line x1="340" y1="95" x2="340" y2="140" stroke="#1f2937" stroke-width="2"/>
        <path d="M300,50 A50,50 0 0,1 380,50" stroke="#2563eb" stroke-width="1.5" fill="none"/>
        <text x="280" y="35" font-size="11" fill="#2563eb">rotor spins ∝ v</text>
        <text x="430" y="70" font-size="11" fill="#6b7280">pickup coil</text>
        <text x="540" y="98" font-size="14" font-weight="700" fill="#16a34a">pulses → Q</text>
      </svg>
      <div class="diagram-cap">An in-line rotor spins at a rate proportional to velocity; a pickup coil counts pulses/revolution against a calibrated K-factor. Input: D, Q (or v), viscosity — Output: velocity, Reynolds number, feasibility check.</div>
    </div>
    <div class="grid">
      <div class="field">
        <label>Volumetric flow rate</label>
        <div class="row">
          <input type="number" id="vmtFlow" value="100" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">m³/hr</span>
        </div>
      </div>
      <div class="field">
        <label>Meter bore, D</label>
        <div class="row">
          <input type="number" id="vmtD" value="100" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">mm</span>
        </div>
      </div>
      <div class="field">
        <label>Fluid density</label>
        <div class="row">
          <input type="number" id="vmtRho" value="850" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">kg/m³</span>
        </div>
      </div>
      <div class="field" id="vmtMuField">
        <label>Dynamic viscosity</label>
        <div class="row">
          <input type="number" id="vmtMu" value="1.0" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">cP</span>
        </div>
      </div>
      <div class="field" id="vmtStField" style="display:none;">
        <label>Strouhal number, St</label>
        <input type="number" id="vmtSt" value="0.22" step="0.01">
      </div>
      <div class="field" id="vmtCondField" style="display:none;">
        <label>Fluid conductivity</label>
        <div class="row">
          <input type="number" id="vmtCond" value="50" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">µS/cm</span>
        </div>
      </div>
    </div>
    <div class="results" id="vmtResults" style="margin-top:12px;"></div>
    <div id="vmtWarn"></div>
  </div>
  </section>

  <section class="meter-section" id="sec-oc">
  <div class="card">
    <h2>Open Channel Flow — Weir / Flume</h2>
    <div class="note" style="margin-bottom:10px;">Standard hydraulic formulas — fully generic, no vendor calibration involved. Used for gravity/open drains, effluent channels, cooling tower basins, etc., not for closed-pipe process service.</div>
    <div class="toggle-row" id="ocToggle">
      <button data-oc="vnotch" class="active">V-notch Weir</button>
      <button data-oc="rect_supp">Rectangular Weir (Suppressed)</button>
      <button data-oc="rect_cont">Rectangular Weir (Contracted)</button>
      <button data-oc="parshall">Parshall Flume</button>
    </div>

    <div class="diagram-wrap" id="diagram-weir">
      <svg viewBox="0 0 640 220" xmlns="http://www.w3.org/2000/svg">
        <rect x="300" y="20" width="20" height="180" fill="#1f2937"/>
        <path d="M300,60 L310,90 L320,60" fill="#fff" stroke="#1f2937" stroke-width="2"/>
        <line x1="20" y1="60" x2="300" y2="60" stroke="#2563eb" stroke-width="2"/>
        <text x="30" y="50" font-size="12" fill="#2563eb">upstream water surface</text>
        <line x1="260" y1="60" x2="260" y2="90" stroke="#d97706" stroke-width="1.5" stroke-dasharray="3,2"/>
        <text x="266" y="78" font-size="13" fill="#d97706">H</text>
        <path d="M320,90 Q 380,110 440,170" stroke="#2563eb" stroke-width="2" fill="none"/>
        <text x="440" y="190" font-size="14" font-weight="700" fill="#16a34a">Q →</text>
        <line x1="20" y1="200" x2="620" y2="200" stroke="#1f2937" stroke-width="2"/>
      </svg>
      <div class="diagram-cap">Head H is measured upstream of the drawdown curve, from the notch vertex (V-notch) or crest (rectangular) to the water surface. Input: H, notch angle or crest length L, Cd — Output: Q, from standard weir formula.</div>
    </div>
    <div class="diagram-wrap diagram-hidden" id="diagram-flume">
      <svg viewBox="0 0 640 220" xmlns="http://www.w3.org/2000/svg">
        <path d="M20,60 L220,60 L280,90 L360,90 L420,60 L620,60" stroke="#1f2937" stroke-width="2" fill="none"/>
        <path d="M20,160 L220,160 L280,130 L360,130 L420,160 L620,160" stroke="#1f2937" stroke-width="2" fill="none"/>
        <text x="30" y="50" font-size="13" fill="#2563eb">Flow →</text>
        <line x1="300" y1="90" x2="300" y2="130" stroke="#6b7280" stroke-width="1" stroke-dasharray="4,3"/>
        <text x="306" y="113" font-size="13" fill="#1f2937">W (throat)</text>
        <line x1="250" y1="60" x2="250" y2="35" stroke="#d97706" stroke-width="1.5"/>
        <text x="220" y="28" font-size="12" fill="#d97706">Ha (upstream head gauge)</text>
        <text x="540" y="115" font-size="14" font-weight="700" fill="#16a34a">Q →</text>
      </svg>
      <div class="diagram-cap">Plan view: converging inlet, fixed throat width W, diverging outlet. Head Ha is read at a fixed gauge point in the converging section under free-flow conditions. Input: W, Ha — Output: Q, from the standard Parshall equation for that throat size.</div>
    </div>
    <div class="grid">
      <div class="field">
        <label>Head over crest / at gauge point, H</label>
        <div class="row">
          <input type="number" id="ocH" value="150" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">mm</span>
        </div>
      </div>
      <div class="field" id="ocAngleField">
        <label>Notch angle, θ</label>
        <select id="ocAngle">
          <option value="90">90°</option>
          <option value="60">60°</option>
          <option value="45">45°</option>
          <option value="22.5">22.5°</option>
        </select>
      </div>
      <div class="field" id="ocLField" style="display:none;">
        <label>Crest length, L</label>
        <div class="row">
          <input type="number" id="ocL" value="0.5" step="any">
          <span style="align-self:center;font-size:0.8rem;color:var(--muted);">m</span>
        </div>
      </div>
      <div class="field" id="ocCdField" style="display:none;">
        <label>Discharge coefficient, Cd</label>
        <input type="number" id="ocCd" value="0.62" step="0.01">
      </div>
      <div class="field" id="ocWField" style="display:none;">
        <label>Parshall throat width, W</label>
        <select id="ocW">
          <option value="0.25">3 in</option>
          <option value="0.5">6 in</option>
          <option value="0.75">9 in</option>
          <option value="1">1 ft</option>
          <option value="1.5">1.5 ft</option>
          <option value="2">2 ft</option>
          <option value="3">3 ft</option>
          <option value="4">4 ft</option>
          <option value="6">6 ft</option>
          <option value="8">8 ft</option>
        </select>
      </div>
    </div>
    <div class="results" id="ocResults" style="margin-top:12px;"></div>
    <div class="note" style="margin-top:8px;" id="ocFormulaNote"></div>
  </div>
  </section>

  <footer class="no-print">
   <h3>Developer Information</h3>
   <p><strong>Gajanand Yadav</strong></p>
   <p>Chemical Engineer </p>
   <p>Email: <a href="mailto:gajanandiitg@gmail.com">gajanandiitg@gmail.com</a></p>
   <p><a href="https://www.linkedin.com/in/gajanand-yadav-512624a5/" target="_blank">LinkedIn</a></p>
   <p>Mobile: <a href="tel:+918369354472">+91-8369354472</a></p>
   <p>For property calculation, Density, Cp, saturation condition visit below link</p>
   <p><a href="https://gajuiitg.github.io/Thermocal/">Clickable Here</a></p>
  </footer>

  <footer>Simplified ISO 5167 differential-pressure flowmeter equations — for preliminary sizing / field checks. Orifice expansibility uses the ISO 5167-2 empirical approximation; Venturi and Nozzle expansibility use the exact isentropic relation (ISO 5167-1/-3/-4). Verify against vendor meter run data sheet before use on custody or safety-critical service.</footer>
</div>

<script>
const $=id=>document.getElementById(id);
let fluid='liquid';
let mode='flow';
let meterType='orifice';
let category='dp';
let pitotType='standard';
let vmtType='vortex';
let ocType='vnotch';

const CAT_LABELS = {dp:'Differential Pressure Flow Meter', coriolis:'Mass Flow Meter (Coriolis / Thermal)', pitot:'Pitot Tube', vmt:'Vortex / Magnetic / Turbine Meter', oc:'Open Channel Flow (Weir / Flume)'};
const DP_LABELS = {orifice:'Orifice Plate', venturi:'Classical Venturi Tube', nozzle:'Flow Nozzle'};
const PITOT_LABELS = {standard:'Standard Pitot', averaging:'Averaging Pitot (Annubar-type)'};
const VMT_LABELS = {vortex:'Vortex Shedding', magnetic:'Magnetic (Mag) Meter', turbine:'Turbine Meter'};
const OC_LABELS = {vnotch:'V-notch Weir', rect_supp:'Rectangular Weir (Suppressed)', rect_cont:'Rectangular Weir (Contracted)', parshall:'Parshall Flume'};

function updatePrintHeader(){
  let sub='';
  if(category==='dp') sub=DP_LABELS[meterType];
  else if(category==='pitot') sub=PITOT_LABELS[pitotType];
  else if(category==='vmt') sub=VMT_LABELS[vmtType];
  else if(category==='oc') sub=OC_LABELS[ocType];
  $('printHeader').innerHTML = `<div class="cat">${CAT_LABELS[category]}${sub?' — '+sub:''}</div><div class="ts">Printed: ${new Date().toLocaleString()}</div>`;
}

document.querySelectorAll('#categoryToggle button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('#categoryToggle button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    category=btn.dataset.cat;
    document.querySelectorAll('.meter-section').forEach(s=>s.classList.remove('active'));
    $('sec-'+category).classList.add('active');
    updatePrintHeader();
  });
});

const VENTURI_CD = {machined:0.995, ascast:0.984, roughwelded:0.985};
const VENTURI_LOSS = {machined:0.10, ascast:0.12, roughwelded:0.20};
const NOZZLE_CD = {isa1932:0.98, longradius:0.99};

document.querySelectorAll('#meterToggle button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('#meterToggle button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    meterType=btn.dataset.meter;
    updateMeterTypeUI();
    calc();
    updatePrintHeader();
  });
});
$('construction').addEventListener('change', ()=>{
  $('Cd').value = VENTURI_CD[$('construction').value];
  updateCdNote();
  calc();
});
$('nozzleType').addEventListener('change', ()=>{
  $('Cd').value = NOZZLE_CD[$('nozzleType').value];
  updateCdNote();
  calc();
});

function updateMeterTypeUI(){
  const isVenturi = meterType==='venturi';
  const isNozzle = meterType==='nozzle';
  $('constructionField').style.display = isVenturi ? 'block':'none';
  $('nozzleTypeField').style.display = isNozzle ? 'block':'none';
  $('dLabel').textContent = (isVenturi||isNozzle) ? (isNozzle?'Nozzle throat diameter, d':'Venturi throat diameter, d') : 'Orifice bore diameter, d';
  $('pageSub').textContent = isVenturi
    ? 'Classical Venturi tube — ISO 5167-4, exact isentropic expansibility'
    : isNozzle
      ? 'ISA 1932 / long radius flow nozzle — ISO 5167-3, exact isentropic expansibility'
      : 'Square-edge concentric orifice — ISO 5167-2 simplified form';
  if(isVenturi) $('Cd').value = VENTURI_CD[$('construction').value];
  else if(isNozzle) $('Cd').value = NOZZLE_CD[$('nozzleType').value];
  else $('Cd').value = 0.61;
  updateCdNote();
  ['orifice','venturi','nozzle'].forEach(t=>{
    $('diagram-'+t).classList.toggle('diagram-hidden', t!==meterType);
  });
}

function updateCdNote(){
  if(meterType==='venturi'){
    $('cdNote').textContent = 'Typical: 0.995 machined / 0.984 as-cast / 0.985 rough-welded (auto-filled — override if vendor-certified)';
  } else if(meterType==='nozzle'){
    $('cdNote').textContent = 'Typical: 0.98 ISA 1932 / 0.99 long radius (auto-filled, roughly constant at high Re — check ISO 5167-3 for exact Re-dependent value)';
  } else {
    $('cdNote').textContent = 'Typical square-edge orifice (corner / D-D/2 taps): 0.60–0.62';
  }
}

document.querySelectorAll('#fluidToggle button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('#fluidToggle button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    fluid=btn.dataset.fluid;
    $('kappaField').style.display = fluid==='gas' ? 'block':'none';
    $('tempField').style.display = fluid==='gas' ? 'block':'none';
    $('p1Field').style.display = fluid==='gas' ? 'block':'none';
    if(fluid==='gas'){ $('mu').value='0.012'; } else { $('mu').value='1.0'; }
    updateFlowUnitOptions();
    calc();
  });
});

document.querySelectorAll('#modeToggle button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('#modeToggle button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    mode=btn.dataset.mode;
    $('dField').style.display = mode==='flow' ? 'block':'none';
    $('targetFlowField').style.display = mode==='size' ? 'block':'none';
    calc();
  });
});

function updateFlowUnitOptions(){
  const sel=$('targetFlowUnit');
  const sm3Opt = sel.querySelector('option[value="sm3ph"]');
  sm3Opt.disabled = (fluid!=='gas');
  if(fluid!=='gas' && sel.value==='sm3ph') sel.value='kgph';
}

function toPa(val, unit){
  switch(unit){
    case 'mmWC': return val*9.80665;
    case 'mbar': return val*100;
    case 'kPa':  return val*1000;
    case 'kgcm2':return val*98066.5;
    case 'bar':  return val*100000;
  }
}
// gauge -> absolute uses 1 atm = 1.01325 bar(a) = 1.033227 kgf/cm2(a) = 101.325 kPa(a)
function absPaToPa(val, unit){
  const ATM_KGCM2=1.033227, ATM_BAR=1.01325, ATM_KPA=101.325;
  switch(unit){
    case 'kgcm2a': return val*98066.5;
    case 'bara':   return val*100000;
    case 'kPaa':   return val*1000;
    case 'kgcm2g': return (val+ATM_KGCM2)*98066.5;
    case 'barg':   return (val+ATM_BAR)*100000;
    case 'kPag':   return (val+ATM_KPA)*1000;
  }
}

function expansibilityVenturi(beta, ratio, kappa){
  // exact isentropic expansibility (ISO 5167-1 / 5167-4), tau = P2/P1 = 1 - ΔP/P1
  if(!(kappa>0) || !(ratio>0)) return 1;
  const tau = 1-ratio;
  if(!(tau>0)) return 1;
  const beta4 = Math.pow(beta,4);
  const p = 2/kappa;
  const tauP = Math.pow(tau,p);
  const term1 = (kappa/(kappa-1)) * tauP * (1-beta4)/(1-beta4*tauP);
  let term2;
  if(Math.abs(1-tau) < 1e-9){ term2 = 1; }
  else { term2 = (1-Math.pow(tau,(kappa-1)/kappa))/(1-tau); }
  const Y2 = term1*term2;
  return Math.sqrt(Math.max(Y2,0));
}

function massFlowForD(d, D, rho, dp_Pa, Cd, fluid, P1_Pa, kappa, meterType){
  const beta=d/D;
  const E=1/Math.sqrt(1-Math.pow(beta,4));
  let Y=1;
  if(fluid==='gas' && P1_Pa){
    const ratio=dp_Pa/P1_Pa;
    if(meterType==='venturi' || meterType==='nozzle'){
      Y=expansibilityVenturi(beta,ratio,kappa);
    } else {
      Y=1-(0.41+0.35*Math.pow(beta,4))*(ratio/kappa);
    }
  }
  const area=Math.PI/4*d*d;
  const mdot=Cd*E*Y*area*Math.sqrt(2*dp_Pa*rho);
  return {mdot,beta,E,Y,area};
}

function solveForD(targetMdot, D, rho, dp_Pa, Cd, fluid, P1_Pa, kappa, meterType){
  let lo=D*0.001, hi=D*0.999;
  const fHi=massFlowForD(hi,D,rho,dp_Pa,Cd,fluid,P1_Pa,kappa,meterType).mdot;
  if(fHi < targetMdot) return null; // not achievable within pipe bore
  for(let i=0;i<80;i++){
    const mid=(lo+hi)/2;
    const fm=massFlowForD(mid,D,rho,dp_Pa,Cd,fluid,P1_Pa,kappa,meterType).mdot;
    if(fm<targetMdot) lo=mid; else hi=mid;
  }
  return (lo+hi)/2;
}

function stdDensity(rho, P1_Pa, T1_K){
  const Pstd=101325, Tstd=288.15; // 1.01325 bar(a), 15 degC
  return rho*(Pstd/P1_Pa)*(T1_K/Tstd);
}

function calc(){
  const D = parseFloat($('D').value)*parseFloat($('Dunit').value)/1000; // m
  const Cd = parseFloat($('Cd').value);
  const rho = parseFloat($('rho').value)*parseFloat($('rhoUnit').value); // kg/m3
  const dp_Pa = toPa(parseFloat($('dp').value), $('dpUnit').value);

  const warn=[];
  let P1_Pa=null, kappa=null, T1_K=null;
  if(fluid==='gas'){
    P1_Pa = absPaToPa(parseFloat($('p1').value), $('p1Unit').value);
    kappa = parseFloat($('kappa').value);
    const T1_C = $('tempUnit').value==='C'?parseFloat($('temp').value):parseFloat($('temp').value)-273.15;
    T1_K = T1_C+273.15;
    if(P1_Pa>0){
      const ratio=dp_Pa/P1_Pa;
      if(ratio>0.25) warn.push('ΔP/P1 exceeds ~0.25 — expansibility approximation less accurate');
    }
  }

  if(!(D>0)||!(rho>0)||!(dp_Pa>0)||!(Cd>0)){
    $('results').innerHTML='<div class="result-box"><div class="label">Status</div><div class="value">Enter valid inputs</div></div>';
    $('diag').innerHTML=''; $('warningBox').innerHTML=''; $('betaTag').textContent='β = —';
    return;
  }

  let d, mdot, beta, E, Y, area;

  if(mode==='flow'){
    d = parseFloat($('d').value)*parseFloat($('dunit').value)/1000; // m
    if(!(d>0)){ return; }
    ({mdot,beta,E,Y,area}=massFlowForD(d,D,rho,dp_Pa,Cd,fluid,P1_Pa,kappa,meterType));
  } else {
    const tfVal = parseFloat($('targetFlow').value);
    const tfUnit = $('targetFlowUnit').value;
    let targetMdot;
    if(tfUnit==='kgph') targetMdot = tfVal/3600;
    else if(tfUnit==='m3ph') targetMdot = tfVal/3600*rho;
    else { // sm3ph, gas only
      const rhoStd = stdDensity(rho, P1_Pa, T1_K);
      targetMdot = tfVal/3600*rhoStd;
    }
    if(!(targetMdot>0)){ return; }
    d = solveForD(targetMdot, D, rho, dp_Pa, Cd, fluid, P1_Pa, kappa, meterType);
    if(d===null){
      $('results').innerHTML='<div class="result-box"><div class="label">Status</div><div class="value" style="color:var(--warn);">Not achievable</div></div>';
      $('diag').innerHTML='';
      $('warningBox').innerHTML='<div class="warning">Required flow exceeds what this pipe bore can pass at the given ΔP, even with d = D. Increase ΔP, increase pipe size, or reduce target flow.</div>';
      $('betaTag').textContent='β = —';
      return;
    }
    ({mdot,beta,E,Y,area}=massFlowForD(d,D,rho,dp_Pa,Cd,fluid,P1_Pa,kappa,meterType));
  }

  $('betaTag').textContent = 'β = '+beta.toFixed(4);
  if(beta<0.1||beta>0.75) warn.push('β outside typical ISO 5167 valid range (0.10–0.75)');

  const mu = parseFloat($('mu').value)*parseFloat($('muUnit').value); // Pa.s
  let reD=null, red=null;
  if(mu>0){
    reD = (4*mdot)/(Math.PI*D*mu);
    red = (4*mdot)/(Math.PI*d*mu);
    if(reD < 5000) warn.push(`Re_D = ${reD.toFixed(0)} is below ~5,000 — Cd correlation may not hold; verify at this flow`);
  }

  const mf_kgph = mdot*3600;
  const vf_m3ph = (mdot/rho)*3600;
  const d_mm = d*1000;
  const bore_word = (meterType==='venturi'||meterType==='nozzle') ? 'throat' : 'orifice';

  let ppl_Pa;
  if(meterType==='venturi'){
    const lossFactor = VENTURI_LOSS[$('construction').value];
    ppl_Pa = dp_Pa*lossFactor;
  } else {
    ppl_Pa = dp_Pa*(1-beta*beta); // orifice & nozzle: no diffuser, similar order of loss
  }
  const ppl_mmWC = ppl_Pa/9.80665;
  const ppl_kgcm2 = ppl_Pa/98066.5;
  const ppl_pct = (ppl_Pa/dp_Pa)*100;

  let stdLine='';
  if(fluid==='gas' && P1_Pa && T1_K>0){
    const rho_std = stdDensity(rho, P1_Pa, T1_K);
    const vf_std_m3ph = mf_kgph/rho_std;
    stdLine = `<div class="result-box"><div class="label">Standard volumetric flow</div><div class="value">${vf_std_m3ph.toFixed(1)} <span>Sm³/hr @ 1.01325 bar(a), 15°C</span></div></div>`;
  }

  let boxes='';
  if(mode==='size'){
    boxes += `<div class="result-box primary"><div class="label">Calculated ${bore_word} diameter</div><div class="value">${d_mm.toFixed(2)} <span>mm</span></div></div>`;
  }
  boxes += `
    <div class="result-box"><div class="label">Mass flow</div><div class="value">${mf_kgph.toFixed(1)} <span>kg/hr</span></div></div>
    <div class="result-box"><div class="label">Volumetric flow (actual)</div><div class="value">${vf_m3ph.toFixed(2)} <span>m³/hr</span></div></div>
    ${stdLine}
    <div class="result-box"><div class="label">Permanent pressure loss (approx.)</div><div class="value">${ppl_mmWC.toFixed(0)} <span>mmWC (${ppl_kgcm2.toFixed(3)} kg/cm², ${ppl_pct.toFixed(0)}% of ΔP)</span></div></div>
  `;
  $('results').innerHTML = boxes;

  $('diag').innerHTML = `
    <span><b>Velocity of approach, E:</b> ${E.toFixed(4)}</span>
    <span><b>Expansibility, Y:</b> ${Y.toFixed(4)}</span>
    <span><b>${bore_word==='throat'?'Throat':'Orifice'} area:</b> ${(area*1e6).toFixed(2)} mm²</span>
    ${reD!==null ? `<span><b>Re_D (pipe):</b> ${reD.toFixed(0)}</span>` : ''}
    ${red!==null ? `<span><b>Re_d (${bore_word}):</b> ${red.toFixed(0)}</span>` : ''}
    ${mode==='size' ? `<span><b>${bore_word==='throat'?'Throat':'Orifice'} diameter:</b> ${d_mm.toFixed(2)} mm</span>` : ''}
  `;

  $('warningBox').innerHTML = warn.length ? `<div class="warning">${warn.join(' • ')}</div>` : '';
  $('assessBox').innerHTML = betaAssessmentHtml(beta);
}

function betaAssessmentHtml(beta){
  let text, color, bg;
  let lo=0.20, midHigh=0.65, hi=0.75;
  if(meterType==='venturi'){ lo=0.30; midHigh=0.65; hi=0.75; }
  else if(meterType==='nozzle'){ lo=0.20; midHigh=0.70; hi=0.80; }
  if(beta<lo){ text=`β < ${lo.toFixed(2)} — small ${meterType==='orifice'?'bore':'throat'} relative to pipe; high permanent pressure loss / below typical range. Consider a larger bore or lower ΔP.`; color='#d97706'; bg='#fff7ed'; }
  else if(beta<=midHigh){ text=`β in preferred range (${lo.toFixed(2)}–${midHigh.toFixed(2)}) — good balance of accuracy and pressure loss.`; color='#16a34a'; bg='#e7f7ee'; }
  else if(beta<=hi){ text=`β in usable but marginal range (${midHigh.toFixed(2)}–${hi.toFixed(2)}) — check Cd/expansibility sensitivity and bore tolerance.`; color='#d97706'; bg='#fff7ed'; }
  else { text=`β > ${hi.toFixed(2)} — outside validated range for this meter type. Re-check line size, ΔP, or flow basis.`; color='#dc2626'; bg='#fef2f2'; }
  return `<div class="warning" style="color:${color};background:${bg};border-color:${color}33;"><b>Selection check:</b> ${text}</div>`;
}

document.querySelectorAll('input,select').forEach(el=>el.addEventListener('input',calc));
updateFlowUnitOptions();
updateMeterTypeUI();
calc();

function calcMFM(){
  const mfR = parseFloat($('mfmFlow').value); // kg/hr
  const rho = parseFloat($('mfmRho').value);   // kg/m3
  const bore = parseFloat($('mfmBore').value); // mm
  if(!(mfR>0)||!(rho>0)||!(bore>0)){
    $('mfmResults').innerHTML='<div class="result-box"><div class="label">Status</div><div class="value">Enter valid inputs</div></div>';
    return;
  }
  const D = bore/1000;
  const area = Math.PI/4*D*D;
  const volFlow = (mfR/3600)/rho; // m3/s
  const vel = volFlow/area; // m/s

  let text, color, bg;
  if(vel<0.3){ text='Low — signal/turndown may be marginal at this flow'; color='#d97706'; bg='#fff7ed'; }
  else if(vel<=7){ text='Within the typical comfortable range for most Coriolis meters'; color='#16a34a'; bg='#e7f7ee'; }
  else if(vel<=10){ text='High — check pressure drop and erosion against vendor curve'; color='#d97706'; bg='#fff7ed'; }
  else { text='Very high — likely undersized bore; check next larger size'; color='#dc2626'; bg='#fef2f2'; }

  $('mfmResults').innerHTML = `
    <div class="result-box"><div class="label">Velocity in bore</div><div class="value">${vel.toFixed(2)} <span>m/s</span></div></div>
    <div class="result-box" style="background:${bg};"><div class="label">Guideline check</div><div class="value" style="color:${color};font-size:0.95rem;">${text}</div></div>
  `;
}
['mfmFlow','mfmRho','mfmBore'].forEach(id=>$(id).addEventListener('input',calcMFM));
calcMFM();

// ---------- Pitot / Averaging Pitot ----------
document.querySelectorAll('#pitotToggle button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('#pitotToggle button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    pitotType=btn.dataset.pitot;
    $('pitotAvgField').style.display = pitotType==='standard' ? 'block':'none';
    $('pitotCp').value = pitotType==='standard' ? 1.0 : 0.6;
    $('pitotCpNote').textContent = pitotType==='standard'
      ? 'Standard pitot (theoretical/calibrated): ≈0.98–1.0'
      : 'Averaging pitot (Annubar-type): typically ≈0.5–0.7 — use the vendor-published Cp for the specific probe model';
    $('diagram-pitot-standard').classList.toggle('diagram-hidden', pitotType!=='standard');
    $('diagram-pitot-averaging').classList.toggle('diagram-hidden', pitotType!=='averaging');
    calcPitot();
    updatePrintHeader();
  });
});
function pitotToPa(val,unit){
  switch(unit){ case 'mmWC': return val*9.80665; case 'mbar': return val*100; case 'kPa': return val*1000; }
}
function calcPitot(){
  const D = parseFloat($('pitotD').value)/1000; // m
  const dp_Pa = pitotToPa(parseFloat($('pitotDp').value), $('pitotDpUnit').value);
  const rho = parseFloat($('pitotRho').value);
  const Cp = parseFloat($('pitotCp').value);
  if(!(D>0)||!(dp_Pa>0)||!(rho>0)||!(Cp>0)){
    $('pitotResults').innerHTML='<div class="result-box"><div class="label">Status</div><div class="value">Enter valid inputs</div></div>';
    return;
  }
  const v_point = Cp*Math.sqrt(2*dp_Pa/rho);
  let v_avg, note;
  if(pitotType==='standard'){
    const f = parseFloat($('pitotAvgFactor').value);
    v_avg = v_point*f;
    note = 'Point velocity × profile factor';
  } else {
    v_avg = v_point;
    note = 'Averaging probe reads representative mean velocity directly';
  }
  const area = Math.PI/4*D*D;
  const Q_m3ph = v_avg*area*3600;
  const mf_kgph = Q_m3ph*rho;
  $('pitotResults').innerHTML = `
    <div class="result-box"><div class="label">Local/measured velocity</div><div class="value">${v_point.toFixed(2)} <span>m/s</span></div></div>
    <div class="result-box primary"><div class="label">Pipe average velocity</div><div class="value">${v_avg.toFixed(2)} <span>m/s — ${note}</span></div></div>
    <div class="result-box"><div class="label">Volumetric flow</div><div class="value">${Q_m3ph.toFixed(2)} <span>m³/hr</span></div></div>
    <div class="result-box"><div class="label">Mass flow</div><div class="value">${mf_kgph.toFixed(1)} <span>kg/hr</span></div></div>
  `;
}
['pitotD','pitotDp','pitotDpUnit','pitotRho','pitotCp','pitotAvgFactor'].forEach(id=>$(id).addEventListener('input',calcPitot));
calcPitot();

// ---------- Vortex / Magnetic / Turbine ----------
document.querySelectorAll('#vmtToggle button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('#vmtToggle button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    vmtType=btn.dataset.vmt;
    $('vmtStField').style.display = vmtType==='vortex' ? 'block':'none';
    $('vmtCondField').style.display = vmtType==='magnetic' ? 'block':'none';
    $('vmtMuField').style.display = vmtType==='magnetic' ? 'none':'block';
    ['vortex','magnetic','turbine'].forEach(t=>{
      $('diagram-'+t).classList.toggle('diagram-hidden', t!==vmtType);
    });
    calcVMT();
    updatePrintHeader();
  });
});
function calcVMT(){
  const Qm3ph = parseFloat($('vmtFlow').value);
  const D = parseFloat($('vmtD').value)/1000;
  const rho = parseFloat($('vmtRho').value);
  if(!(Qm3ph>0)||!(D>0)||!(rho>0)){
    $('vmtResults').innerHTML='<div class="result-box"><div class="label">Status</div><div class="value">Enter valid inputs</div></div>';
    $('vmtWarn').innerHTML=''; return;
  }
  const area = Math.PI/4*D*D;
  const Qm3ps = Qm3ph/3600;
  const v = Qm3ps/area;
  let boxes = `<div class="result-box"><div class="label">Velocity in bore</div><div class="value">${v.toFixed(2)} <span>m/s</span></div></div>`;
  let warn='';

  if(vmtType==='vortex'){
    const mu = parseFloat($('vmtMu').value)*0.001; // Pa.s
    const Re = mu>0 ? (rho*v*D/mu) : null;
    const St = parseFloat($('vmtSt').value);
    const bluff = 0.2*D; // typical bluff body width ≈0.2D
    const f = St*v/bluff;
    boxes += `<div class="result-box"><div class="label">Est. shedding frequency</div><div class="value">${f.toFixed(1)} <span>Hz (bluff body ≈${(bluff*1000).toFixed(0)} mm)</span></div></div>`;
    if(Re!==null) boxes += `<div class="result-box"><div class="label">Reynolds number</div><div class="value">${Re.toFixed(0)}</div></div>`;
    if(Re!==null && Re<20000) warn='Re below ~20,000 — vortex shedding may be unstable at this flow; check vendor minimum Re for the specific meter.';
    if(v<1) warn += (warn?' • ':'')+'Velocity below typical vortex meter minimum (~1 m/s liquids, ~4–6 m/s gas/steam) — check turndown.';
  } else if(vmtType==='magnetic'){
    const cond = parseFloat($('vmtCond').value);
    boxes += `<div class="result-box"><div class="label">Feasibility</div><div class="value" style="font-size:0.95rem;">${cond>=5?'Conductivity OK for most mag meters':'Below typical 5 µS/cm minimum — check vendor spec'}</div></div>`;
    if(v<0.3||v>10) warn='Velocity outside typical 0.3–10 m/s mag meter guideline — check vendor curve for the model.';
    warn += (warn?' • ':'')+'Mag meters only work on electrically conductive liquids — not usable on hydrocarbons/non-conductive fluids without a conductive additive.';
  } else { // turbine
    const mu = parseFloat($('vmtMu').value)*0.001;
    const Re = mu>0 ? (rho*v*D/mu) : null;
    if(Re!==null) boxes += `<div class="result-box"><div class="label">Reynolds number</div><div class="value">${Re.toFixed(0)}</div></div>`;
    if(Re!==null && Re<4000) warn='Re below ~4,000 — outside the linear K-factor region for most turbine meters.';
    if(v<0.3||v>7) warn += (warn?' • ':'')+'Velocity outside typical 0.3–7 m/s turbine meter range — check vendor curve; higher for gas turbine meters.';
    warn += (warn?' • ':'')+'Turbine meters need clean, low-viscosity, non-pulsating flow — high viscosity sharply reduces turndown.';
  }
  $('vmtResults').innerHTML = boxes;
  $('vmtWarn').innerHTML = warn ? `<div class="warning">${warn}</div>` : '';
}
['vmtFlow','vmtD','vmtRho','vmtMu','vmtSt','vmtCond'].forEach(id=>$(id).addEventListener('input',calcVMT));
calcVMT();

// ---------- Open Channel: Weir / Flume ----------
const G=9.80665;
document.querySelectorAll('#ocToggle button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('#ocToggle button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    ocType=btn.dataset.oc;
    $('ocAngleField').style.display = ocType==='vnotch' ? 'block':'none';
    $('ocLField').style.display = (ocType==='rect_supp'||ocType==='rect_cont') ? 'block':'none';
    $('ocCdField').style.display = (ocType==='vnotch'||ocType==='rect_supp'||ocType==='rect_cont') ? 'block':'none';
    $('ocWField').style.display = ocType==='parshall' ? 'block':'none';
    $('diagram-weir').classList.toggle('diagram-hidden', ocType==='parshall');
    $('diagram-flume').classList.toggle('diagram-hidden', ocType!=='parshall');
    calcOC();
    updatePrintHeader();
  });
});
function calcOC(){
  const H = parseFloat($('ocH').value)/1000; // m
  if(!(H>0)){ $('ocResults').innerHTML='<div class="result-box"><div class="label">Status</div><div class="value">Enter valid head</div></div>'; return; }
  let Q_m3ps=0, formula='';

  if(ocType==='vnotch'){
    const Cd = parseFloat($('ocCd').value);
    const theta = parseFloat($('ocAngle').value)*Math.PI/180;
    Q_m3ps = (8/15)*Cd*Math.sqrt(2*G)*Math.tan(theta/2)*Math.pow(H,2.5);
    formula = 'Q = (8/15)·Cd·√(2g)·tan(θ/2)·H^(5/2)';
  } else if(ocType==='rect_supp'){
    const Cd = parseFloat($('ocCd').value);
    const L = parseFloat($('ocL').value);
    Q_m3ps = (2/3)*Cd*Math.sqrt(2*G)*L*Math.pow(H,1.5);
    formula = 'Q = (2/3)·Cd·√(2g)·L·H^(3/2)  [Francis, suppressed weir — full channel width, no side contractions]';
  } else if(ocType==='rect_cont'){
    const Cd = parseFloat($('ocCd').value);
    const L = parseFloat($('ocL').value);
    const Leff = Math.max(L-0.2*H,0);
    Q_m3ps = (2/3)*Cd*Math.sqrt(2*G)*Leff*Math.pow(H,1.5);
    formula = 'Q = (2/3)·Cd·√(2g)·(L−0.2H)·H^(3/2)  [Francis, contracted weir — 2 end contractions]';
  } else { // parshall
    const W = parseFloat($('ocW').value); // ft
    const Ha = H/0.3048; // ft
    let Q_cfs;
    if(W===0.25) Q_cfs = 0.992*Math.pow(Ha,1.547);
    else if(W===0.5) Q_cfs = 2.06*Math.pow(Ha,1.58);
    else if(W===0.75) Q_cfs = 3.07*Math.pow(Ha,1.53);
    else Q_cfs = 4*W*Math.pow(Ha,1.522*Math.pow(W,0.026));
    Q_m3ps = Q_cfs*0.0283168;
    formula = 'Parshall flume free-flow equation (US customary, converted) — check submergence ratio Hb/Ha; equation only valid for free (unsubmerged) flow.';
  }

  const Q_m3ph = Q_m3ps*3600;
  $('ocResults').innerHTML = `<div class="result-box primary"><div class="label">Flow rate</div><div class="value">${Q_m3ph.toFixed(2)} <span>m³/hr</span></div></div>`;
  $('ocFormulaNote').textContent = formula;
}
document.querySelectorAll('#ocH,#ocAngle,#ocL,#ocCd,#ocW').forEach(()=>{});
['ocH','ocAngle','ocL','ocCd','ocW'].forEach(id=>$(id).addEventListener('input',calcOC));
calcOC();
updatePrintHeader();
</script>
</body>
</html>
