# index.html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>보상형 할일 게임 – 로그인/튜토리얼/게임</title>
<style>
  body{font-family:system-ui,Segoe UI,Apple SD Gothic Neo,Roboto,Arial; margin:0; background:#0b1015; color:#e6edf3}
  header{display:flex; gap:12px; align-items:center; justify-content:space-between; padding:14px 16px; border-bottom:1px solid #1f2937; background:#0f1620}
  .brand{font-weight:700}
  .container{max-width:1100px; margin:0 auto; padding:16px}
  .card{background:#111827; border:1px solid #1f2937; border-radius:16px; padding:16px; box-shadow:0 10px 30px rgba(0,0,0,.25); margin-bottom:16px}
  .title{font-weight:700; margin-bottom:10px}
  .grid{display:grid; gap:10px}
  .grid2{display:grid; grid-template-columns:1fr 1fr; gap:10px}
  .row{display:flex; gap:10px; flex-wrap:wrap}
  input,button{border-radius:12px; border:1px solid #1f2937; padding:10px 12px}
  input{background:#0b1220; color:#e6edf3}
  button{background:#2563eb; color:white; border:0; cursor:pointer}
  button.ghost{background:transparent; color:#93c5fd; border:1px solid #1f2937}
  button.warn{background:#ef4444}
  .chip{display:inline-flex; gap:6px; padding:6px 10px; border-radius:999px; background:#0b1220; border:1px solid #1f2937; font-size:14px}
  .list{display:flex; flex-direction:column; gap:8px}
  .task{display:flex; justify-content:space-between; align-items:center; gap:10px; padding:10px; border-radius:12px; background:#0c1423; border:1px solid #1f2937}
  .foods{display:grid; grid-template-columns:repeat(3,1fr); gap:8px}
  .food{padding:10px; border-radius:12px; background:#0c1423; border:1px solid #1f2937; display:flex; justify-content:space-between; align-items:center}
  .pill{font-size:12px; padding:4px 8px; border-radius:999px; border:1px solid #1f2937; background:#0b1220}
  .mono{font-family:ui-monospace, SFMono-Regular, Menlo, monospace; font-size:13px; white-space:pre-wrap}
  .kicker{font-size:12px; color:#9ca3af}
  .hidden{display:none}
  .center{text-align:center}
  .modal{position:fixed; inset:0; background:rgba(0,0,0,.6); display:flex; align-items:center; justify-content:center}
  .modal .dialog{background:#111827; border:1px solid #1f2937; border-radius:16px; padding:18px; width:min(520px,92vw)}
</style>
</head>
<body>
<header>
  <div class="brand">✅ 보상형 할일 게임</div>
  <div id="topActions" class="row">
    <button id="exportBtn" class="ghost hidden">데이터 내보내기</button>
    <label class="chip hidden" id="importWrap">JSON 가져오기<input type="file" id="importFile" accept="application/json" /></label>
    <button id="logoutBtn" class="ghost hidden">로그아웃</button>
  </div>
</header>

<div class="container">

  <!-- 로그인 섹션 -->
  <section id="authSection" class="card">
    <div class="title">로그인</div>
    <div class="grid">
      <div class="kicker">방식 선택</div>
      <div class="row">
        <button id="googleBtn">구글 로그인</button>
        <button id="naverBtn" class="ghost">네이버 로그인</button>
      </div>
      <div class="kicker">또는 이메일 로그인</div>
      <div class="grid2">
        <input id="emailInput" placeholder="이메일" />
        <input id="passInput" type="password" placeholder="비밀번호" />
      </div>
      <div class="row">
        <button id="emailLoginBtn">이메일 로그인</button>
        <button id="emailSignupBtn" class="ghost">이메일 회원가입</button>
      </div>
      <div class="kicker mono">로그인 성공 시 자동으로 튜토리얼 시작 → “어서오세요!” 표시</div>
    </div>
  </section>

  <!-- 튜토리얼 -->
  <section id="tutorialSection" class="card hidden">
    <div class="title">튜토리얼</div>
    <div class="grid">
      <div>1) 할일 추가 → 완료 클릭 시 코인+오라 보상, 음식 6종 보상 생성</div>
      <div>2) 상점에서 음식 선택 → 결제 확인 → 레시피 공개(고급요리 20~30% 비싸짐)</div>
      <div>3) 특수 규칙: 11시 실패, 라면 2회는 오라 패널티</div>
      <div>4) 규칙 위반(오라/코인/거짓완료) 시 중징계(철퇴) 즉시 발동</div>
      <div class="row">
        <button id="tutorialDone">튜토리얼 완료</button>
      </div>
    </div>
  </section>

  <!-- 게임 섹션 -->
  <section id="gameSection" class="hidden">
    <div class="card">
      <div class="title">프로필</div>
      <div class="row">
        <span class="chip">아바타: <span id="avatar">😐</span></span>
        <span class="chip">이름: <span id="name">신지성</span></span>
        <span class="chip">레벨: <span id="level">Vb.1</span></span>
        <span class="chip">코인: <b id="coins">0</b></span>
        <span class="chip">에메랄드: <b id="emeralds">0</b></span>
        <span class="chip">Aura: <b id="aura">0</b></span>
        <span class="chip">보상차단: <b id="blockDays">0</b>d</span>
        <span class="chip">정지: <b id="banDays">0</b>d</span>
      </div>
      <div class="row" style="margin-top:8px">
        <input id="nameInput" placeholder="이름 변경(엔터)" />
        <button id="tickBtn" class="ghost">하루 경과</button>
        <button id="resetBtn" class="warn">전체 초기화</button>
      </div>
    </div>

    <div class="card">
      <div class="title">할일</div>
      <div class="row">
        <input id="taskInput" placeholder='예) "11시에 잠들기" / "라면 2번 절대 먹지 말기"' />
        <button id="addTaskBtn">할일저장</button>
      </div>
      <div id="taskList" class="list" style="margin-top:10px"></div>
    </div>

    <div class="card">
      <div class="title">라면 위반 카운터</div>
      <div class="row">
        <button id="ramenPlus">라면 +1</button>
        <button id="ramenZero" class="ghost">카운터 초기화</button>
        <div class="kicker">오늘 라면: <b id="ramenCount">0</b> / 2회 달성 시 오라 -3</div>
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
      <div class="kicker">실운영에선 숨겨도 됨. 여기선 작동 확인용.</div>
    </div>
  </section>
</div>

<!-- “어서오세요!” 모달 -->
<div id="welcomeModal" class="modal hidden"><div class="dialog">
  <div class="title">어서오세요!</div>
  <div class="grid">
    <div>튜토리얼을 시작합니다. 1분 컷.</div>
    <div class="row center">
      <button id="startTutorial">시작</button>
    </div>
  </div>
</div></div>

<!-- Firebase SDK (Auth만) -->
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-auth-compat.js"></script>
<!-- 네이버 로그인 SDK -->
<script src="https://static.nid.naver.com/js/naveridlogin_js_sdk_2.0.2.js"></script>

<script>
// ===================== 인증 설정 =====================
// TODO: 아래 Firebase 키 채우기 (프로젝트 만들고 Web App 추가)
// Firebase 콘솔: https://console.firebase.google.com/
const firebaseConfig = {
  apiKey: "YOUR_FIREBASE_API_KEY",
  authDomain: "YOUR_FIREBASE_AUTH_DOMAIN", // 예: your-app.firebaseapp.com
  projectId: "YOUR_FIREBASE_PROJECT_ID",
  appId: "YOUR_FIREBASE_APP_ID"
};
firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();

// 네이버 OAuth (리다이렉트 기반)
// TODO: 아래 clientId, redirectUrl을 네이버 개발자 콘솔에서 발급/설정
const NAVER_CLIENT_ID = "YOUR_NAVER_CLIENT_ID";
const NAVER_REDIRECT_URL = location.origin + location.pathname; // 깃허브 Pages URL 그대로
// 네이버는 공식 SDK로 팝업/리다이렉트. 여기선 리다이렉트 후 해시 파싱만.
function initNaver(){
  // 로그인 버튼 없이 바로 이동 유도: 실제로는 별도 페이지 구성 가능.
  const url = new URL(location.href);
  const params = new URLSearchParams(url.hash.replace(/^#/, '')); // 액세스 토큰이 해쉬에 담겨옴
  if(params.get("access_token")){
    // 토큰을 백엔드로 보내 검증하는 게 정석이나, 여기선 데모라 로컬 세션만 생성
    const fakeUser = { uid: "naver-"+params.get("access_token").slice(0,8), provider:"naver" };
    localStorage.setItem("naverUser", JSON.stringify(fakeUser));
    onAuthReady(fakeUser); // 데모용: 실제 서비스면 서버검증 필수
    // 해시 정리
    history.replaceState({}, "", location.pathname);
  }
}
initNaver();

document.getElementById("googleBtn").onclick = async ()=>{
  try{
    const provider = new firebase.auth.GoogleAuthProvider();
    await auth.signInWithPopup(provider);
  }catch(e){ alert("구글 로그인 실패: "+e.message); }
};

document.getElementById("naverBtn").onclick = ()=>{
  // 네이버 OAuth 2.0 권한 요청 (Implicit)
  const state = Math.random().toString(36).slice(2);
  const authUrl =
    "https://nid.naver.com/oauth2.0/authorize?response_type=token"
    + "&client_id=" + encodeURIComponent(NAVER_CLIENT_ID)
    + "&redirect_uri=" + encodeURIComponent(NAVER_REDIRECT_URL)
    + "&state=" + encodeURIComponent(state);
  location.href = authUrl;
};

document.getElementById("emailSignupBtn").onclick = async ()=>{
  const email = document.getElementById("emailInput").value.trim();
  const pass  = document.getElementById("passInput").value;
  if(!email || !pass) return alert("이메일/비밀번호 입력");
  try{ await auth.createUserWithEmailAndPassword(email, pass); }
  catch(e){ alert("회원가입 실패: "+e.message); }
};

document.getElementById("emailLoginBtn").onclick = async ()=>{
  const email = document.getElementById("emailInput").value.trim();
  const pass  = document.getElementById("passInput").value;
  if(!email || !pass) return alert("이메일/비밀번호 입력");
  try{ await auth.signInWithEmailAndPassword(email, pass); }
  catch(e){ alert("로그인 실패: "+e.message); }
};

document.getElementById("logoutBtn").onclick = async ()=>{
  localStorage.removeItem("naverUser");
  await auth.signOut();
  location.reload();
};

auth.onAuthStateChanged((user)=>{
  const naverUser = localStorage.getItem("naverUser");
  if(user){ onAuthReady(user); }
  else if(naverUser){ onAuthReady(JSON.parse(naverUser)); }
  else{ showAuth(); }
});

function showAuth(){
  document.getElementById("authSection").classList.remove("hidden");
  document.getElementById("tutorialSection").classList.add("hidden");
  document.getElementById("gameSection").classList.add("hidden");
  document.getElementById("logoutBtn").classList.add("hidden");
  document.getElementById("exportBtn").classList.add("hidden");
  document.getElementById("importWrap").classList.add("hidden");
}

async function onAuthReady(user){
  document.getElementById("logoutBtn").classList.remove("hidden");
  document.getElementById("authSection").classList.add("hidden");

  // 처음 로그인 시 “어서오세요!” 모달 → 튜토리얼 섹션
  const first = !localStorage.getItem("__tut_done__");
  if(first){
    document.getElementById("welcomeModal").classList.remove("hidden");
    document.getElementById("startTutorial").onclick = ()=>{
      document.getElementById("welcomeModal").classList.add("hidden");
      document.getElementById("tutorialSection").classList.remove("hidden");
    };
    document.getElementById("tutorialDone").onclick = ()=>{
      localStorage.setItem("__tut_done__", "1");
      startGameSession(user);
    };
  }else{
    startGameSession(user);
  }
}

// ===================== 게임 로직 =====================
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

let state = null;
function newState(uid){
  return {
    owner: uid,
    name: "신지성",
    levelIdx: 0,
    coins: 40,
    emeralds: 0,
    aura: 30,
    banDays: 0,
    blockDays: 0,
    ramenCount: 0,
    items: [],
    tasks: [
      {id: id(), title:"걷기 5,000~6,000걸음 (8/18)", done:false},
      {id: id(), title:"매일 적당히 먹기 (저녁 남음)", done:false},
      {id: id(), title:"아무 음악이나 30분 틀기", done:false},
      {id: id(), title:"11시에 잠들기", done:false, special:"sleep11"},
      {id: id(), title:"라면 2번 절대 먹지 말기", done:false, special:"ramen2"}
    ],
    lastReward: []
  };
}
function storageKey(uid){ return "questGame_"+uid; }
function load(uid){
  try{ return JSON.parse(localStorage.getItem(storageKey(uid))); }catch(e){ return null; }
}
function save(){ if(state) localStorage.setItem(storageKey(state.owner), JSON.stringify(state)); }
function id(){ return Math.random().toString(36).slice(2,9); }

function startGameSession(user){
  const uid = user.uid;
  state = load(uid) || newState(uid);
  // UI 전환
  document.getElementById("tutorialSection").classList.add("hidden");
  document.getElementById("gameSection").classList.remove("hidden");
  document.getElementById("exportBtn").classList.remove("hidden");
  document.getElementById("importWrap").classList.remove("hidden");
  renderAll();
}

function syncLevel(){
  let idx = 0;
  for(let i=0;i<AURA_THRESH.length;i++){
    if(state.aura>=AURA_THRESH[i]) idx = i;
  }
  state.levelIdx = idx;
}

function renderAll(){
  syncLevel();
  // 프로필
  byId("avatar").textContent = AVATARS[state.levelIdx];
  byId("name").textContent = state.name;
  byId("level").textContent = LEVELS[state.levelIdx];
  byId("coins").textContent = state.coins;
  byId("emeralds").textContent = state.emeralds;
  byId("aura").textContent = state.aura;
  byId("blockDays").textContent = state.blockDays;
  byId("banDays").textContent = state.banDays;
  byId("ramenCount").textContent = state.ramenCount;

  // 할일
  const box = byId("taskList"); box.innerHTML="";
  state.tasks.forEach(t=>{
    const el = document.createElement("div");
    el.className="task";
    el.innerHTML = `<div>
      <div>${t.title} ${t.special?'<span class="pill">특수</span>':''}</div>
      <div class="kicker">${t.done?'<span style="color:#86efac">완료</span>':'진행중'}</div>
    </div>`;
    const right = document.createElement("div");
    const doneBtn = document.createElement("button");
    doneBtn.textContent = "완료"; doneBtn.onclick = ()=>completeTask(t.id);
    const failBtn = document.createElement("button");
    failBtn.textContent = "실패"; failBtn.className="ghost"; failBtn.onclick = ()=>failTask(t.id);
    right.append(doneBtn, failBtn);
    el.appendChild(right);
    box.appendChild(el);
  });

  // 보상/상점
  renderReward();
  renderShop();
}

function generateReward6(){
  const cats = Object.keys(FOODS);
  state.lastReward = cats.map(cat=>{
    const pick = {...randOne(FOODS[cat])};
    pick.category = cat;
    pick.price = priced(pick);
    return pick;
  });
}
function renderReward(){
  const box = byId("rewardBox"); box.innerHTML="";
  state.lastReward.forEach(f=>{
    const el = document.createElement("div");
    el.className="food";
    el.innerHTML = `<div><b>${f.name}</b> <span class="pill">${f.category}${f.premium?' · 고급':''}</span><div class="kicker">보상 생성</div></div>
                    <div class="pill">${f.price}코인</div>`;
    box.appendChild(el);
  });
}
function renderShop(){
  const box = byId("shop"); box.innerHTML="";
  state.lastReward.forEach((f,i)=>{
    const el = document.createElement("div");
    el.className="food";
    const btn = document.createElement("button");
    btn.textContent = "선택";
    btn.onclick = ()=>attemptBuy(i);
    el.innerHTML = `<div><b>${f.name}</b> <span class="pill">${f.category}${f.premium?' · 고급':''}</span><div class="kicker">${f.price}코인 필요</div></div>`;
    el.appendChild(btn);
    box.appendChild(el);
  });
}
function showRecipe(f){
  const steps = (f.recipe||["재료 준비","기본 손질","조리"]).map((s,i)=>`${i+1}. ${s}`).join("\n");
  byId("recipeBox").textContent = `[${f.category}] ${f.name} 레시피\n${steps}`;
}

function attemptBuy(i){
  const f = state.lastReward[i];
  if(!confirm(`${f.name}은(는) ${f.price}코인이 필요합니다. ${f.price}코인을 지불하시겠습니까?`)) return;
  if(state.coins < f.price) return alert("코인 부족");
  state.coins -= f.price;
  showRecipe(f);
  save(); renderAll();
}

function rewardOnSuccess(){
  if(state.blockDays>0 || state.banDays>0){
    alert("보상 차단/정지 중 → 코인·오라 지급 없음 (음식 목록만 생성)");
  }else{
    state.coins += 10;
    state.aura  += 10;
  }
  generateReward6();
  save(); renderAll();
}

function completeTask(id){
  const t = state.tasks.find(x=>x.id===id); if(!t) return;
  t.done = true;
  alert("잘하네, 너가 가진 코인으로 추천음식 골라봐 ⬇️");
  rewardOnSuccess();
}
function failTask(id){
  const t = state.tasks.find(x=>x.id===id); if(!t) return;
  t.done = false;
  let delta = -1;
  if(t.special==="sleep11") delta = -3;
  state.aura += delta;
  alert("아이고...실패했네..다음에 다시 도전해봐!");
  save(); renderAll();
}

function addTask(title){
  const task = {id:id(), title:title.trim(), done:false};
  if(task.title.includes("11시에 잠들기")) task.special="sleep11";
  if(task.title.includes("라면 2번 절대 먹지 말기")) task.special="ramen2";
  state.tasks.unshift(task);
  save(); renderAll();
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
  function levelDown(n){ state.levelIdx=Math.max(0,state.levelIdx-n); state.aura=Math.min(state.aura,AURA_THRESH[state.levelIdx]); }
  function vbReset(){ state.levelIdx=0; state.aura=Math.min(state.aura,AURA_THRESH[0]); }
  function removeItems(k=2){ /* 아이템 목록 구현시 삭제 처리 */ }

  if(type==="aura"){
    ban(10); state.aura -=5; state.coins=Math.max(0,state.coins-20); vbReset(); block(5); removeItems(2);
    alert("오라 무단 사용 철퇴 발동");
  }else if(type==="coin"){
    ban(7); state.coins=Math.max(0,state.coins-30); state.aura-=5; levelDown(2); block(3);
    alert("코인 무단 조작 철퇴 발동");
  }else if(type==="task"){
    state.aura-=10; state.coins=Math.max(0,state.coins-40); levelDown(1);
    addTask("패널티: 푸쉬업 100회"); addTask("패널티: 걷기 10,000걸음");
    alert("할일 결과 조작 철퇴 발동");
  }
  save(); renderAll();
}

function tickDay(){ if(state.blockDays>0)state.blockDays--; if(state.banDays>0)state.banDays--; save(); renderAll(); }

function byId(id){ return document.getElementById(id); }

// 이벤트 바인딩
byId("addTaskBtn").onclick = ()=>{ const v=byId("taskInput").value; byId("taskInput").value=""; if(v.trim()) addTask(v); };
byId("nameInput").addEventListener("keydown", e=>{ if(e.key==="Enter"){ const v=e.target.value.trim(); if(v){ state.name=v; save(); renderAll(); e.target.value=""; } }});
byId("ramenPlus").onclick = ramenInc;
byId("ramenZero").onclick = ramenReset;
byId("cheatAura").onclick = ()=>punish("aura");
byId("cheatCoin").onclick = ()=>punish("coin");
byId("cheatTask").onclick = ()=>punish("task");
byId("tickBtn").onclick = tickDay;

byId("exportBtn").onclick = ()=>{
  const blob = new Blob([JSON.stringify(state,null,2)], {type:"application/json"});
  const a = document.createElement("a"); a.href = URL.createObjectURL(blob); a.download = "questGame-save.json"; a.click(); URL.revokeObjectURL(a.href);
};
byId("importFile").onchange = (e)=>{
  const f = e.target.files[0]; if(!f) return;
  const r = new FileReader();
  r.onload = ()=>{ try{ const data=JSON.parse(r.result); if(data.owner===state.owner){ state=data; save(); renderAll(); } else alert("다른 계정 데이터"); }catch(_){ alert("JSON 오류"); } };
  r.readAsText(f);
};

// 초기 보상 생성(최초 진입 시)
function bootstrap(){
  if(!state) return;
  if((state.lastReward||[]).length===0){ generateReward6(); save(); }
  renderAll();
}
</script>
</body>
</html>
