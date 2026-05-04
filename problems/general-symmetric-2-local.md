---
type: problem
---
# General Symmetric 2-Local

Here we consider a general class of 2-Local Hamiltonian problems first introduced by [[CM16]]({{site.baseurl}}/bib#CM16). These Hamiltonians are 2-Local, so they can be specified by a graph 

$$
 H = \sum_{(i,j) \in E} w_{ij} K_{ij}
$$

where $$E$$ is the set of edges in the graph, $$w_{ij}$$ are edge weights, and $$K$$ is a two local term (when specified with indices $$i$$ and $$j$$, this means that $$K$$ acts nontrivially on qubits $$i$$ and $$j$$ and as the identity on the rest). 

[[CM16]]({{site.baseurl}}/bib#CM16) considers the case where $$w_{ij}$$ can be positive or negative, and classifies the problem as either in $$P$$, $$StoqMA$$-complete, or $$QMA$$-complete depending on the choice of $$K$$. Allowing for arbirary signs on the weights is often unhelpful, as we cannot distinguish between ferromagnetic and antiferromagnetic interactions (i.e. MinCut vs MaxCut). We define the local Hamiltonian problems restricted to the single interaction term $$K$$ with *positive weights* as the $$\{K^+\}^+$$-Hamiltonian problem. 

We may also restrict the problem further:  [[PM15]]({{site.baseurl}}/bib#PM15) considers the case where the local term $$K$$ is symmetric upon the interchange of its qubits. This means that $$K_{ij}=K_{ji}$$. This restriction is common in statistical mechanics (Heisenberg, Ising, XYZ models) and in computer science (MaxCut).  [[PM15]]({{site.baseurl}}/bib#PM15) then shows that in this case, the local term can always be diagonalized in the Bell basis, i.e. it can be written as

$$ K = \alpha' |\psi^+\rangle \langle\psi^+| +  \beta' |\phi^+\rangle \langle\phi^+| +  \gamma' |\phi^-\rangle \langle\phi^-| +  \delta' |\psi^-\rangle \langle\psi^-|$$

where 

$$|\psi^{\pm}\rangle = |01\rangle \pm |01\rangle, \quad |\phi^{\pm}\rangle = |00\rangle \pm |11\rangle,$$

are the canonical *Bell states.* We can always choose an identity shift by $$-\delta' I$$ to convert the local term to the form

$$ K = \alpha |\psi^+\rangle \langle\psi^+| +  \beta |\phi^+\rangle \langle\phi^+| +  \gamma |\phi^-\rangle \langle\phi^-|. $$

Shifting by the identity does not change the complexity of the Local Hamiltonian problem. Then, [[PM15]]({{site.baseurl}}/bib#PM15) and [[MS26]]({{site.baseurl}}/bib#MS26) further classifies the $$\{K^+\}^+$$-Hamiltonian problem for any $$\alpha, \, \beta, \, \gamma$$. The green region denotes that the problem is reducible to an augmented version of the EPR problem, which we call EPR*.

<style>
:root {
  --color-background-primary: #ffffff;
  --color-background-secondary: #f1efe8;
  --color-text-primary: #1a1a1a;
  --color-text-secondary: #5f5e5a;
  --color-border-tertiary: rgba(0,0,0,0.15);
  --color-border-secondary: rgba(0,0,0,0.3);
  --font-sans: system-ui, sans-serif;
}
@media (prefers-color-scheme: dark) {
  :root {
    --color-background-primary: #1e1e1c;
    --color-background-secondary: #2c2c2a;
    --color-text-primary: #e8e6de;
    --color-text-secondary: #9c9a92;
    --color-border-tertiary: rgba(255,255,255,0.15);
    --color-border-secondary: rgba(255,255,255,0.3);
  }
}
</style>

<div style="position:relative;width:100%;height:580px;overflow:hidden;border-radius:12px;border:0.5px solid var(--color-border-tertiary);background:var(--color-background-secondary)">

<canvas id="c3d" style="width:100%;height:100%;display:block;cursor:grab"></canvas>

<div style="position:absolute;top:10px;left:12px;display:flex;flex-direction:column;gap:6px;pointer-events:none">
  <div style="display:flex;align-items:center;gap:7px"><div style="width:12px;height:12px;border-radius:2px;background:#7F77DD;opacity:.85"></div><span style="font-size:12px;color:var(--color-text-primary);font-family:var(--font-sans)">QMA-complete</span></div>
  <div style="display:flex;align-items:center;gap:7px"><div style="width:12px;height:12px;border-radius:2px;background:#E8593C;opacity:.85"></div><span style="font-size:12px;color:var(--color-text-primary);font-family:var(--font-sans)">StoqMA-complete</span></div>
  <div style="display:flex;align-items:center;gap:7px"><div style="width:12px;height:12px;border-radius:2px;background:#F2A623;opacity:.85"></div><span style="font-size:12px;color:var(--color-text-primary);font-family:var(--font-sans)">NP-complete</span></div>
  <div style="display:flex;align-items:center;gap:7px"><div style="width:12px;height:12px;border-radius:2px;background:#1D9E75;opacity:.85"></div><span style="font-size:12px;color:var(--color-text-primary);font-family:var(--font-sans)">EPR / BPP (easy)</span></div>
</div>

<div style="position:absolute;top:10px;right:12px;display:flex;flex-direction:column;gap:6px">
  <div style="background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:8px;padding:8px 10px">
    <div style="font-size:11px;color:var(--color-text-secondary);font-family:var(--font-sans);margin-bottom:4px">Slice axis</div>
    <div style="display:flex;gap:4px">
      <button id="bax" onclick="setAxis('x')" style="font-size:11px;padding:3px 8px;border-radius:5px;border:0.5px solid var(--color-border-secondary);cursor:pointer;background:var(--color-background-secondary);color:var(--color-text-primary);font-family:var(--font-sans)">$$\alpha$$</button>
      <button id="bbx" onclick="setAxis('y')" style="font-size:11px;padding:3px 8px;border-radius:5px;border:0.5px solid var(--color-border-secondary);cursor:pointer;background:var(--color-background-secondary);color:var(--color-text-primary);font-family:var(--font-sans)">$$\beta$$</button>
      <button id="bcx" onclick="setAxis('z')" style="font-size:11px;padding:3px 8px;border-radius:5px;border:0.5px solid var(--color-border-secondary);cursor:pointer;background:var(--color-background-secondary);color:var(--color-text-primary);font-family:var(--font-sans)">$$\gamma$$</button>
    </div>
    <div style="margin-top:6px">
      <input type="range" id="sliceR" min="-100" max="100" value="0" style="width:100%" oninput="updateSlice(this.value)">
      <div style="font-size:11px;color:var(--color-text-secondary);font-family:var(--font-sans);text-align:center" id="sliceLabel">slice = 0.00</div>
    </div>
    <button onclick="toggleSlice()" id="sliceBtn" style="margin-top:4px;width:100%;font-size:11px;padding:3px 8px;border-radius:5px;border:0.5px solid var(--color-border-secondary);cursor:pointer;background:var(--color-background-secondary);color:var(--color-text-primary);font-family:var(--font-sans)">Show slice</button>
  </div>
  <div style="background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:8px;padding:8px 10px">
    <div style="font-size:11px;color:var(--color-text-secondary);font-family:var(--font-sans);margin-bottom:4px">Point size</div>
    <input type="range" id="sizeR" min="1" max="8" value="3" step="0.5" style="width:100%" oninput="updateSize(this.value)">
  </div>
  <div style="background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:8px;padding:8px 10px">
    <button onclick="resetCam()" style="width:100%;font-size:11px;padding:3px 8px;border-radius:5px;border:0.5px solid var(--color-border-secondary);cursor:pointer;background:var(--color-background-secondary);color:var(--color-text-primary);font-family:var(--font-sans)">Reset view</button>
  </div>
</div>

<div style="position:absolute;bottom:10px;left:50%;transform:translateX(-50%);font-size:11px;color:var(--color-text-secondary);font-family:var(--font-sans);pointer-events:none;text-align:center">drag to orbit · scroll to zoom · shift+drag to pan</div>

<div id="tooltip" style="position:absolute;display:none;background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:6px;padding:6px 10px;font-size:12px;font-family:var(--font-sans);color:var(--color-text-primary);pointer-events:none"></div>
</div>

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
const canvas = document.getElementById('c3d');
const W = canvas.parentElement.offsetWidth;
const H = canvas.parentElement.offsetHeight;

const renderer = new THREE.WebGLRenderer({canvas, antialias:true, alpha:true});
renderer.setPixelRatio(Math.min(devicePixelRatio,2));
renderer.setSize(W,H);
renderer.setClearColor(0x000000,0);

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(45, W/H, 0.01, 100);

const QMA  = new THREE.Color('#7F77DD');
const STOQ = new THREE.Color('#E8593C');
const NP_c = new THREE.Color('#F2A623');
const EPR  = new THREE.Color('#1D9E75');

function classify(a, b, g) {
  // sort descending so a >= b >= g (WLOG)
  const s = [a, b, g].sort((x,y) => y - x);
  a = s[0]; b = s[1]; g = s[2];
  if (g > 0)                          return 'QMA';
  if (g === 0 && a === b && b > 0)    return 'NP';
  if (g === 0 && a > b && b > 0)      return 'STOQ';
  if (g < 0 && b > 0)                 return 'STOQ';
  return 'EPR';
}

function classColor(cls) {
  if (cls==='QMA')  return QMA;
  if (cls==='STOQ') return STOQ;
  if (cls==='NP')   return NP_c;
  return EPR;
}

const N = 28;
const lo = -1, hi = 1;
const step = (hi - lo) / (N - 1);
const positions_arr = [];
const colors = [];
const points = [];

for (let i = 0; i < N; i++) {
  for (let j = 0; j < N; j++) {
    for (let k = 0; k < N; k++) {
      const x = lo + i * step;
      const y = lo + j * step;
      const z = lo + k * step;
      const cls = classify(x, y, z);
      positions_arr.push(x, y, z);
      const c = classColor(cls);
      colors.push(c.r, c.g, c.b);
      points.push({x, y, z, cls});
    }
  }
}

const geo = new THREE.BufferGeometry();
geo.setAttribute('position', new THREE.BufferAttribute(new Float32Array(positions_arr), 3));
geo.setAttribute('color',    new THREE.BufferAttribute(new Float32Array(colors), 3));

const mat = new THREE.PointsMaterial({
  size: 0.06, vertexColors: true, sizeAttenuation: true, transparent: true, opacity: 0.75
});
const cloud = new THREE.Points(geo, mat);
scene.add(cloud);

// Wireframe cube outline
const boxGeo = new THREE.BoxGeometry(2, 2, 2);
const boxEdges = new THREE.EdgesGeometry(boxGeo);
const boxLine = new THREE.LineSegments(boxEdges, new THREE.LineBasicMaterial({color: 0x888880, opacity: 0.35, transparent: true}));
scene.add(boxLine);

// Axis lines
const axMat = new THREE.LineBasicMaterial({vertexColors: true, opacity: 0.7, transparent: true});
const axPositions = new Float32Array([
  -1,0,0, 1,0,0,
   0,-1,0, 0,1,0,
   0,0,-1, 0,0,1
]);
const axColors = new Float32Array([
  1,0.3,0.3, 1,0.3,0.3,
  0.3,1,0.3, 0.3,1,0.3,
  0.3,0.6,1, 0.3,0.6,1
]);
const axGeo = new THREE.BufferGeometry();
axGeo.setAttribute('position', new THREE.BufferAttribute(axPositions, 3));
axGeo.setAttribute('color',    new THREE.BufferAttribute(axColors, 3));
scene.add(new THREE.LineSegments(axGeo, axMat));

// Floating axis labels
const axLabels = [];
function makeLabel(text, pos) {
  const div = document.createElement('div');
  div.textContent = text;
  div.style.cssText = 'position:absolute;font-size:13px;font-family:system-ui,sans-serif;color:var(--color-text-primary);pointer-events:none;font-weight:500';
  canvas.parentElement.appendChild(div);
  axLabels.push({div, pos3: new THREE.Vector3(...pos)});
}
makeLabel('α', [1.25, 0, 0]);
makeLabel('β', [0, 1.25, 0]);
makeLabel('γ', [0, 0, 1.25]);

// Slice state
let sliceAxis = 'z';
let sliceVal = 0;
let showSlice = false;
let sliceMesh = null;

function setAxis(ax) {
  sliceAxis = ax;
  document.getElementById('bax').style.fontWeight = ax==='x' ? '600' : '400';
  document.getElementById('bbx').style.fontWeight = ax==='y' ? '600' : '400';
  document.getElementById('bcx').style.fontWeight = ax==='z' ? '600' : '400';
  if (showSlice) buildSlice();
}
setAxis('z');

function updateSlice(v) {
  sliceVal = parseFloat(v) / 100;
  document.getElementById('sliceLabel').textContent = 'slice = ' + sliceVal.toFixed(2);
  if (showSlice) buildSlice();
}

function toggleSlice() {
  showSlice = !showSlice;
  document.getElementById('sliceBtn').textContent = showSlice ? 'Hide slice' : 'Show slice';
  if (showSlice) buildSlice();
  else {
    if (sliceMesh) { scene.remove(sliceMesh); sliceMesh = null; }
    cloud.visible = true;
    boxLine.visible = true;
  }
}

function buildSlice() {
  if (sliceMesh) { scene.remove(sliceMesh); sliceMesh = null; }
  const eps = step * 0.75;
  const sp = [], sc = [];
  points.forEach(pt => {
    const v = sliceAxis==='x' ? pt.x : sliceAxis==='y' ? pt.y : pt.z;
    if (Math.abs(v - sliceVal) > eps) return;
    sp.push(pt.x, pt.y, pt.z);
    const c = classColor(pt.cls);
    sc.push(c.r, c.g, c.b);
  });
  const sg = new THREE.BufferGeometry();
  sg.setAttribute('position', new THREE.BufferAttribute(new Float32Array(sp), 3));
  sg.setAttribute('color',    new THREE.BufferAttribute(new Float32Array(sc), 3));
  sliceMesh = new THREE.Points(sg, new THREE.PointsMaterial({
    size: 0.1, vertexColors: true, sizeAttenuation: true, transparent: true, opacity: 1.0
  }));
  scene.add(sliceMesh);
  cloud.visible = false;
  boxLine.visible = true;
}

function updateSize(v) {
  mat.size = parseFloat(v) * 0.02;
  if (sliceMesh) sliceMesh.material.size = parseFloat(v) * 0.035;
}

// Camera orbit
let drag = false, lastX = 0, lastY = 0;
let theta = Math.PI * 0.35, phi = 0.38, radius = 4.5, panX = 0, panY = 0;

const el = canvas.parentElement;
el.addEventListener('mousedown', e => { drag=true; lastX=e.clientX; lastY=e.clientY; el.style.cursor='grabbing'; });
el.addEventListener('mouseup',   () => { drag=false; el.style.cursor='grab'; });
el.addEventListener('mouseleave',() => { drag=false; el.style.cursor='grab'; });
el.addEventListener('mousemove', e => {
  if (!drag) return;
  const dx = e.clientX - lastX, dy = e.clientY - lastY;
  if (e.shiftKey) { panX -= dx*0.004; panY += dy*0.004; }
  else { theta -= dx*0.012; phi = Math.max(-1.4, Math.min(1.4, phi + dy*0.012)); }
  lastX = e.clientX; lastY = e.clientY;
});
el.addEventListener('wheel', e => {
  e.preventDefault();
  radius = Math.max(1.5, Math.min(9, radius + e.deltaY*0.005));
}, {passive:false});

el.addEventListener('touchstart', e => { if(e.touches.length===1){ drag=true; lastX=e.touches[0].clientX; lastY=e.touches[0].clientY; }});
el.addEventListener('touchend',   () => drag=false);
el.addEventListener('touchmove',  e => {
  if (!drag || e.touches.length!==1) return;
  const dx = e.touches[0].clientX - lastX, dy = e.touches[0].clientY - lastY;
  theta -= dx*0.012; phi = Math.max(-1.4, Math.min(1.4, phi + dy*0.012));
  lastX = e.touches[0].clientX; lastY = e.touches[0].clientY;
}, {passive:true});

function resetCam() { theta=Math.PI*0.35; phi=0.38; radius=4.5; panX=0; panY=0; }

// Tooltip
const raycaster = new THREE.Raycaster();
raycaster.params.Points.threshold = 0.05;
const mouse = new THREE.Vector2();
const tooltip = document.getElementById('tooltip');

el.addEventListener('mousemove', e => {
  const rect = el.getBoundingClientRect();
  mouse.x = ((e.clientX - rect.left) / W) * 2 - 1;
  mouse.y = -((e.clientY - rect.top) / H) * 2 + 1;
  raycaster.setFromCamera(mouse, camera);
  const target = showSlice && sliceMesh ? sliceMesh : cloud;
  const hits = raycaster.intersectObject(target);
  if (hits.length > 0) {
    const idx = hits[0].index;
    const pt = showSlice ? (() => {
      // reconstruct from slice geometry
      const pos = target.geometry.attributes.position;
      return {x: pos.getX(idx), y: pos.getY(idx), z: pos.getZ(idx),
              cls: classify(pos.getX(idx), pos.getY(idx), pos.getZ(idx))};
    })() : points[idx];
    tooltip.style.display = 'block';
    tooltip.style.left = (e.clientX - rect.left + 12) + 'px';
    tooltip.style.top  = (e.clientY - rect.top  - 30) + 'px';
    const label = pt.cls==='STOQ'?'StoqMA-complete':pt.cls==='NP'?'NP-complete':pt.cls==='QMA'?'QMA-complete':'EPR';
    tooltip.innerHTML = 'x='+pt.x.toFixed(2)+', y='+pt.y.toFixed(2)+', z='+pt.z.toFixed(2)+'<br><b>'+label+'</b>';
  } else tooltip.style.display = 'none';
});

const projV = new THREE.Vector3();
function projectToScreen(v3) {
  projV.copy(v3).project(camera);
  return { x: (projV.x*0.5+0.5)*W, y: (-projV.y*0.5+0.5)*H };
}

function animate() {
  requestAnimationFrame(animate);
  const cx = Math.cos(phi)*Math.cos(theta)*radius + panX;
  const cy = Math.sin(phi)*radius + panY;
  const cz = Math.cos(phi)*Math.sin(theta)*radius;
  camera.position.set(cx, cy, cz);
  camera.lookAt(panX, panY, 0);
  axLabels.forEach(({div, pos3}) => {
    const s = projectToScreen(pos3);
    div.style.left = (s.x - 8) + 'px';
    div.style.top  = (s.y - 8) + 'px';
  });
  renderer.render(scene, camera);
}
animate();
</script>
{% endraw %}
