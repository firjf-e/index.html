# index.html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>보상형 할일 게임 – 게스트 즉시 시작</title>
<style>
  :root{--bg:#0b1015;--panel:#111827;--line:#1f2937;--text:#e6edf3;--mut:#9ca3af;--pri:#2563eb}
  *{box-sizing:border-box}
  body{margin:0;font-family:system-ui,Segoe UI,Apple SD Gothic Neo,Roboto,Arial;background:var(--bg);color:var(--text)}
  header{position:sticky;top:0;z-index:10;display:flex;justify-content:space-between;align-items:center;padding:14px 16px;border-bottom:1px solid var(--line);background:#0f1620}
  .brand{font-weight:800}
  .wrap{max-width:1100px;margin:0 auto;padding:16px}
  .card{background:var(--panel);border:1px solid var(--line);border-radius:16px;padding:16px;box-shadow:0 10px 30px rgba(0,0,0,.25);margin-bottom:16px}
  .title{font-weight:800;margin-bottom:8px}
  .row{display:flex;gap:10px;flex-wrap:wrap}
  .grid{display:grid;gap:10px}
  .grid2{display:grid;grid-template-columns:1fr 1fr;gap:10px}
  input,button{border-radius:12px;border:1px solid var(--line);padding:10px 12px}
  input{background:#0b1220;color:var(--text)}
  button{background:var(--pri);color:white;border:0;cursor:pointer}
  button.ghost{background:transparent;color:#93c5fd;border:1px solid var(--line)}
  button.warn{background:#ef4444}
  .chip{display:inline-flex;gap:6px;align-items:center;padding:6px 10px;border-radius:999px;border:1px solid var(--line);background:#0b1220}
  .list{display:flex;flex-direction:column;gap:8px}
  .task{display:flex;justify-content:space-between;align-items:center;gap:10px;padding:10px;border-radius:12px;background:#0c1423;border:1px solid var(--line)}
  .foods{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
  .food{padding:10px;border-radius:12px;background:#0c1423;border:1px solid var(--line);display:flex;justify-content:space-between;align-items:center}
  .pill{font-size:12px;padding:4px 8px;border-radius:999px;border:1px solid var(--line);background:#0b1220}
  .mono{font-family:ui-monospace, SFMono-Regular, Menlo, monospace;white-space:pre-wrap;font-size:13px}
  .mut{color:#9ca3af}
  .hidden{display:none}
  .center{text-align:center}
</style>
</head>
<body>
<header>
  <div class="brand">✅ 보상형 할일 게임</div>
  <div class="row">
    <button id="exportBtn" class="ghost">내보내기</button>
    <label class="chip">가져오기<input type="file" id="importFile" accept="application/json" /></label>
    <button id="resetBtn" class="warn">초기화</button>
  </div>
</header>

<div class="wrap">

  <!-- 웰컴/튜토리얼 -->
  <section id="welcome" class="card">
    <div class="title">어서오세요!</div>
    <div class="grid">
      <div>로그인 없이 바로 시작. 끝나고 원하면 구글/네이버/이메일 로그인 훅 붙일 수 있음.</div>
      <div class="row">
        <button id="startBtn">시작</button>
      </div>
      <div class="mut mono">Tip: GitHub Pages는 루트에 <code>.nojekyll</code> 넣어. Pages 설정은 main/root.</div>
    </div>
  </section>

  <!-- 게임 -->
  <section id="game" class="hidden">
    <div class="card">
      <div class="title">프로필</div>
      <div class="row">
        <span class="chip">아바타: <b id="avatar">😐</b></span>
        <span class="chip">이름: <b id="name">신지성</b></span>
        <span class="chip">레벨: <b id="level">Vb.1</b></span>
        <span class="chip">코인: <b id="coins">0</b></span>
        <span class="chip">에메랄드: <b id="emeralds">0</b></span>
        <span class="chip">Aura: <b id="aura">0</b></span>
        <span class="chip">보상차단: <b id="blockDays">0</b>d</span>
        <span class="chip">정지: <b id="banDays">0</b>d</span>
      </div>
      <div class="row" style="margin-top:8px">
        <input id="nameInput" placeholder="이름 변경(엔터)" />
        <button id="tickBtn" class="ghost">하루 경과</button>
      </div>
    </div>

    <div class="card">
      <div class="title">할일</div>
      <div class="row">
        <input id="taskInput" placeholder='예) "11시에 잠들기" / "라면 2번 절대 먹지 말기"' />
        <button id="addTaskBtn">할일저장</button>
      </div>
      <div id="taskList" class="list" style="margin-top:10px"></div>
      <div class="mut">완료하면: 코인+10, Aura+10, **랜덤 음식 6종 세트** 생성. 실패: 기본 Aura-1(특수 규칙 있음).</div>
    </div>

    <div class="card">
      <div class="title">라면 위반 카운터</div>
      <div class="row">
        <button id="ramenPlus">라면 +1</button>
        <button id="ramenZero" class="ghost">카운터 초기화</button>
        <div class="mut">오늘 라면: <b id="ramenCount">0</b> / 2회 달성 시 Aura -3</div>
      </div>
    </div>

    <div class="card">
      <div class="title">보상 – 랜덤 음식 6종 세트</div>
      <div id="rewardBox" class="foods"></div>
    </div>

    <div class="card">
      <div class="title">🛒 쇼핑창</div>
      <div id="shop" class="foods"></div>
      <div id="recipeBox" class="mono" style="margin-top:8px"></div>
    </div>

    <div class="card">
      <div class="title">규칙 위반(철퇴) 테스트</div>
      <div class="row">
        <button id="cheatAura" class="warn">오라 무단 사용</button>
        <button id="cheatCoin" class="warn">코인 무단 조작</button>
        <button id="cheatTask" class="warn">할일 결과 조작</button>
      </div>
      <div class="mut">실서비스에선 숨겨도 됨. 여기선 동작 확인용.</div>
    </div>
  </section>

</div>

<script>
/* ========= 핵심 상태 ========= */
const AVATARS = ["😐","🤨","🤔","😙","😏","🤑"];
const LEVELS  = ["Vb.1","Vb.2","Vb.3","Vb.4","Vb.5","Vb.6"];
const AURA_THRESH = [0,50,120,210,320,450];

const FOODS = {
  "국류":[
    {name:"된장국", premium:false, recipe:["물+멸치육수","된장 풀기","두부/호박 넣고 끓이기"]},
    {name:"미역국", premium:false, recipe:["미역 불리기","참기름에 볶기","물+간장 살짝 끓이기"]},
    {name:"육개장", premium:true, recipe:["소고기/파 볶기","고춧가루/고사리","오래 끓이기"]}
  ],
  "고기류":[
    {name:"불고기", premium:false, recipe:["간장+설탕+마늘","소고기 재우기","센불 볶기"]},
    {name:"삼겹살", premium:false, recipe:["소금간","노릇하게 굽기","쌈장이랑"]},
    {name:"닭갈비", premium:true, recipe:["고추장 양념","닭/양배추 볶기","떡사리"]}
  ],
  "밥류":[
    {name:"김치볶음밥", premium:false, recipe:["김치 먼저 볶기","밥 넣고 섞기","참기름"]},
    {name:"참치마요덮밥", premium:false, recipe:["참치+마요","간장 약간","밥 위"]},
    {name:"비빔밥", premium:true, recipe:["나물/고기","고추장","비비기"]}
  ],
  "면류":[
    {name:"우동", premium:false, recipe:["가쓰오육수","우동면","쪽파"]},
    {name:"라멘", premium:true, recipe:["베이스","면 삶기","차슈/계란"]},
    {name:"스파게티", premium:true, recipe:["면 삶기","올리브유/마늘","소스"]}
  ],
  "빵·간식류":[
    {name:"크루아상", premium:true, recipe:["버터 레이어","숙성","굽기"]},
    {name:"호떡", premium:false, recipe:["반죽","설탕/씨앗","굽기"]},
    {name:"샌드위치", premium:false, recipe:["빵","햄치즈/야채","소스"]}
  ],
  "해산물류":[
    {name:"연어초밥", premium:true, recipe:["밥 식초간","연어","와사비"]},
    {name:"새우튀김", premium:false, recipe:["반죽","튀기기","소금/소스"]},
    {name:"조개탕", premium:true, recipe:["해감","맑게 끓이기","파/마늘"]}
  ]
};

const BASE_PRICE = 10;
function priced(item){ return item.premium ? (Math.random()<0.5?12:13) : BASE_PRICE; }

function storageKey(){ return "questGame_guest"; }
function load(){ try{ return JSON.parse(localStorage.getItem(storageKey())); }catch(e){ return null; } }
function save(){ localStorage.setItem(storageKey(), JSON.stringify(state)); }
function id(){ return Math.random().toString(36).slice(2,9); }
function randOne(arr){ return arr[Math.floor(Math.random()*arr.length)]; }

/* ========= 초기 상태 ========= */
let state = load() || {
  name:"신지성",
  levelIdx:0,
  coins:40,
  emeralds:0,
  aura:30,
  banDays:0,
  blockDays:0,
  ramenCount:0,
  items:[],
  tasks:[
    {id:id(), title:"걷기 5,000~6,000걸음 (8/18)", done:false},
    {id:id(), title:"매일 적당히 먹기 (저녁 남음)", done:false},
    {id:id(), title:"아무 음악이나 30분 틀기", done:false},
    {id:id(), title:"11시에 잠들기", done:false, special:"sleep11"},
    {id:id(), title:"라면 2번 절대 먹지 말기", done:false, special:"ramen2"}
  ],
  lastReward:[]
};

/* ========= 렌더 ========= */
function syncLevel(){
  let idx=0; for(let i=0;i<AURA_THRESH.length;i++){ if(state.aura>=AURA_THRESH[i]) idx=i; }
  state.levelIdx=idx;
}
function $(id){ return document.getElementById(id); }

function renderProfile(){
  syncLevel();
  $("avatar").textContent = AVATARS[state.levelIdx];
  $("name").textContent   = state.name;
  $("level").textContent  = LEVELS[state.levelIdx];
  $("coins").textContent  = state.coins;
  $("emeralds").textContent=state.emeralds;
  $("aura").textContent   = state.aura;
  $("blockDays").textContent=state.blockDays;
  $("banDays").textContent  =state.banDays;
  $("ramenCount").textContent=state.ramenCount;
}
function renderTasks(){
  const box = $("taskList"); box.innerHTML="";
  state.tasks.forEach(t=>{
    const el = document.createElement("div");
    el.className="task";
    el.innerHTML = `<div>
      <div>${t.title} ${t.special?'<span class="pill">특수</span>':''}</div>
      <div class="mut">${t.done?'<span style="color:#86efac">완료</span>':'진행중'}</div>
    </div>`;
    const right = document.createElement("div");
    const d = document.createElement("button"); d.textContent="완료"; d.onclick=()=>completeTask(t.id);
    const f = document.createElement("button"); f.textContent="실패"; f.className="ghost"; f.onclick=()=>failTask(t.id);
    right.append(d,f);
    el.appendChild(right);
    box.appendChild(el);
  });
}
function generateReward6(){
  const cats = Object.keys(FOODS);
  state.lastReward = cats.map(cat=>{
    const pick = {...randOne(FOODS[cat])};
    pick.category = cat; pick.price = priced(pick);
    return pick;
  });
}
function renderReward(){
  const box = $("rewardBox"); box.innerHTML="";
  state.lastReward.forEach(f=>{
    const el = document.createElement("div");
    el.className="food";
    el.innerHTML = `<div><b>${f.name}</b> <span class="pill">${f.category}${f.premium?' · 고급':''}</span>
      <div class="mut">보상 생성</div></div>
      <div class="pill">${f.price}코인</div>`;
    box.appendChild(el);
  });
}
function renderShop(){
  const box = $("shop"); box.innerHTML="";
  state.lastReward.forEach((f,i)=>{
    const el = document.createElement("div");
    el.className="food";
    const btn = document.createElement("button");
    btn.textContent="선택"; btn.onclick=()=>attemptBuy(i);
    el.innerHTML = `<div><b>${f.name}</b> <span class="pill">${f.category}${f.premium?' · 고급':''}</span>
      <div class="mut">${f.price}코인 필요</div></div>`;
    el.appendChild(btn);
    box.appendChild(el);
  });
}
function showRecipe(f){
  const steps = (f.recipe||["재료 준비","기본 손질","조리"]).map((s,i)=>`${i+1}. ${s}`).join("\n");
  $("recipeBox").textContent = `[${f.category}] ${f.name} 레시피\n${steps}`;
}
function renderAll(){ renderProfile(); renderTasks(); renderReward(); renderShop(); }

/* ========= 동작 ========= */
function rewardOnSuccess(){
  if(state.blockDays>0 || state.banDays>0){
    alert("보상 차단/정지 중 → 코인·오라 지급 없음 (음식만 생성)");
  }else{
    state.coins += 10;
    state.aura  += 10;
  }
  generateReward6(); save(); renderAll();
}
function completeTask(taskId){
  const t = state.tasks.find(x=>x.id===taskId); if(!t) return;
  t.done = true;
  alert("잘하네, 너가 가진 코인으로 추천음식 골라봐 ⬇️");
  rewardOnSuccess();
}
function failTask(taskId){
  const t = state.tasks.find(x=>x.id===taskId); if(!t) return;
  t.done = false;
  let delta = -1; if(t.special==="sleep11") delta = -3;
  state.aura += delta;
  alert("아이고...실패했네..다음에 다시 도전해봐!");
  save(); renderAll();
}
function addTask(raw){
  const title = raw.trim(); if(!title) return;
  const task = {id:id(), title, done:false};
  if(title.includes("11시에 잠들기")) task.special="sleep11";
  if(title.includes("라면 2번 절대 먹지 말기")) task.special="ramen2";
  state.tasks.unshift(task); save(); renderTasks();
}
function attemptBuy(i){
  const f = state.lastReward[i];
  if(!confirm(`${f.name}은(는) ${f.price}코인이 필요합니다. ${f.price}코인을 지불하시겠습니까?`)) return;
  if(state.coins < f.price) return alert("코인 부족");
  state.coins -= f.price; showRecipe(f); save(); renderAll();
}
function ramenInc(){
  state.ramenCount++;
  if(state.ramenCount>=2){ state.aura -= 3; alert("라면 2회 달성 → Aura -3"); }
  save(); renderAll();
}
function ramenReset(){ state.ramenCount=0; save(); renderAll(); }
function punish(type){
  function block(d){ state.blockDays=Math.max(state.blockDays,d); }
  function ban(d){ state.banDays=Math.max(state.banDays,d); }
  function levelDown(n){ state.levelIdx=Math.max(0,state.levelIdx-n); state.aura=Math.min(state.aura, AURA_THRESH[state.levelIdx]); }
  function vbReset(){ state.levelIdx=0; state.aura=Math.min(state.aura, AURA_THRESH[0]); }

  if(type==="aura"){ ban(10); state.aura-=5; state.coins=Math.max(0,state.coins-20); vbReset(); block(5); alert("오라 무단 사용 철퇴 발동"); }
  else if(type==="coin"){ ban(7); state.coins=Math.max(0,state.coins-30); state.aura-=5; levelDown(2); block(3); alert("코인 무단 조작 철퇴 발동"); }
  else if(type==="task"){ state.aura-=10; state.coins=Math.max(0,state.coins-40); levelDown(1); addTask("패널티: 푸쉬업 100회"); addTask("패널티: 걷기 10,000걸음"); alert("할일 결과 조작 철퇴 발동"); }
  save(); renderAll();
}
function tickDay(){ if(state.blockDays>0)state.blockDays--; if(state.banDays>0)state.banDays--; save(); renderAll(); }

/* ========= 이벤트 ========= */
$("startBtn").onclick = ()=>{
  $("welcome").classList.add("hidden");
  $("game").classList.remove("hidden");
  if(state.lastReward.length===0) generateReward6();
  renderAll();
};
$("addTaskBtn").onclick = ()=>{ const v=$("taskInput").value; $("taskInput").value=""; addTask(v); };
$("nameInput").addEventListener("keydown", e=>{ if(e.key==="Enter"){ const v=e.target.value.trim(); if(v){ state.name=v; save(); renderAll(); e.target.value=""; } }});
$("ramenPlus").onclick = ramenInc;
$("ramenZero").onclick = ramenReset;
$("cheatAura").onclick = ()=>punish("aura");
$("cheatCoin").onclick = ()=>punish("coin");
$("cheatTask").onclick = ()=>punish("task");
$("tickBtn").onclick = tickDay;

$("exportBtn").onclick = ()=>{
  const blob = new Blob([JSON.stringify(state,null,2)], {type:"application/json"});
  const a = document.createElement("a"); a.href = URL.createObjectURL(blob); a.download="questGame-save.json"; a.click(); URL.revokeObjectURL(a.href);
};
$("importFile").onchange = (e)=>{
  const f = e.target.files[0]; if(!f) return;
  const r = new FileReader();
  r.onload = ()=>{ try{ const data=JSON.parse(r.result); state=data; save(); renderAll(); }catch{ alert("JSON 오류"); } };
  r.readAsText(f);
};
$("resetBtn").onclick = ()=>{
  if(!confirm("정말 초기화할래?")) return;
  localStorage.removeItem(storageKey()); location.reload();
};
</script>

<!-- ▼▼▼ 이후에 로그인 붙이고 싶으면 여기 주석 풀어 ▼▼▼
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-auth-compat.js"></script>
<script>
// 1) Firebase 키 채우고 initialize → auth.onAuthStateChanged에서 welcome 대신 바로 game 열기
// 2) 구글/이메일 버튼 추가해서 signInWithPopup / signInWithEmailAndPassword 연결
</script>
<!-- ▲▲▲ 로그인은 나중에. 지금은 게스트 즉시 시작으로 끝 ▲▲▲ -->

</body>
</html>
