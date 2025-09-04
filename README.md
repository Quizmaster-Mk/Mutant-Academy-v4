<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Mutant Academy — Danger Room (V2.1)</title>
<style>
:root{
  --bg:#04060a; --card:#071022; --muted:#9fb0bd; --accent:#6ee7b7; --accent2:#7cc1ff;
  --danger:#ff6b6b; --gold:#ffd166; --text:#e9fbf6;
}
*{box-sizing:border-box}
html,body{height:100%}
body{
  margin:0;font-family:Inter,system-ui,Segoe UI,Arial;background:
  radial-gradient(1200px 600px at 10% 10%, rgba(110,231,183,0.03), transparent 6%),
  radial-gradient(800px 400px at 90% 90%, rgba(124,193,255,0.02), transparent 6%),
  linear-gradient(180deg,var(--bg),#00111a 60%);
  color:var(--text); display:flex; align-items:flex-start; justify-content:center; padding:16px;
  transition:background .25s ease;
}
.app{
  width:100%; max-width:760px; background:linear-gradient(180deg, rgba(255,255,255,0.012), rgba(255,255,255,0.01));
  border:1px solid rgba(255,255,255,0.04); border-radius:14px; padding:14px; box-shadow:0 18px 50px rgba(0,0,0,0.6);
  overflow:hidden;
}
.header{display:flex;justify-content:space-between;align-items:center}
.title{display:flex;gap:12px;align-items:center}
.logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent2));display:flex;align-items:center;justify-content:center;font-weight:900;color:#02121a;font-size:20px}
.h1{font-size:18px;margin:0}
.hint{color:var(--muted);font-size:13px}

.grid{display:grid;grid-template-columns:1fr 360px;gap:12px;margin-top:12px}
@media(max-width:900px){ .grid{grid-template-columns:1fr} .rightCol{order:2} }

