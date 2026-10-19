<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
</head>
<body>
<title>Louisiana Tree Map</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,600;12..96,800&family=Atkinson+Hyperlegible:wght@400;700&display=swap">
<style>
/* Layout: field-journal map on the left, a sliding notebook panel on the right; stacks on phones */
:root{
  --bg:#EEF3EC; --paper:#FFFFFF; --ink:#16302A; --muted:#5B6F66; --line:#C9D8CF;
  --land:#D9E8C9; --land-edge:#4F7A4A; --water:#BFDCE3; --water-deep:#8FC1CE; --river:#6FA8BA;
  --pin:#1F6B4F; --pin-ring:#FFFFFF; --accent:#D98E04; --accent-ink:#3B2600; --danger:#B3261E; --chip:#E3EEE6;
  --display:"Bricolage Grotesque", "Trebuchet MS", system-ui, sans-serif;
  --body:"Atkinson Hyperlegible", "Segoe UI", system-ui, sans-serif;
}
@media (prefers-color-scheme: dark){:root:not([data-theme="light"]){
  --bg:#0F1A17; --paper:#17251F; --ink:#E4EFE8; --muted:#9DB3A8; --line:#2C4038;
  --land:#24402F; --land-edge:#7FB27A; --water:#11303A; --water-deep:#0B2530; --river:#3E8095;
  --pin:#5FD3A0; --pin-ring:#0F1A17; --accent:#F2B13C; --accent-ink:#2A1A00; --danger:#FF8A80; --chip:#203229; color-scheme:dark}}
:root[data-theme="dark"]{
  --bg:#0F1A17; --paper:#17251F; --ink:#E4EFE8; --muted:#9DB3A8; --line:#2C4038;
  --land:#24402F; --land-edge:#7FB27A; --water:#11303A; --water-deep:#0B2530; --river:#3E8095;
  --pin:#5FD3A0; --pin-ring:#0F1A17; --accent:#F2B13C; --accent-ink:#2A1A00; --danger:#FF8A80; --chip:#203229; color-scheme:dark}
*{box-sizing:border-box}
body{background:var(--bg);color:var(--ink);font-family:var(--body);font-size:16px;line-height:1.5;
  padding-left:max(16px,env(safe-area-inset-left,0px));padding-right:max(16px,env(safe-area-inset-right,0px));padding-block:20px 40px}
.wrap{max-width:1180px;margin:0 auto;display:flex;flex-direction:column;gap:18px}
header{display:flex;flex-wrap:wrap;align-items:flex-end;justify-content:space-between;gap:12px}
h1{font-family:var(--display);font-weight:800;font-size:clamp(28px,5vw,46px);line-height:1.02;margin:0;letter-spacing:-.01em;text-wrap:balance}
.sub{color:var(--muted);margin:6px 0 0;max-width:60ch}
.tally{display:flex;gap:10px;flex-wrap:wrap}
.stat{background:var(--paper);border:1px solid var(--line);border-radius:12px;padding:8px 14px;min-width:110px}
.stat b{display:block;font-family:var(--display);font-size:28px;line-height:1;font-variant-numeric:tabular-nums}
.stat span{font-size:12px;letter-spacing:.06em;text-transform:uppercase;color:var(--muted)}
.main{display:grid;grid-template-columns:minmax(0,1.45fr) minmax(0,1fr);gap:18px;align-items:start}
@media (max-width:860px){.main{grid-template-columns:1fr}}
.mapbox{background:var(--water);border:1px solid var(--line);border-radius:18px;overflow:hidden;position:relative}
.mapbox svg{display:block;width:100%;height:auto;touch-action:manipulation}
.maphint{position:absolute;left:12px;bottom:10px;right:12px;font-size:13px;color:var(--ink);background:color-mix(in srgb,var(--paper) 85%,transparent);
  border-radius:10px;padding:6px 10px;pointer-events:none}
.maphint[hidden]{display:none}
.pin{cursor:pointer}
.pin circle.dot{fill:var(--pin);stroke:var(--pin-ring);stroke-width:2.5}
.pin text{fill:var(--pin-ring);font:700 15px var(--body);pointer-events:none}
.pin:hover circle.dot,.pin:focus circle.dot,.pin.sel circle.dot{fill:var(--accent);outline:none}
.pin:focus{outline:none}
.pin.sel text,.pin:hover text{fill:var(--accent-ink)}
.mine circle.dot{stroke:var(--accent);stroke-width:4}
.panel{background:var(--paper);border:1px solid var(--line);border-radius:18px;padding:18px;display:flex;flex-direction:column;gap:14px;min-width:0}
.tabs{display:flex;gap:6px;background:var(--chip);padding:4px;border-radius:12px}
.tabs button{flex:1;border:0;background:transparent;color:var(--ink);font:700 15px var(--body);padding:9px 8px;border-radius:9px;cursor:pointer}
.tabs button[aria-selected="true"]{background:var(--paper);box-shadow:0 1px 3px rgb(0 0 0 / .12)}
h2{font-family:var(--display);font-weight:800;font-size:24px;margin:0;line-height:1.1}
label{font-weight:700;font-size:14px;display:block;margin-bottom:4px}
.field{display:flex;flex-direction:column}
input[type=text],select,textarea{font:16px var(--body);color:var(--ink);background:var(--bg);border:1.5px solid var(--line);border-radius:10px;padding:10px 12px;width:100%}
textarea{resize:vertical;min-height:64px}
input:focus,select:focus,textarea:focus,button:focus-visible{outline:3px solid var(--accent);outline-offset:1px}
.help{font-size:13px;color:var(--muted);margin-top:4px}
.drop{border:2px dashed var(--line);border-radius:14px;padding:14px;text-align:center;cursor:pointer;display:flex;flex-direction:column;align-items:center;gap:8px}
.vh{position:absolute;width:1px;height:1px;opacity:0;overflow:hidden;clip:rect(0 0 0 0)}
.drop img{max-height:180px;border-radius:10px}
.btn{font:700 16px var(--body);border:0;border-radius:12px;padding:12px 16px;cursor:pointer;background:var(--pin);color:var(--pin-ring)}
.btn.ghost{background:transparent;color:var(--ink);border:1.5px solid var(--line)}
.btn.danger{background:transparent;color:var(--danger);border:1.5px solid var(--danger);padding:6px 10px;font-size:13px}
.btn:disabled{opacity:.55;cursor:wait}
.row{display:flex;gap:10px;flex-wrap:wrap;align-items:center}
.safety{background:var(--chip);border-radius:12px;padding:10px 12px;font-size:14px}
.msg{font-size:14px;min-height:1.2em}
.msg.err{color:var(--danger)}
.cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(150px,1fr));gap:12px}
.card{border:1px solid var(--line);border-radius:14px;overflow:hidden;background:var(--bg);display:flex;flex-direction:column}
.card img{width:100%;aspect-ratio:1/1;object-fit:cover;max-width:100%;display:block;background:var(--chip)}
.card .meta{padding:8px 10px;display:flex;flex-direction:column;gap:2px}
.card .who{font-weight:700}
.card .kind{font-size:13px;color:var(--muted)}
.card .note{font-size:13px}
.empty{color:var(--muted);text-align:center;padding:20px 8px}
.notice{background:var(--chip);border-radius:12px;padding:14px;font-size:15px}
.list{display:flex;flex-direction:column;gap:6px;max-height:420px;overflow:auto}
.list button{display:flex;justify-content:space-between;gap:8px;text-align:left;border:1px solid var(--line);background:var(--bg);color:var(--ink);border-radius:10px;padding:8px 12px;font:15px var(--body);cursor:pointer}
.list button b{font-variant-numeric:tabular-nums}
@media (prefers-reduced-motion: no-preference){.pin circle.dot{transition:fill .15s}}
</style>

<div class="wrap">
  <header>
    <div>
      <h1>Louisiana Tree Map</h1>
      <p class="sub">Snap a tree near you, add your town, and watch our class forest grow across the whole state.</p>
    </div>
    <div class="tally" aria-live="polite">
      <div class="stat"><b id="nTrees">0</b><span>trees</span></div>
      <div class="stat"><b id="nTowns">0</b><span>towns</span></div>
    </div>
  </header>

  <div class="main">
    <div class="mapbox">
      <svg id="map" viewBox="0 0 960 890" role="img" aria-label="Map of Louisiana with student tree pins"></svg>
      <p class="maphint" id="mapHint" hidden>Tap the map where your tree is.</p>
    </div>

    <section class="panel">
      <div class="tabs" role="tablist">
        <button role="tab" id="tabAdd" aria-selected="true">Add my tree</button>
        <button role="tab" id="tabSee" aria-selected="false">See trees</button>
      </div>

      <div id="viewAdd">
        <div id="addGate" class="notice" hidden></div>
        <div id="nojs" class="notice" style="border:2px solid var(--danger)"><b>This page can’t save trees here.</b> It’s being shown in a preview that turns off the map’s buttons. Download the file, then open it in Chrome, Edge, or Safari, or use the class map link from your teacher.</div>
        <form id="form" novalidate action="#" onsubmit="return false">
          <div style="display:flex;flex-direction:column;gap:12px">
            <div class="safety">📸 Take a picture of just the tree. No faces, no house numbers, and use your <b>first name only</b>.</div>
            <div class="field">
              <label for="fName">First name</label>
              <input type="text" id="fName" maxlength="20" autocomplete="off" placeholder="e.g. Jordan">
            </div>
            <div class="field">
              <label for="fCity">Your town or city</label>
              <input type="text" id="fCity" list="cityList" maxlength="40" autocomplete="off" placeholder="Start typing… e.g. Lafayette">
              <datalist id="cityList"></datalist>
              <div class="help" id="cityHelp">Pick your town from the list. Not there? Type it, then tap the map near it.</div>
            </div>
            <div class="field">
              <label for="fKind">What kind of tree? (best guess is fine)</label>
              <select id="fKind">
                <option>I’m not sure</option><option>Live oak</option><option>Bald cypress</option><option>Southern magnolia</option>
                <option>Pine</option><option>Pecan</option><option>Crape myrtle</option><option>Palm</option><option>Willow</option>
                <option>Maple</option><option>Sweetgum</option><option>Fruit tree</option><option>Other</option>
              </select>
            </div>
            <div class="field">
              <label for="fNote">One cool thing about your tree</label>
              <textarea id="fNote" maxlength="140" placeholder="It has Spanish moss! / Its roots crack the sidewalk!"></textarea>
            </div>
            <div class="field">
              <label for="fPhoto">Tree photo</label>
              <label class="drop" for="fPhoto" id="drop">
                <img id="preview" alt="" hidden>
                <span id="dropText">Tap to choose a photo</span>
              </label>
              <input type="file" id="fPhoto" accept="image/*" class="vh">
            </div>
            <div class="row">
              <button class="btn" id="submit" type="submit" disabled>Loading the map…</button>
              <button class="btn danger" id="removeMine" type="button" hidden>Remove my tree</button>
            </div>
            <p class="msg" id="msg" role="status"></p>
          </div>
        </form>
      </div>

      <div id="viewSee" hidden>
        <div id="seeHead" class="row" style="justify-content:space-between">
          <h2 id="seeTitle">All towns</h2>
          <button class="btn ghost" id="backAll" type="button" hidden>All towns</button>
        </div>
        <div id="seeBody"></div>
      </div>
    </section>
  </div>
</div>

<script>
(() => {
const CITIES = {"Abbeville":[29.97,-92.13],"Addis":[30.35,-91.26],"Alexandria":[31.31,-92.45],"Amite":[30.73,-90.51],"Arcadia":[32.55,-92.92],"Baker":[30.59,-91.17],"Bastrop":[32.78,-91.91],"Baton Rouge":[30.45,-91.15],"Belle Chasse":[29.85,-89.99],"Benton":[32.69,-93.74],"Bogalusa":[30.79,-89.85],"Bossier City":[32.52,-93.73],"Breaux Bridge":[30.27,-91.9],"Broussard":[30.15,-91.96],"Brusly":[30.39,-91.25],"Bunkie":[30.95,-92.18],"Cameron":[29.8,-93.32],"Carencro":[30.32,-92.05],"Central":[30.55,-91.04],"Chalmette":[29.94,-89.96],"Columbia":[32.1,-92.08],"Coushatta":[32.02,-93.34],"Covington":[30.48,-90.1],"Crowley":[30.21,-92.37],"Delcambre":[29.95,-91.99],"Delhi":[32.46,-91.49],"Denham Springs":[30.49,-90.96],"DeQuincy":[30.45,-93.43],"DeRidder":[30.85,-93.29],"Destrehan":[29.94,-90.35],"Donaldsonville":[30.1,-90.99],"Eunice":[30.49,-92.42],"Farmerville":[32.77,-92.41],"Ferriday":[31.63,-91.55],"Franklin":[29.8,-91.5],"Galliano":[29.44,-90.3],"Golden Meadow":[29.38,-90.27],"Gonzales":[30.24,-90.92],"Grambling":[32.53,-92.71],"Grand Isle":[29.24,-89.99],"Greenwell Springs":[30.54,-90.99],"Gretna":[29.91,-90.05],"Hammond":[30.5,-90.46],"Harvey":[29.9,-90.08],"Haughton":[32.53,-93.5],"Homer":[32.79,-93.06],"Houma":[29.6,-90.72],"Iowa":[30.24,-93.01],"Jena":[31.68,-92.13],"Jennings":[30.22,-92.66],"Jonesboro":[32.24,-92.72],"Kaplan":[29.99,-92.28],"Kenner":[29.99,-90.24],"Lafayette":[30.22,-92.02],"Lake Arthur":[30.08,-92.67],"Lake Charles":[30.23,-93.22],"Lake Providence":[32.81,-91.17],"LaPlace":[30.07,-90.48],"Leesville":[31.14,-93.26],"Livingston":[30.5,-90.75],"Luling":[29.93,-90.37],"Lutcher":[30.04,-90.7],"Mamou":[30.63,-92.42],"Mandeville":[30.36,-90.07],"Mansfield":[32.04,-93.7],"Mansura":[31.06,-92.05],"Many":[31.57,-93.48],"Marksville":[31.13,-92.07],"Marrero":[29.9,-90.1],"Maurice":[30.11,-92.12],"Metairie":[29.98,-90.15],"Minden":[32.62,-93.29],"Monroe":[32.51,-92.12],"Morgan City":[29.7,-91.21],"Moss Bluff":[30.3,-93.19],"Napoleonville":[29.94,-91.02],"Natchitoches":[31.76,-93.09],"New Iberia":[30.0,-91.82],"New Orleans":[29.95,-90.07],"Oak Grove":[32.86,-91.39],"Oakdale":[30.82,-92.66],"Opelousas":[30.53,-92.08],"Pierre Part":[29.96,-91.2],"Pineville":[31.32,-92.43],"Plaquemine":[30.29,-91.23],"Ponchatoula":[30.44,-90.44],"Port Allen":[30.45,-91.21],"Prairieville":[30.3,-90.97],"Raceland":[29.73,-90.6],"Rayne":[30.23,-92.27],"Rayville":[32.47,-91.76],"Reserve":[30.05,-90.55],"Ruston":[32.52,-92.64],"Scott":[30.24,-92.09],"Shreveport":[32.52,-93.75],"Slidell":[30.28,-89.78],"Springhill":[33.0,-93.47],"St. Francisville":[30.78,-91.38],"St. Gabriel":[30.26,-91.1],"St. Martinville":[30.13,-91.83],"Sterlington":[32.7,-92.09],"Sulphur":[30.24,-93.38],"Tallulah":[32.41,-91.19],"Thibodaux":[29.8,-90.82],"Vacherie":[29.97,-90.71],"Vidalia":[31.57,-91.43],"Ville Platte":[30.69,-92.27],"Vinton":[30.19,-93.58],"Walker":[30.49,-90.86],"Watson":[30.58,-90.95],"West Monroe":[32.52,-92.15],"Westlake":[30.24,-93.25],"Westwego":[29.91,-90.14],"Winnfield":[31.93,-92.64],"Winnsboro":[32.16,-91.72],"Youngsville":[30.1,-91.99],"Zachary":[30.65,-91.16]};
const OUTLINE = [[-94.04,33.02],[-91.17,33.0],[-91.08,32.95],[-91.14,32.62],[-91.0,32.35],[-91.06,32.1],[-91.25,31.85],[-91.42,31.62],[-91.52,31.3],[-91.62,31.0],[-89.73,31.0],[-89.75,30.65],[-89.63,30.36],[-89.55,30.18],[-89.66,30.08],[-89.4,30.02],[-89.6,29.85],[-89.38,29.62],[-89.18,29.33],[-89.05,29.15],[-89.38,29.0],[-89.62,29.2],[-90.0,29.12],[-90.12,29.2],[-90.4,29.06],[-90.75,29.05],[-91.0,29.2],[-91.3,29.25],[-91.55,29.5],[-91.85,29.48],[-92.05,29.58],[-92.3,29.54],[-92.65,29.6],[-93.0,29.75],[-93.4,29.77],[-93.86,29.7],[-93.73,30.0],[-93.71,30.4],[-93.55,30.98],[-93.6,31.2],[-93.8,31.55],[-94.04,31.99]];
const RIVER = [[-91.17,33.0],[-91.14,32.62],[-91.0,32.35],[-91.25,31.85],[-91.62,31.0],[-91.45,30.6],[-91.2,30.45],[-90.98,30.1],[-90.6,30.02],[-90.08,29.93],[-89.9,29.8],[-89.6,29.55],[-89.25,29.2]];
const LABELS = [["Shreveport",-93.75,32.52],["Monroe",-92.12,32.51],["Alexandria",-92.45,31.31],["Lake Charles",-93.22,30.23],["Lafayette",-92.02,30.22],["Baton Rouge",-91.15,30.45],["New Orleans",-90.07,29.95]];
const X = lon => (lon + 94.2) * 178, Y = lat => (33.15 - lat) * 208;
const inv = (x, y) => [33.15 - y / 208, x / 178 - 94.2];
const NS = "http://www.w3.org/2000/svg";
const $ = id => document.getElementById(id);
const svg = $("map");
const el = (tag, attrs = {}, parent) => { const n = document.createElementNS(NS, tag); for (const k in attrs) n.setAttribute(k, attrs[k]); if (parent) parent.appendChild(n); return n; };
const cssVar = n => getComputedStyle(document.documentElement).getPropertyValue(n).trim();

// ---- base map ----
const base = el("g", {}, svg);
el("rect", {x: 0, y: 0, width: 960, height: 890, fill: "var(--water)"}, base);
el("path", {d: "M0,780 Q480,720 960,800 L960,890 L0,890Z", fill: "var(--water-deep)", opacity: ".6"}, base);
el("path", {d: "M" + OUTLINE.map(p => X(p[0]).toFixed(1) + "," + Y(p[1]).toFixed(1)).join(" L") + "Z", fill: "var(--land)", stroke: "var(--land-edge)", "stroke-width": 3, "stroke-linejoin": "round"}, base);
el("ellipse", {cx: X(-90.1), cy: Y(30.19), rx: 52, ry: 20, fill: "var(--water)", stroke: "var(--land-edge)", "stroke-width": 1.5}, base);
el("path", {d: "M" + RIVER.map(p => X(p[0]).toFixed(1) + "," + Y(p[1]).toFixed(1)).join(" L"), fill: "none", stroke: "var(--river)", "stroke-width": 4, "stroke-linecap": "round", "stroke-linejoin": "round"}, base);
const lbl = el("g", {fill: "var(--muted)", "font-size": 14, "font-family": "var(--body)"}, base);
for (const [n, lo, la] of LABELS) { el("circle", {cx: X(lo), cy: Y(la), r: 3, fill: "var(--muted)"}, lbl); const t = el("text", {x: X(lo) + 7, y: Y(la) - 6}, lbl); t.textContent = n; }
const gulf = el("text", {x: 640, y: 860, fill: "var(--muted)", "font-size": 20, "font-style": "italic", "font-family": "var(--body)"}, base); gulf.textContent = "Gulf";
const pinLayer = el("g", {}, svg);
const tempLayer = el("g", {}, svg);

// ---- state ----
let trees = [];            // [{id, ...data}]
let myId = null, owner = false, db = null, assets = null, canWrite = false, local = false;
const LKEY = "la-tree-map-v1";
function loadLocal(){ try { return JSON.parse(localStorage.getItem(LKEY) || "[]"); } catch { return []; } }
function saveLocal(){ try { localStorage.setItem(LKEY, JSON.stringify(trees)); return true; } catch { return false; } }
function blobToDataUrl(b){ return new Promise((res, rej) => { const r = new FileReader(); r.onload = () => res(r.result); r.onerror = rej; r.readAsDataURL(b); }); }
function startLocal(msg){ local = true; owner = true; canWrite = true; trees = loadLocal(); gate(msg); renderPins(); renderAll(); }
let photoBlob = null, customPt = null, picking = false, selectedKey = null;

const normCity = s => (s || "").trim().replace(/\s+/g, " ");
const cityKey = s => normCity(s).toLowerCase();
const findCity = s => { const k = cityKey(s).replace(/^st\.? /, "st. ").replace(/,?\s*(la|louisiana)$/, "").trim(); if (!k) return null;
  for (const n in CITIES) if (n.toLowerCase() === k) return n;
  for (const n in CITIES) if (n.toLowerCase().replace(/[^a-z]/g, "") === k.replace(/[^a-z]/g, "")) return n;
  const starts = Object.keys(CITIES).filter(n => n.toLowerCase().startsWith(k)); if (k.length >= 4 && starts.length === 1) return starts[0];
  return null; };
const dl = $("cityList"); Object.keys(CITIES).sort().forEach(n => { const o = document.createElement("option"); o.value = n; dl.appendChild(o); });

function groups() {
  const g = new Map();
  for (const t of trees) {
    if (typeof t.lat !== "number" || typeof t.lon !== "number") continue;
    const k = cityKey(t.city) || (t.lat.toFixed(2) + "," + t.lon.toFixed(2));
    if (!g.has(k)) g.set(k, {key: k, city: normCity(t.city) || "Somewhere", lat: t.lat, lon: t.lon, items: []});
    g.get(k).items.push(t);
  }
  return [...g.values()];
}

function renderPins() {
  pinLayer.replaceChildren();
  const gs = groups().sort((a, b) => b.items.length - a.items.length);
  for (const g of gs) {
    const n = g.items.length, r = Math.min(26, 11 + Math.sqrt(n) * 4.5);
    const mine = g.items.some(t => t.id === myId);
    const p = el("g", {class: "pin" + (mine ? " mine" : "") + (g.key === selectedKey ? " sel" : ""), tabindex: 0, role: "button", "aria-label": `${g.city}: ${n} tree${n > 1 ? "s" : ""}`, transform: `translate(${X(g.lon).toFixed(1)},${Y(g.lat).toFixed(1)})`}, pinLayer);
    el("circle", {class: "dot", r}, p);
    const t = el("text", {"text-anchor": "middle", dy: 5}, p); t.textContent = n;
    const open = () => showCity(g.key);
    p.addEventListener("click", e => { if (picking) return; e.stopPropagation(); open(); });
    p.addEventListener("keydown", e => { if (e.key === "Enter" || e.key === " ") { e.preventDefault(); open(); } });
  }
  $("nTrees").textContent = trees.length;
  $("nTowns").textContent = gs.length;
}

// ---- tabs ----
function tab(which) {
  const add = which === "add";
  $("tabAdd").setAttribute("aria-selected", add); $("tabSee").setAttribute("aria-selected", !add);
  $("viewAdd").hidden = !add; $("viewSee").hidden = add;
  if (add) { selectedKey = null; renderPins(); } else if (!selectedKey) renderAll();
}
$("tabAdd").onclick = () => tab("add");
$("tabSee").onclick = () => { selectedKey = null; tab("see"); renderAll(); };
$("backAll").onclick = () => { selectedKey = null; renderPins(); renderAll(); };

function renderAll() {
  $("seeTitle").textContent = "All towns"; $("backAll").hidden = true;
  const body = $("seeBody"); body.replaceChildren();
  const gs = groups().sort((a, b) => b.items.length - a.items.length || a.city.localeCompare(b.city));
  if (!gs.length) { const p = document.createElement("p"); p.className = "empty"; p.textContent = db ? "No trees on the map yet. Be the first to add one!" : "Trees will show up here once the map can load."; body.appendChild(p); return; }
  const list = document.createElement("div"); list.className = "list";
  for (const g of gs) {
    const b = document.createElement("button"); b.type = "button";
    const s1 = document.createElement("span"); s1.textContent = g.city;
    const s2 = document.createElement("b"); s2.textContent = g.items.length + (g.items.length > 1 ? " trees" : " tree");
    b.append(s1, s2); b.onclick = () => showCity(g.key); list.appendChild(b);
  }
  body.appendChild(list);
}

function showCity(key) {
  selectedKey = key; tab("see"); renderPins();
  const g = groups().find(x => x.key === key); if (!g) { renderAll(); return; }
  $("seeTitle").textContent = g.city; $("backAll").hidden = false;
  const body = $("seeBody"); body.replaceChildren();
  const cards = document.createElement("div"); cards.className = "cards";
  for (const t of g.items.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0))) {
    const c = document.createElement("article"); c.className = "card";
    const img = document.createElement("img"); img.loading = "lazy"; img.alt = `${t.kind || "Tree"} photo from ${t.name || "a student"}`;
    if (t.photoUrl || t.assetId) img.src = t.photoUrl || ("/_blob/" + t.assetId);
    const m = document.createElement("div"); m.className = "meta";
    const w = document.createElement("span"); w.className = "who"; w.textContent = (t.name || "A student") + (t.id === myId ? " (you)" : "");
    const k = document.createElement("span"); k.className = "kind"; k.textContent = t.kind || "";
    m.append(w, k);
    if (t.note) { const n = document.createElement("span"); n.className = "note"; n.textContent = t.note; m.appendChild(n); }
    if (owner && (local || t.id !== myId)) {
      const del = document.createElement("button"); del.className = "btn danger"; del.type = "button"; del.textContent = "Remove";
      let armed = false;
      del.onclick = async () => {
        if (!armed) { armed = true; del.textContent = "Tap again to remove"; return; }
        del.disabled = true;
        try { if (local) { trees = trees.filter(x => x.id !== t.id); saveLocal(); renderPins(); showCity(key); return; }
          await db.collection("trees").doc(t.id).delete(); if (t.assetId && assets) assets.delete(t.assetId).catch(() => {}); }
        catch (e) { del.textContent = "Couldn’t remove"; }
      };
      m.appendChild(del);
    }
    c.append(img, m); cards.appendChild(c);
  }
  body.appendChild(cards);
}

// ---- map picking for unknown towns ----
function setPicking(on) { picking = on; $("mapHint").hidden = !on; svg.style.cursor = on ? "crosshair" : ""; }
function drawTemp() {
  tempLayer.replaceChildren();
  if (!customPt) return;
  const g = el("g", {transform: `translate(${X(customPt[1])},${Y(customPt[0])})`}, tempLayer);
  el("circle", {r: 14, fill: "var(--accent)", stroke: "var(--pin-ring)", "stroke-width": 3}, g);
}
svg.addEventListener("click", e => {
  if (!picking) return;
  const pt = svg.createSVGPoint(); pt.x = e.clientX; pt.y = e.clientY;
  const p = pt.matrixTransform(svg.getScreenCTM().inverse());
  customPt = inv(p.x, p.y); drawTemp(); setPicking(false);
  say("Spot saved! Now add your photo.");
});
$("fCity").addEventListener("input", () => {
  const known = findCity($("fCity").value);
  if (known) { customPt = null; drawTemp(); setPicking(false); $("cityHelp").textContent = "Got it: " + known + "."; }
  else if (normCity($("fCity").value).length > 2) { setPicking(!customPt); $("cityHelp").textContent = customPt ? "Spot saved on the map. Tap the map again to move it." : "We don’t have that town yet. Tap the map near where it is."; }
});

// ---- photo ----
let photoDataUrl = null;
$("fPhoto").addEventListener("change", async () => {
  const f = $("fPhoto").files[0]; if (!f) return;
  say("Loading your photo…");
  try {
    photoDataUrl = await blobToDataUrl(f);
    try { photoBlob = await shrink(photoDataUrl, 1100); photoDataUrl = await blobToDataUrl(photoBlob); }
    catch { if (f.size > 8 * 1024 * 1024) throw new Error("big"); photoBlob = f; }
    $("preview").src = photoDataUrl; $("preview").hidden = false;
    $("dropText").textContent = "Tap to choose a different photo"; say("Photo ready! ✔");
  } catch (e) {
    photoBlob = null; photoDataUrl = null;
    say(e && e.message === "big" ? "That photo is too big. Try a smaller one or take a screenshot of it." : "That file didn’t work as a photo. Try a JPG or PNG.", true);
  }
});
function loadImg(src) { return new Promise((res, rej) => { const img = new Image(); img.onload = () => res(img); img.onerror = rej; img.src = src; }); }
async function shrink(src, maxSide) {
  let w, h, draw;
  try { const bmp = await createImageBitmap(typeof src === "string" ? await (await fetch(src)).blob() : src); w = bmp.width; h = bmp.height; draw = bmp; }
  catch { const img = await loadImg(typeof src === "string" ? src : await blobToDataUrl(src)); w = img.naturalWidth; h = img.naturalHeight; draw = img; }
  if (!w || !h) throw new Error("decode");
  const s = Math.min(1, (maxSide || 1100) / Math.max(w, h));
  const c = document.createElement("canvas"); c.width = Math.round(w * s); c.height = Math.round(h * s);
  c.getContext("2d").drawImage(draw, 0, 0, c.width, c.height);
  const d = c.toDataURL("image/jpeg", 0.82);
  const bin = atob(d.split(",")[1]); const u8 = new Uint8Array(bin.length); for (let i = 0; i < bin.length; i++) u8[i] = bin.charCodeAt(i);
  return new Blob([u8], {type: "image/jpeg"});
}
function shrinkBlob(b, max) { return shrink(b, max); }

function say(t, err, field) { const m = $("msg"); m.textContent = t; m.className = "msg" + (err ? " err" : ""); try { m.scrollIntoView({block: "nearest", behavior: "smooth"}); } catch {} if (field) { const f = $(field); f.style.borderColor = "var(--danger)"; f.focus(); setTimeout(() => f.style.borderColor = "", 2500); } }

// ---- submit ----
async function addLocal(entry, note) {
  let url; try { url = await blobToDataUrl(await shrinkBlob(photoBlob, 700)); } catch { url = photoDataUrl; }
  trees.push(Object.assign({id: "l" + Date.now(), photoUrl: url, createdAt: Date.now()}, entry));
  const ok = saveLocal(); renderPins();
  resetForm();
  say((note ? note + " " : "") + (ok ? "Tree added to the map! 🌳 Next student can add theirs." : "Tree added to the map! 🌳 (It will disappear if this page is closed.)"));
}
function resetForm() { $("form").reset(); $("preview").hidden = true; $("dropText").textContent = "Tap to choose a photo"; photoBlob = null; photoDataUrl = null; customPt = null; drawTemp(); setPicking(false); $("cityHelp").textContent = "Pick your town from the list. Not there? Type it, then tap the map near it."; }
const LA_CENTER = [31.0, -92.0];
$("form").addEventListener("submit", async e => {
  e.preventDefault();
  const name = normCity($("fName").value), cityRaw = normCity($("fCity").value);
  const known = findCity(cityRaw);
  if (!name) return say("Type your first name first.", true, "fName");
  if (!cityRaw) return say("Type your town or city.", true, "fCity");
  // read the photo now if the 'change' event never fired
  if (!photoBlob && $("fPhoto").files && $("fPhoto").files[0]) {
    const f = $("fPhoto").files[0];
    try { photoDataUrl = await blobToDataUrl(f); try { photoBlob = await shrink(photoDataUrl, 1100); } catch { photoBlob = f; } } catch {}
  }
  if (!photoBlob && (local || !mine())) return say("Choose a photo of your tree (tap the photo box).", true, "drop");
  const pt = known ? CITIES[known] : (customPt || LA_CENTER);
  const unplaced = !known && !customPt;
  const entry = {name: name.slice(0, 20), city: (known || cityRaw).slice(0, 40), lat: pt[0], lon: pt[1], kind: $("fKind").value, note: normCity($("fNote").value).slice(0, 140)};
  const placeNote = unplaced ? "We didn’t find that town, so your pin is in the middle of the state." : "";
  const btn = $("submit"); btn.disabled = true; say("Planting your tree on the map…");
  try {
    if (local || !db || !canWrite || !myId) {
      await addLocal(entry, (!local ? "Saved on this device only — ask your teacher for edit access to add it to the class map." : "") + (placeNote ? " " + placeNote : ""));
      return;
    }
    const prev = mine();
    let assetId = prev && prev.assetId, photoUrl = prev && prev.photoUrl;
    if (photoBlob) {
      if (!assets) throw {message: "Photo uploads aren’t turned on for you."};
      const up = await assets.upload(photoBlob, {type: photoBlob.type || "image/jpeg"});
      assetId = up.id; photoUrl = up.url;
    }
    await db.collection("trees").doc(myId).set(Object.assign({}, entry, {assetId: assetId || "", photoUrl: photoUrl || "", createdAt: Date.now()}));
    if (prev && prev.assetId && prev.assetId !== assetId && assets) assets.delete(prev.assetId).catch(() => {});
    photoBlob = null; customPt = null; drawTemp(); setPicking(false);
    say("Your tree is on the class map! 🌳 " + placeNote);
  } catch (err) {
    const why = (err && err.code === "too_large") ? "That photo was too big for the class map." : (err && err.code === "unsupported_type") ? "That kind of photo can’t go on the class map." : "The class map couldn’t save it" + (err && err.code ? " (" + err.code + ")" : "") + ".";
    try { await addLocal(entry, why + " It’s saved on this device for now."); } catch { say(why + " Try again in a minute.", true); }
  } finally { btn.disabled = false; }
});

const mine = () => trees.find(t => t.id === myId);
function fillMine() {
  const m = mine(); $("removeMine").hidden = !m;
  $("submit").textContent = m ? "Update my tree" : "Put my tree on the map";
  if (m && !$("fName").value) { $("fName").value = m.name || ""; $("fCity").value = m.city || ""; $("fKind").value = m.kind || "I’m not sure"; $("fNote").value = m.note || ""; if (!findCity(m.city)) customPt = [m.lat, m.lon];
    if (m.assetId || m.photoUrl) { $("preview").src = m.photoUrl || ("/_blob/" + m.assetId); $("preview").hidden = false; $("dropText").textContent = "Tap to choose a different photo"; } }
}
let armedRemove = false;
$("removeMine").onclick = async () => {
  if (!armedRemove) { armedRemove = true; $("removeMine").textContent = "Tap again to remove"; return; }
  armedRemove = false; $("removeMine").textContent = "Remove my tree";
  const m = mine(); if (!m) return;
  try { await db.collection("trees").doc(myId).delete(); if (m.assetId && assets) assets.delete(m.assetId).catch(() => {}); say("Your tree was removed."); $("form").reset(); $("preview").hidden = true; $("dropText").textContent = "Tap to choose a photo"; }
  catch { say("Couldn’t remove it. Try again.", true); }
};

function gate(text) { const g = $("addGate"); g.textContent = text; g.hidden = !text; }

renderPins(); renderAll();
gate("");
$("nojs").hidden = true; $("submit").disabled = false; $("submit").textContent = "Put my tree on the map";

// ---- capabilities ----
(async () => {
  const c = window.claude;
  if (c && c.use) { local = true; owner = true; trees = loadLocal(); renderPins(); }
  if (!c || !c.use) { startLocal("Classroom mode: trees added here are saved on this computer only. To share one map with all students, use the Claude class link."); return; }
  const [dbNs, user, as] = await Promise.all([c.use("db"), c.use("user"), c.use("assets")]);
  db = dbNs; assets = as;
  if (db) { local = false; owner = false; trees = []; }
  if (!db) { startLocal("You’re not signed in, so trees you add are saved on this device only. Sign in with the account your teacher shared the map with to add yours to the class map."); return; }
  if (user) { myId = await user.id(); owner = await user.isOwner(); const cw = await user.can("data.write"); canWrite = cw !== false; }
  if (!myId) { gate("Sign in to add your tree. You can still look at everyone’s trees."); }
  else if (!canWrite) gate("You can look at the trees, but adding one needs edit access. Ask your teacher.");
  else gate("");
  db.collection("trees").onSnapshot(snap => {
    trees = snap.docs.map(d => Object.assign({id: d.id}, d.data()));
    renderPins(); fillMine();
    if (!$("viewSee").hidden) { if (selectedKey) showCity(selectedKey); else renderAll(); }
  }, () => { gate("The map couldn’t load right now. Try again in a minute."); });
})();
})();
</script>

</body>
</html>