.card{background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.006)); border-radius:10px;padding:12px;border:1px solid rgba(255,255,255,0.03)}
.powerList{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.btn{background:#081021;border:1px solid rgba(255,255,255,0.04);color:var(--text);padding:10px;border-radius:8px;font-weight:700;cursor:pointer}
.btn:disabled{opacity:.5;cursor:not-allowed}
.row{display:flex;gap:8px;align-items:center}
.col{flex:1}
.stat{font-size:13px;color:var(--muted)}
.avatar{width:78px;height:78px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));display:flex;align-items:center;justify-content:center;font-size:38px;transition:transform .24s ease}
.hpbar{height:12px;background:#071219;border-radius:8px;overflow:hidden;border:1px solid rgba(255,255,255,0.03)}
.fill{height:100%;transition:width .35s linear, background .3s linear;background:linear-gradient(90deg,#46d0b9,#1aa08c)}
.fill.enemy{background:linear-gradient(90deg,#ff7b7b,#ff4d4d)}
.log{height:260px;overflow:auto;padding:8px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);border-radius:8px;font-size:13px;color:var(--muted)}
.controls{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
.small{font-size:13px;color:var(--muted)}
.note{font-size:12px;color:var(--muted);margin-top:8px}
.floating{position:fixed;pointer-events:none;font-weight:900;text-shadow:0 8px 24px rgba(0,0,0,.6);animation:floatUp 900ms ease-out forwards}
@keyframes floatUp{from{opacity:1;transform:translate(-50%,0) scale(1)}to{opacity:0;transform:translate(-50%,-60px) scale(1.02)}}
.badge{display:inline-block;padding:6px 10px;border-radius:999px;background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.03);font-size:13px}
.footer{display:flex;justify-content:space-between;align-items:center;margin-top:12px}
.endTitle{font-size:18px;margin:0}
.center{display:flex;justify-content:center;align-items:center}
.hidden{display:none}
.actionBtn{flex:1;padding:10px;border-radius:8px;background:#08121a;border:1px solid rgba(255,255,255,0.04);cursor:pointer}
.actionBtn:disabled{opacity:.45;cursor:not-allowed}
.placeSmall{font-size:12px;color:var(--muted);margin-top:6px}

/* attack animation */
.attackAnim{position:fixed;padding:8px 10px;border-radius:8px;background:rgba(255,255,255,0.03);font-weight:900;z-index:9999;transform:translate(-50%,-50%);transition:transform .45s cubic-bezier(.2,.9,.2,1),opacity .45s}
@keyframes shake {
  0%{ transform: translateX(0) }
  25%{ transform: translateX(-6px) }
  50%{ transform: translateX(6px) }
  75%{ transform: translateX(-3px) }
  100%{ transform: translateX(0) }
}
.shake{animation:shake .45s linear}

/* glow on big hit */
.glow{
  box-shadow: 0 0 18px rgba(255,120,120,0.18), inset 0 0 12px rgba(255,255,255,0.02);
  transition: box-shadow .25s ease;
}

/* small fade for log entries */
.log .small{opacity:.95;transition:opacity .2s}
</style>
</head>
<body>
<div class="app" role="application" aria-label="Mutant Academy Danger Room v2.1">
  <div class="header">
    <div class="title">
      <div class="logo">MA</div>
      <div>
        <h1 class="h1">Mutant Academy — Danger Room</h1>
        <div class="hint">Nouveau : animations d’attaque & transitions. Choisis bien ton pouvoir.</div>
      </div>
    </div>
    <div class="badge">v2.1 — animations</div>
  </div>

  <div class="grid">
    <!-- LEFT: main flow -->
    <div>
      <div class="card" id="startCard">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <div class="small">1 • Choisis ton pouvoir</div>
            <div style="margin-top:8px" class="small">Chaque pouvoir est <b>usage unique</b> durant l’entraînement + combat final.</div>
          </div>
          <div class="badge" id="chosenBadge">Aucun</div>
        </div>

        <div style="margin-top:10px" class="powerList" id="powerList"></div>

        <hr style="border:none;border-top:1px solid rgba(255,255,255,0.03);margin:12px 0">

        <div style="display:flex;gap:8px;align-items:center">
          <div class="small">2 • Choisis la difficulté</div>
          <select id="difficultySelect" class="badge" style="margin-left:auto">
            <option value="facile">😃 Facile</option>
            <option value="normal" selected>😐 Normal</option>
            <option value="difficile">😈 Difficile</option>
          </select>
        </div>

        <div style="margin-top:12px" class="controls">
          <button id="btnStart" class="btn" disabled>▶️ Démarrer la session</button>
          <button id="btnHelp" class="btn" onclick="showHelp()">❔ Aide</button>
        </div>
        <div class="note">Version V2.1 — Danger Room animé. PV et aléa dépendant de la difficulté.</div>
      </div>

      <div id="gameCard" class="card hidden" style="margin-top:12px">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <div class="small">Épreuve</div>
            <div id="stageTitle" style="font-weight:800;margin-top:6px">—</div>
          </div>
          <div class="badge" id="sessionBadge">Score: 0</div>
        </div>

        <div id="challengeArea" style="margin-top:12px"></div>

        <div id="actionArea" style="margin-top:12px" class="controls"></div>

        <div style="margin-top:12px" class="small">Historique</div>
        <div id="log" class="log" aria-live="polite"></div>
      </div>

      <div id="endCard" class="card hidden" style="margin-top:12px;text-align:center">
        <h2 id="endTitle" class="endTitle">—</h2>
        <p id="endText" class="small"></p>
        <div style="margin-top:10px"><button class="btn" onclick="location.reload()">🔁 Rejouer</button></div>
      </div>
    </div>

    <!-- RIGHT: player & enemy -->
    <div class="rightCol">
      <div class="card">
        <div style="display:flex;gap:8px;align-items:center">
          <div>
            <div class="avatar" id="playerAvatar">🦸</div>
          </div>
          <div style="flex:1">
            <div style="display:flex;justify-content:space-between;align-items:center">
              <div><strong id="playerName">Élève</strong></div>
              <div class="stat">PV: <span id="playerPV">0</span>/<span id="playerMax">0</span></div>
            </div>
            <div class="hpbar" style="margin-top:8px"><div id="playerFill" class="fill" style="width:100%"></div></div>
            <div class="placeSmall" id="powerNote">Pouvoir: —</div>
          </div>
        </div>
      </div>

      <div id="enemyCard" class="card hidden" style="margin-top:12px">
        <div style="display:flex;gap:8px;align-items:center">
          <div class="avatar" id="enemyAvatar">🤖</div>
          <div style="flex:1">
            <div style="display:flex;justify-content:space-between;align-items:center">
              <div><strong id="enemyName">Ennemi</strong></div>
              <div class="stat">PV: <span id="enemyPV">0</span>/<span id="enemyMax">0</span></div>
            </div>
            <div class="hpbar" style="margin-top:8px"><div id="enemyFill" class="fill enemy" style="width:0%"></div></div>
            <div class="placeSmall" id="enemyNote">—</div>
          </div>
        </div>
      </div>

      <div class="card" style="margin-top:12px">
        <div class="small">Raccourcis</div>
        <div style="margin-top:8px;display:flex;gap:8px">
          <button class="btn" id="btnUsePower" disabled>✨ Utiliser pouvoir</button>
          <button class="btn" id="btnStatus" onclick="showStatus()">📋 Statut</button>
        </div>
        <div class="note" style="margin-top:8px">Astuce : utilise le pouvoir au moment décisif pour un bonus stratégique.</div>
      </div>
    </div>
  </div>

</div>

<script>
/* ========== Data & State (identique à V2) ========== */
const POWERS = {
  telekinesis:{id:'telekinesis',name:'Télékinésie',emoji:'🧠',desc:'Augmente l’esquive ennemie (30%) 1 tour',usage:false},
  claws:{id:'claws',name:'Griffes',emoji:'🗡️',desc:'Prochaine attaque inflige +6 dégâts',usage:false},
  speed:{id:'speed',name:'Vitesse',emoji:'⚡',desc:'Donne 1 action supplémentaire ce tour',usage:false},
  ice:{id:'ice',name:'Glace',emoji:'❄️',desc:'Gèle l’ennemi (rate sa riposte) 1 tour',usage:false},
  pyro:{id:'pyro',name:'Pyro',emoji:'🔥',desc:'Applique brûlure (2 tours, 3 dégâts/turn)',usage:false}
};

const VILLAINS = [
  {id:'magneto',name:'Magneto',emoji:'🧲',pv:36,dmg:[3,7],note:'Réduit l’efficacité des armes (aléa)'},
  {id:'mystique',name:'Mystique',emoji:'🌀',pv:28,dmg:[2,6],note:'Peut imiter une action aléatoire'},
  {id:'sentinel',name:'sentinel',emoji:'🤖',pv:44,dmg:[4,9],note:'Attaques lourdes mais lentes'}
];

let state = {
  difficulty:'normal',
  chosenPower:null,
  player:{pv:0,max:0,avatar:'🦸',name:'Novice',powerAvailable:false,clawsBoost:false,extraAction:false,evade:0},
  enemy:null,
  score:0,
  stage:0,
  challenges:[],
  trainingRounds:0
};

/* ========== Elements ========== */
const powerListEl = document.getElementById('powerList');
const btnStart = document.getElementById('btnStart');
const difficultySelect = document.getElementById('difficultySelect');
const chosenBadge = document.getElementById('chosenBadge');
const sessionBadge = document.getElementById('sessionBadge');
const challengeArea = document.getElementById('challengeArea');
const actionArea = document.getElementById('actionArea');
const logEl = document.getElementById('log');
const playerAvatar = document.getElementById('playerAvatar');
const enemyCard = document.getElementById('enemyCard');
const enemyAvatar = document.getElementById('enemyAvatar');

/* ========== Helpers (animations) ========== */
function log(msg){
  const d=document.createElement('div'); d.className='small'; d.innerHTML=msg; logEl.appendChild(d); logEl.scrollTop=logEl.scrollHeight;
}
function spawnFloatingOver(el,txt,color='white'){
  if(!el) return;
  const r = el.getBoundingClientRect();
  const f = document.createElement('div'); f.className='floating'; f.textContent=txt; f.style.left=(r.left+r.width/2)+'px'; f.style.top=(r.top+8)+'px'; f.style.color=color;
  document.body.appendChild(f); setTimeout(()=>f.remove(),900);
}
function animateAttack(fromEl,toEl,icon,cb){
  if(!fromEl || !toEl){ if(cb) cb(); return; }
  const r1 = fromEl.getBoundingClientRect();
  const r2 = toEl.getBoundingClientRect();
  const anim = document.createElement('div'); anim.className='attackAnim'; anim.textContent = icon; anim.style.left = (r1.left + r1.width/2)+'px'; anim.style.top = (r1.top + r1.height/2)+'px';
  anim.style.opacity = 1; anim.style.transform = 'translate(-50%,-50%) scale(1.0)';
  document.body.appendChild(anim);
  // move to target
  requestAnimationFrame(()=>{
    anim.style.transform = `translate(${r2.left + r2.width/2 - (r1.left + r1.width/2)}px, ${r2.top + r2.height/2 - (r1.top + r1.height/2)}px) scale(1.0)`;
    anim.style.opacity = 0.95;
  });
  setTimeout(()=>{
    // small flash on target
    toEl.classList.add('glow');
    setTimeout(()=>toEl.classList.remove('glow'),220);
    anim.remove();
    if(cb) cb();
  },420);
}
function screenShake(){
  const app = document.querySelector('.app');
  app.classList.add('shake');
  setTimeout(()=>app.classList.remove('shake'),420);
}

/* ========== UI updates ========== */
function updatePlayerUI(){
  document.getElementById('playerPV').textContent = state.player.pv;
  document.getElementById('playerMax').textContent = state.player.max;
  document.getElementById('playerFill').style.width = Math.max(0,(state.player.pv/state.player.max*100))+'%';
  playerAvatar.textContent = state.player.avatar;
  document.getElementById('chosenBadge').textContent = state.chosenPower ? (state.chosenPower.emoji+' '+state.chosenPower.name) : 'Aucun';
  document.getElementById('powerNote').textContent = state.chosenPower ? `${state.chosenPower.desc}` : 'Pouvoir : —';
  sessionBadge.textContent = 'Score: '+state.score;
  document.getElementById('btnUsePower').disabled = !state.player.powerAvailable;
}
function updateEnemyUI(){
  if(!state.enemy) { enemyCard.classList.add('hidden'); return; }
  enemyCard.classList.remove('hidden');
  document.getElementById('enemyName').textContent = state.enemy.emoji+' '+state.enemy.name;
  document.getElementById('enemyPV').textContent = state.enemy.pv;
  document.getElementById('enemyMax').textContent = state.enemy.max;
  document.getElementById('enemyFill').style.width = Math.max(0,(state.enemy.pv/state.enemy.max*100))+'%';
  document.getElementById('enemyNote').textContent = state.enemy.note || '';
  enemyAvatar.textContent = state.enemy.emoji;
}

/* ========== Build power buttons ========== */
function buildPowerButtons(){
  powerListEl.innerHTML='';
  Object.values(POWERS).forEach(p=>{
    const btn = document.createElement('button');
    btn.className='btn';
    btn.innerHTML = `<div style="text-align:left"><strong>${p.emoji} ${p.name}</strong><div class="small" style="margin-top:6px">${p.desc}</div></div>`;
    btn.onclick = ()=>selectPower(p.id, btn);
    powerListEl.appendChild(btn);
  });
}
buildPowerButtons();

/* ========== Selection & start ========== */
function selectPower(id, btn){
  state.chosenPower = POWERS[id];
  state.player.avatar = state.chosenPower.emoji;
  state.player.powerAvailable = true;
  Array.from(powerListEl.children).forEach(b=>b.style.opacity=.45);
  btn.style.opacity=1;
  btnStart.disabled = false;
  updatePlayerUI();
  log(`✨ Pouvoir choisi : ${state.chosenPower.name}`);
}
btnStart.onclick = ()=>{
  if(!state.chosenPower) return alert('Choisis un pouvoir avant de démarrer.');
  state.difficulty = difficultySelect.value;
  beginSession();
};

/* ========== Session flow (same as V2 but with animations) ========== */
function beginSession(){
  const basePV = state.difficulty==='facile'?44:(state.difficulty==='difficile'?30:36);
  state.player.pv = basePV; state.player.max = basePV;
  state.score = 0; state.stage = 0; state.challenges = [];
  const pool = ['combat','rescue','puzzle'];
  for(let i=0;i<3;i++) state.challenges.push(pool[Math.floor(Math.random()*pool.length)]);
  state.player.clawsBoost = false; state.player.extraAction=false; state.player.evade=0; state.player.powerAvailable=true; state.chosenPower.usage=false;
  document.getElementById('startCard').classList.add('hidden');
  document.getElementById('gameCard').classList.remove('hidden');
  logEl.innerHTML=''; log('🔰 Session débutée — Danger Room active');
  nextChallenge();
  updatePlayerUI(); updateEnemyUI();
}

/* Challenge dispatch */
function nextChallenge(){
  if(state.stage < 3){
    const ch = state.challenges[state.stage];
    document.getElementById('stageTitle').textContent = `Entraînement — épreuve ${state.stage+1}/3 : ${capitalize(ch)}`;
    if(ch === 'combat') startTrainingCombat();
    else if(ch === 'rescue') startTrainingRescue();
    else startTrainingPuzzle();
  } else {
    prepareFinal();
  }
}

/* Training Combat (with animation) */
function startTrainingCombat(){
  const botPV = state.difficulty==='facile'?12:(state.difficulty==='difficile'?18:14);
  state.enemy = {name:'Robot d’entraînement',emoji:'🤖',pv:botPV,max:botPV,dmg: state.difficulty==='difficile'? [3,6]:[2,4],note:'Bot — simple IA',frozen:false,burn:0};
  updateEnemyUI();
  log('🤖 Épreuve Combat : neutralise le robot.');
  renderCombat(true);
}

/* Training Rescue */
function startTrainingRescue(){
  state.enemy = null; updateEnemyUI();
  const correct = Math.floor(Math.random()*3)+1;
  state._rescue = {correct};
  challengeArea.innerHTML = `<div class="small">🚪 Sauvetage : un otage est derrière une des 3 portes. Choisis une porte.</div>`;
  actionArea.innerHTML = '';
  for(let i=1;i<=3;i++){
    const b = document.createElement('button'); b.className='actionBtn'; b.textContent='Porte '+i; b.onclick = ()=>resolveRescue(i);
    actionArea.appendChild(b);
  }
  log('🚪 Épreuve Sauvetage : choisis la bonne porte.');
}

/* Training Puzzle (QCM) */
const QUIZZES = {
  facile:[{q:'Quelle tactique pour esquiver ?',choices:['Attaque directe','Esquive latérale','Crier'],a:1}],
  normal:[
    {q:'Meilleure option pour neutraliser plusieurs cibles ?',choices:['Fuir','Zone d’effet','Attaque précise'],a:1},
    {q:'Priorité en embuscade ?',choices:['Frapper fort','Analyser','Rester immobile'],a:1}
  ],
  difficile:[
    {q:'Comment contrer un ennemi blindé ?',choices:['Attaquer source énergie','Coup aléatoire','Reculer'],a:0},
    {q:'Mouvement utile contre tir automatique ?',choices:['Rester','Se couvrir+bouger','Crier'],a:1}
  ]
};
function startTrainingPuzzle(){
  const pool = state.difficulty==='facile'?QUIZZES.facile:(state.difficulty==='difficile'?QUIZZES.difficile:QUIZZES.normal);
  const q = pool[Math.floor(Math.random()*pool.length)];
  state._quiz = q;
  challengeArea.innerHTML = `<div class="small"><b>❓ ${q.q}</b></div>`;
  actionArea.innerHTML = '';
  q.choices.forEach((c,i)=>{
    const b = document.createElement('button'); b.className='actionBtn'; b.textContent=c; b.onclick = ()=>resolveQuiz(i);
    actionArea.appendChild(b);
  });
  log('🧠 Épreuve Stratégie : réponds au QCM.');
}

/* Resolvers */
function resolveRescue(choice){
  actionArea.innerHTML='';
  if(choice === state._rescue.correct){
    log('🟢 Sauvetage réussi — otage sécurisé.');
    spawnFloatingOver(playerAvatar,'+10','lightgreen'); state.score += 10;
  } else {
    log('🔴 Mauvaise porte — simulation piège : légère blessure.');
    spawnFloatingOver(playerAvatar,'-6','red'); state.player.pv -= 6; state.score -= 2; if(state.player.pv<0) state.player.pv=0; updatePlayerUI();
  }
  state.stage++; setTimeout(()=>nextChallenge(),700);
}

function resolveQuiz(choice){
  actionArea.innerHTML='';
  const q = state._quiz;
  if(choice === q.a){
    log('🟢 Bonne réponse — tactique améliorée.'); state.score += 6;
  } else {
    log('🔴 Mauvaise réponse — perte de temps, légère fatigue.'); state.player.pv -= 4; if(state.player.pv<0) state.player.pv=0; state.score -= 1; updatePlayerUI();
  }
  state.stage++; setTimeout(()=>nextChallenge(),700);
}

/* Combat render */
function renderCombat(isTraining){
  updatePlayerUI(); updateEnemyUI();
  challengeArea.innerHTML = `<div class="small"><b>${state.enemy.emoji} ${state.enemy.name}</b></div><div class="small" id="enemyPVtxt">PV: ${state.enemy.pv}/${state.enemy.max}</div>`;
  actionArea.innerHTML = '';
  const a1 = document.createElement('button'); a1.className='actionBtn'; a1.textContent='⚔️ Coup léger (2-4)'; a1.onclick = ()=>playerAttack('light');
  const a2 = document.createElement('button'); a2.className='actionBtn'; a2.textContent='💥 Coup fort (4-7)'; a2.onclick = ()=>playerAttack('heavy');
  const a3 = document.createElement('button'); a3.className='actionBtn'; a3.textContent='🛡️ Défendre (réduit dégâts)'; a3.onclick = ()=>playerDefend();
  actionArea.appendChild(a1); actionArea.appendChild(a2); actionArea.appendChild(a3);
  const useBtn = document.getElementById('btnUsePower');
  useBtn.disabled = !state.player.powerAvailable;
  useBtn.onclick = ()=>{ usePower(); document.getElementById('btnUsePower').disabled=true; };
}

/* Player actions with animations */
function playerAttack(kind){
  let dmg = kind==='light'? rand(2,4): rand(4,7);
  if(state.player.clawsBoost){ dmg += 6; state.player.clawsBoost=false; log('🗡️ Griffes : attaque renforcée (+6)'); }
  if(state.enemy && Math.random()<0.08 && state.difficulty==='difficile'){ dmg = Math.max(0,dmg-2); log('🔰 Résilience adverse réduit les dégâts.'); }
  if(state.enemy){
    // animate from player avatar to enemy fill
    animateAttack(document.getElementById('playerAvatar'), document.getElementById('enemyFill'), state.player.avatar, ()=>{
      state.enemy.pv -= dmg; if(state.enemy.pv<0) state.enemy.pv=0;
      updateEnemyUI();
      log(`🦸 Tu infliges ${dmg} PV à ${state.enemy.name}`);
      spawnFloatingOver(document.getElementById('enemyFill'),`-${dmg}`,'#ffcccc');
      // small screen shake if big dmg
      if(dmg>=8) screenShake();
      // scoring
      if(state.enemy.name.includes('Robot')) state.score += 2;
      if(state.player.extraAction){
        state.player.extraAction = false; log('🔁 Action supplémentaire utilisée (pas de riposte immédiate).'); updatePlayerUI(); return;
      }
      setTimeout(()=>enemyReact(),420);
    });
  } else {
    log('ℹ️ Pas d’ennemi présent.');
  }
}

/* defend */
function playerDefend(){
  state.player.evade = Math.min(0.6, (state.player.evade||0) + 0.25);
  log('🛡️ Tu te protèges — prochaine attaque réduite.');
  setTimeout(()=>enemyReact(),300);
}

/* use power (with animation) */
function usePower(){
  if(!state.player.powerAvailable || !state.chosenPower) return;
  const p = state.chosenPower;
  log(`✨ Tu actives ${p.emoji} ${p.name} — ${p.desc}`);
  // show small effect animation toward enemy (if exists)
  const toEl = state.enemy ? document.getElementById('enemyFill') : document.getElementById('playerFill');
  animateAttack(document.getElementById('playerAvatar'), toEl, p.emoji, ()=>{
    if(p.id==='telekinesis'){ state.player.evade += 0.3; log('🔮 Télékinésie : evade +30% pour une attaque ennemie.'); }
    else if(p.id==='claws'){ state.player.clawsBoost = true; log('🗡️ Griffes activées : prochaine attaque +6.'); }
    else if(p.id==='speed'){ state.player.extraAction = true; log('⚡ Vitesse : action supplémentaire disponible.'); }
    else if(p.id==='ice'){ if(state.enemy) { state.enemy.frozen = true; log('❄️ Glace : l’ennemi sera gelé une fois.'); } }
    else if(p.id==='pyro'){ if(state.enemy) { state.enemy.burn = 2; log('🔥 Pyro : brûlure 2 tours infligée.'); } }
    state.player.powerAvailable = false; state.chosenPower.usage = true; updatePlayerUI();
    // if extraAction not set, enemy reacts
    if(!state.player.extraAction) setTimeout(()=>enemyReact(),420);
  });
}

/* enemy react with animation */
function enemyReact(){
  if(!state.enemy){
    log('ℹ️ Aucun ennemi — épreuve terminée.');
    state.stage++; setTimeout(()=>nextChallenge(),600);
    return;
  }
  if(state.enemy.frozen){
    log(`❄️ ${state.enemy.name} est gelé et rate son action !`);
    state.enemy.frozen = false;
    if(state.enemy.burn && state.enemy.burn>0){ state.enemy.pv -= 3; state.enemy.burn--; log(`🔥 Brûlure : ${state.enemy.name} subit 3 PV`); spawnFloatingOver(document.getElementById('enemyFill'),'-3','#ffb86b'); updateEnemyUI();}
    checkCombatEnd(); return;
  }
  // enemy attack animation
  const dmgBase = rand(state.enemy.dmg[0], state.enemy.dmg[1]);
  const evade = state.player.evade || 0;
  const reduced = Math.round(dmgBase * evade);
  const final = Math.max(0, dmgBase - reduced);
  animateAttack(document.getElementById('enemyAvatar'), document.getElementById('playerFill'), state.enemy.emoji, ()=>{
    state.player.pv -= final; if(state.player.pv<0) state.player.pv=0;
    log(`💥 ${state.enemy.name} riposte : ${final} PV infligés.`);
    spawnFloatingOver(document.getElementById('playerFill'),`-${final}`,'#ff9aa2');
    state.player.evade = 0;
    if(state.enemy.burn && state.enemy.burn>0){ state.enemy.pv -= 3; state.enemy.burn--; log(`🔥 Brûlure : ${state.enemy.name} subit 3 PV`); spawnFloatingOver(document.getElementById('enemyFill'),'-3','#ffb86b'); updateEnemyUI();}
    updatePlayerUI(); checkCombatEnd();
    // screen shake on heavy hit
    if(final >= 6) screenShake();
  });
}

/* check combat end */
function checkCombatEnd(){
  if(state.enemy && state.enemy.pv <= 0){
    log(`🏁 ${state.enemy.name} neutralisé !`);
    state.score += (state.enemy.name.includes('Robot')? 8 : 20);
    state.enemy = null; updateEnemyUI();
    state.stage++; setTimeout(()=>nextChallenge(),700);
    return;
  }
  if(state.player.pv <= 0){
    log('💀 Tu es K.O. — session interrompue.');
    conclude(false);
  }
}

/* prepare final */
function prepareFinal(){
  document.getElementById('stageTitle').textContent = 'Combat final — Simulateur';
  const v = VILLAINS[Math.floor(Math.random()*VILLAINS.length)];
  const mul = state.difficulty==='difficile'?1.15:(state.difficulty==='facile'?0.9:1);
  state.enemy = {name:v.name,emoji:v.emoji,pv:Math.round(v.pv * mul),max:Math.round(v.pv * mul),dmg:v.dmg,note:v.note,frozen:false,burn:0};
  updateEnemyUI();
  log(`⚠️ Combat final : ${v.name} simulé — ${v.note}`);
  renderCombat(false);
}

/* conclude */
function conclude(victory){
  document.getElementById('gameCard').classList.add('hidden');
  document.getElementById('endCard').classList.remove('hidden');
  let title='', text='';
  if(!victory){
    title='💥 Simulation échouée';
    text=`Tu as été dépassé par le simulateur. Score final : ${state.score}. Recommence l'entraînement.`;
  } else {
    if(state.score >= 60){ title='🏆 Admis avec mention'; text=`Excellent — tu rejoins les X-Men comme membre prometteur.<br>Score: ${state.score}`; }
    else if(state.score >= 35){ title='✅ Admis à l’Académie'; text=`Bon travail — admis pour formation avancée.<br>Score: ${state.score}`; }
    else if(state.score >= 15){ title='⚠️ Admis en formation'; text=`Tu as réussi mais tu dois t’entraîner davantage.<br>Score: ${state.score}`; }
    else { title='🔰 En formation (recalé temporaire)'; text=`Tu n’as pas atteint le niveau requis — réessaie.<br>Score: ${state.score}`; }
  }
  document.getElementById('endTitle').textContent = title;
  document.getElementById('endText').innerHTML = text;
}

/* utility */
function rand(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }
function capitalize(s){ return s.charAt(0).toUpperCase()+s.slice(1); }
function showHelp(){ alert('Mutant Academy — Danger Room\\n\\n1) Choisis ton pouvoir (usage unique).\\n2) Choisis la difficulté.\\n3) Passe 3 épreuves aléatoires (combat, sauvetage, puzzle).\\n4) Affronte le simulateur final.\\n\\nUtilise ton pouvoir au moment stratégique.'); }
function showStatus(){ alert(`PV: ${state.player.pv}/${state.player.max}\\nScore: ${state.score}\\nPouvoir: ${state.chosenPower?state.chosenPower.name:'—'}`); }

/* init */
document.getElementById('btnUsePower').onclick = ()=>{ usePower(); document.getElementById('btnUsePower').disabled=true; };
updatePlayerUI(); updateEnemyUI();
log('ℹ️ Chargement prêt — choisis ton pouvoir pour commencer.');
</script>
</body>
</html>
