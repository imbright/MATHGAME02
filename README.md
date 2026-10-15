[MATHGAME.html](https://github.com/user-attachments/files/33487923/MATHGAME.html)[Uploading MATHGA<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>数学益智乐园 | Math Puzzle Park</title>
<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family:system-ui, sans-serif;
}
body{
  background:#eeeeee;
  color:#222222;
}
.page{
  display:none;
  min-height:100vh;
  padding:24px;
  max-width:1200px;
  margin:0 auto;
}
.page.active{
  display:block;
}
.top-nav{
  display:flex;
  gap:10px;
  margin-bottom:16px;
  flex-wrap:wrap;
}
.top-nav button{
  padding:12px 24px;
  border:none;
  border-radius:8px;
  font-size:18px;
  cursor:pointer;
}
.btn-blue{background:#348ede;color:white;}
.btn-red{background:#dd4444;color:white;}
.lang-box{
  margin:12px 0;
}
.lang-box button{
  padding:8px 16px;
  font-size:16px;
  border:2px solid #333;
  border-radius:6px;
  cursor:pointer;
  margin-right:8px;
}
.lang-box button.active{
  background:#2266dd;
  color:white;
}
.header-row{
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:16px;
  flex-wrap:wrap;
  gap:12px;
}
.score-text{
  font-size:20px;
  font-weight:bold;
}
.player-panel{
  background:#fff;
  padding:20px;
  border-radius:10px;
  border:1px solid #ccc;
  margin-bottom:16px;
  display:flex;
  align-items:center;
  gap:20px;
  flex-wrap:wrap;
}
.player-inputs{
  display:flex;
  flex-direction:column;
  gap:8px;
}
.player-inputs input{
  padding:8px;
  font-size:16px;
  width:220px;
}
.game-card{
  background:#fff;
  padding:24px;
  border-radius:12px;
  box-shadow:0 2px 8px #00000015;
}
.game-card h2{
  font-size:30px;
  margin-bottom:16px;
}
.difficulty-line{
  margin:16px 0;
  font-size:18px;
}
.difficulty-line select{
  padding:6px 10px;
  font-size:16px;
  min-width:220px;
}
.question-text{
  font-size:24px;
  margin:20px 0;
}
.input-line input{
  width:100%;
  max-width:600px;
  padding:12px;
  font-size:20px;
}
.ctrl-buttons{
  margin-top:20px;
  display:flex;
  gap:12px;
  flex-wrap:wrap;
}
.ctrl-buttons button{
  padding:14px 30px;
  border:none;
  border-radius:8px;
  font-size:18px;
  cursor:pointer;
}
.btn-green{background:#22aa44;color:white;}
.msg-correct{color:#22aa44;font-weight:bold;font-size:22px;margin-top:16px;}
.msg-wrong{color:#dd2222;font-weight:bold;font-size:22px;margin-top:16px;}
.progress-container{
  margin:12px 0;
}
.progress-bar-bg{
  width:100%;
  height:22px;
  background:#ddd;
  border-radius:11px;
  overflow:hidden;
}
.progress-fill{
  height:100%;
  background:#22aa44;
  transition:0.25s;
}
.game-list{
  display:grid;
  grid-template-columns: repeat(auto-fit, minmax(220px,1fr));
  gap:16px;
  margin-top:24px;
}
.game-list button{
  padding:24px;
  font-size:20px;
  border:none;
  border-radius:10px;
  background:#348ede;
  color:white;
  cursor:pointer;
}
button:hover{
  opacity:0.85;
}
</style>
</head>
<body>

<!-- 主页 -->
<div id="page-home" class="page active">
  <div class="header-row">
    <h1>数学益智乐园</h1>
    <div class="score-text">总积分 Point: <span id="home-score">0</span></div>
  </div>
  <div class="player-panel">
    <div class="player-inputs">
      <div>玩家名字：<input id="playerName" placeholder="输入你的名字"></div>
      <div>玩家ID：<input id="playerID" placeholder="自定义ID"></div>
    </div>
  </div>
  <div class="lang-box">
    <button id="btn-zh" class="active">中文</button>
    <button id="btn-en">English</button>
  </div>
  <div class="game-list">
    <button onclick="openGame('game24')">24点</button>
    <button onclick="openGame('calc')">口算计算</button>
    <button onclick="openGame('wordprob')">应用题</button>
    <button onclick="openGame('sudoku')">数独</button>
    <button onclick="openGame('multi')">多人对战(支持Bot人机)</button>
  </div>
</div>

<!-- 24点 -->
<div id="page-game24" class="page">
  <div class="top-nav">
    <button class="btn-red" onclick="goHome()">返回主页</button>
  </div>
  <div class="lang-box">
    <button id="g24-zh" class="active">中文</button>
    <button id="g24-en">English</button>
  </div>
  <div class="header-row">
    <h2>24点游戏</h2>
    <div class="score-text">单项积分: <span id="g24-score">0</span></div>
  </div>
  <div class="game-card">
    <div class="difficulty-line">
      难度：
      <select id="diff24" onchange="resetGame24()">
        <option value="easy">简单 (+3)</option>
        <option value="mid">中等 (+6)</option>
        <option value="hard">困难 (+10)</option>
        <option value="devil">魔鬼训练（单项35分解锁）</option>
      </select>
    </div>
    <div class="question-text" id="q24">数字： -- -- -- --</div>
    <div class="input-line">
      <input id="ans24" placeholder="写算式，使用 +-*/()">
    </div>
    <div class="ctrl-buttons">
      <button class="btn-green" onclick="submit24()">提交答案</button>
      <button class="btn-blue" onclick="nextQ24()">下一题</button>
      <button class="btn-blue" onclick="prevQ24()">上一题</button>
    </div>
    <div id="msg24"></div>
  </div>
</div>

<!-- 口算 -->
<div id="page-calc" class="page">
  <div class="top-nav">
    <button class="btn-red" onclick="goHome()">返回主页</button>
  </div>
  <div class="lang-box">
    <button id="calc-zh" class="active">中文</button>
    <button id="calc-en">English</button>
  </div>
  <div class="header-row">
    <h2>口算计算</h2>
    <div class="score-text">单项积分: <span id="calc-score">0</span></div>
  </div>
  <div class="game-card">
    <div class="difficulty-line">
      难度：
      <select id="diffCalc" onchange="resetCalc()">
        <option value="easy">简单 (+3)</option>
        <option value="mid">中等 (+6)</option>
        <option value="hard">困难 (+10)</option>
        <option value="devil">魔鬼训练（单项35分解锁）</option>
      </select>
    </div>
    <div class="question-text" id="qCalc">题目：</div>
    <div class="input-line">
      <input id="ansCalc" placeholder="填入答案">
    </div>
    <div class="ctrl-buttons">
      <button class="btn-green" onclick="submitCalc()">提交答案</button>
      <button class="btn-blue" onclick="nextQCalc()">下一题</button>
      <button class="btn-blue" onclick="prevQCalc()">上一题</button>
    </div>
    <div id="msgCalc"></div>
  </div>
</div>

<!-- 应用题 -->
<div id="page-wordprob" class="page">
  <div class="top-nav">
    <button class="btn-red" onclick="goHome()">返回主页</button>
  </div>
  <div class="lang-box">
    <button id="wp-zh" class="active">中文</button>
    <button id="wp-en">English</button>
  </div>
  <div class="header-row">
    <h2>应用题（希望杯难度）</h2>
    <div class="score-text">单项积分: <span id="wp-score">0</span></div>
  </div>
  <div class="game-card">
    <div class="difficulty-line">
      难度：
      <select id="diffWP" onchange="resetWP()">
        <option value="easy">简单 (+3)</option>
        <option value="mid">中等 (+6)</option>
        <option value="hard">困难 (+10)</option>
        <option value="devil">魔鬼训练（单项35分解锁）</option>
      </select>
    </div>
    <div class="question-text" id="qWP"></div>
    <div class="input-line">
      <input id="ansWP" placeholder="填入答案">
    </div>
    <div class="ctrl-buttons">
      <button class="btn-green" onclick="submitWP()">提交答案</button>
      <button class="btn-blue" onclick="nextQWP()">下一题</button>
      <button class="btn-blue" onclick="prevQWP()">上一题</button>
    </div>
    <div id="msgWP"></div>
  </div>
</div>

<!-- 数独 -->
<div id="page-sudoku" class="page">
  <div class="top-nav">
    <button class="btn-red" onclick="goHome()">返回主页</button>
  </div>
  <div class="lang-box">
    <button id="sd-zh" class="active">中文</button>
    <button id="sd-en">English</button>
  </div>
  <div class="header-row">
    <h2>数独</h2>
    <div class="score-text">单项积分: <span id="sd-score">0</span></div>
  </div>
  <div class="game-card">
    <div class="difficulty-line">
      难度：
      <select id="diffSD" onchange="resetSudoku()">
        <option value="easy">简单 (+3)</option>
        <option value="mid">中等 (+6)</option>
        <option value="hard">困难 (+10)</option>
        <option value="devil">魔鬼训练（单项35分解锁）</option>
      </select>
    </div>
    <div id="sudokuBox"></div>
    <div class="ctrl-buttons">
      <button class="btn-green" onclick="checkSudoku()">检查答案</button>
      <button class="btn-blue" onclick="nextSudoku()">新数独</button>
    </div>
    <div id="msgSD"></div>
  </div>
</div>

<!-- 多人对战（Bot人机版） -->
<div id="page-multi" class="page">
  <div class="top-nav">
    <button class="btn-red" onclick="goHome()">返回主页</button>
  </div>
  <div class="lang-box">
    <button id="multi-zh" class="active">中文</button>
    <button id="multi-en">English</button>
  </div>
  <div class="header-row">
    <h2>多人对战（最多5人，可添加Bot）</h2>
  </div>
  <div class="game-card">
    <div class="ctrl-buttons">
      <button class="btn-blue" onclick="addBotPlayer()">添加Bot对手</button>
      <button class="btn-blue" onclick="clearAllPlayer()">清空玩家</button>
    </div>
    <div id="multiPlayerList"></div>
    <div class="progress-container" id="multiProgressWrap"></div>
    <div class="question-text" id="multiQ"></div>
    <div class="input-line">
      <input id="multiAns" placeholder="你的答案">
    </div>
    <div class="ctrl-buttons">
      <button class="btn-green" onclick="multiSubmit()">提交</button>
      <button class="btn-blue" onclick="startMultiGame()">开始比赛</button>
    </div>
    <div id="multiMsg"></div>
    <div id="multiRank"></div>
  </div>
</div>

<script>
//全局存储
let saveData = JSON.parse(localStorage.getItem("mathGameSave")) || {
  totalScore:0,
  game24Score:0,
  calcScore:0,
  wpScore:0,
  sudokuScore:0,
  playerName:"",
  playerID:"",
  lang:"zh"
};
const pointTable = {easy:3,mid:6,hard:10,devil:18};

//24点变量
let q24List = [];
let q24Index = -1;
let curr24Nums = [];
//口算
let calcList = [];
let calcIndex = -1;
let currCalcAns = 0;
//应用题
let wpList = [];
let wpIndex = -1;
let currWPAns = 0;
//数独
let sudokuGrid = [];
//多人Bot对战
let multiPlayers = [];
let multiGameRunning = false;
let multiTotalQuestion = 5;
let multiCurrentQ = 0;
let multiQuestionPool = [];

//页面切换
function hideAllPages(){
  document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
}
function goHome(){
  hideAllPages();
  document.getElementById("page-home").classList.add("active");
  updateAllScore();
}
function openGame(gameName){
  hideAllPages();
  if(gameName === "game24"){
    document.getElementById("page-game24").classList.add("active");
    resetGame24();
  }else if(gameName === "calc"){
    document.getElementById("page-calc").classList.add("active");
    resetCalc();
  }else if(gameName === "wordprob"){
    document.getElementById("page-wordprob").classList.add("active");
    resetWP();
  }else if(gameName === "sudoku"){
    document.getElementById("page-sudoku").classList.add("active");
    resetSudoku();
  }else if(gameName === "multi"){
    document.getElementById("page-multi").classList.add("active");
    initMultiPlayer();
  }
  updateAllScore();
}

//保存和更新分数
function saveGameData(){
  localStorage.setItem("mathGameSave",JSON.stringify(saveData));
}
function updateAllScore(){
  document.getElementById("home-score").innerText = saveData.totalScore;
  document.getElementById("g24-score").innerText = saveData.game24Score;
  document.getElementById("calc-score").innerText = saveData.calcScore;
  document.getElementById("wp-score").innerText = saveData.wpScore;
  document.getElementById("sd-score").innerText = saveData.sudokuScore;
}

//玩家信息初始化
function initPlayerInfo(){
  document.getElementById("playerName").value = saveData.playerName;
  document.getElementById("playerID").value = saveData.playerID;
  document.getElementById("playerName").oninput = ()=>{
    saveData.playerName = document.getElementById("playerName").value;
    saveGameData();
  }
  document.getElementById("playerID").oninput = ()=>{
    saveData.playerID = document.getElementById("playerID").value;
    saveGameData();
  }
}

//随机数
function getR(min,max){
  return Math.floor(Math.random()*(max-min+1))+min;
}

//=====24点=====
function gen24(diff){
  let nums;
  if(diff === "easy") nums = [getR(1,10),getR(1,10),getR(1,10),getR(1,10)];
  else if(diff === "mid") nums = [getR(1,13),getR(1,13),getR(1,13),getR(1,13)];
  else if(diff === "hard") nums = [getR(1,20),getR(1,20),getR(1,20),getR(1,20)];
  else nums = [getR(1,30),getR(1,30),getR(1,30),getR(1,30)];
  return nums;
}
function resetGame24(){
  q24List = [];q24Index=-1;nextQ24();
}
function nextQ24(){
  const sel = document.getElementById("diff24").value;
  if(sel==="devil" && saveData.game24Score<35){
    document.getElementById("msg24").innerText="需要单项35分解锁魔鬼训练";
    return;
  }
  curr24Nums = gen24(sel);
  q24List.push([...curr24Nums]);
  q24Index = q24List.length-1;
  document.getElementById("q24").innerText = "数字："+curr24Nums.join("  ");
  document.getElementById("ans24").value="";
  document.getElementById("msg24").innerText="";
}
function prevQ24(){
  if(q24Index<=0) return;
  q24Index--;
  curr24Nums = q24List[q24Index];
  document.getElementById("q24").innerText = "数字："+curr24Nums.join("  ");
  document.getElementById("ans24").value="";
  document.getElementById("msg24").innerText="";
}
function submit24(){
  const expr = document.getElementById("ans24").value.trim();
  const sel = document.getElementById("diff24").value;
  try{
    const usedNums = expr.match(/\d+/g)?.map(Number) || [];
    const sortedCurr = [...curr24Nums].sort((a,b)=>a-b);
    const sortedUsed = usedNums.sort((a,b)=>a-b);
    if(JSON.stringify(sortedCurr)!==JSON.stringify(sortedUsed)){
      document.getElementById("msg24").className="msg-wrong";
      document.getElementById("msg24").innerText="❌ 必须使用全部4个数字！";
      return;
    }
    const res = eval(expr);
    if(Math.abs(res-24)<0.001){
      document.getElementById("msg24").className="msg-correct";
      document.getElementById("msg24").innerText="✅ 答对！+"+pointTable[sel]+"分";
      saveData.game24Score += pointTable[sel];
      saveData.totalScore += pointTable[sel];
      saveGameData();updateAllScore();
    }else{
      document.getElementById("msg24").className="msg-wrong";
      document.getElementById("msg24").innerText="❌ 结果不是24";
    }
  }catch(e){
    document.getElementById("msg24").className="msg-wrong";
    document.getElementById("msg24").innerText="❌ 算式格式错误";
  }
}

//=====口算=====
function genCalc(diff){
  if(diff==="easy"){
    let a=getR(1,20),b=getR(1,20);
    return {q:`${a}+${b}=?`,ans:a+b};
  }else if(diff==="mid"){
    let a=getR(10,50),b=getR(10,50);
    return {q:`${a}×${b}=?`,ans:a*b};
  }else if(diff==="hard"){
    let a=getR(20,100),b=getR(2,12);
    return {q:`${a}÷${b}=?`,ans:a/b};
  }else{
    let a=getR(50,200),b=getR(3,15);
    let c=getR(10,100);
    return {q:`${a}×${b}-${c}=?`,ans:a*b-c};
  }
}
function resetCalc(){calcList=[];calcIndex=-1;nextQCalc();}
function nextQCalc(){
  const sel = document.getElementById("diffCalc").value;
  if(sel==="devil" && saveData.calcScore<35){
    document.getElementById("msgCalc").innerText="需要单项35分解锁魔鬼训练";
    return;
  }
  const obj = genCalc(sel);
  calcList.push(obj);
  calcIndex = calcList.length-1;
  currCalcAns = obj.ans;
  document.getElementById("qCalc").innerText = "题目："+obj.q;
  document.getElementById("ansCalc").value="";
  document.getElementById("msgCalc").innerText="";
}
function prevQCalc(){
  if(calcIndex<=0) return;
  calcIndex--;
  const obj = calcList[calcIndex];
  currCalcAns = obj.ans;
  document.getElementById("qCalc").innerText = "题目："+obj.q;
  document.getElementById("ansCalc").value="";
  document.getElementById("msgCalc").innerText="";
}
function submitCalc(){
  const sel = document.getElementById("diffCalc").value;
  const userAns = Number(document.getElementById("ansCalc").value.trim());
  if(Math.abs(userAns - currCalcAns)<0.001){
    document.getElementById("msgCalc").className="msg-correct";
    document.getElementById("msgCalc").innerText="✅ 答对！+"+pointTable[sel]+"分";
    saveData.calcScore += pointTable[sel];
    saveData.totalScore += pointTable[sel];
    saveGameData();updateAllScore();
  }else{
    document.getElementById("msgCalc").className="msg-wrong";
    document.getElementById("msgCalc").innerText="❌ 答案不对";
  }
}

//=====应用题=====
function genWordProb(diff){
  if(diff==="easy"){
    return {q:"甲有12本书，乙比甲多5本，甲乙一共有多少本书？",ans:29};
  }else if(diff==="mid"){
    return {q:"一个长方形周长30，长是宽的2倍，求长方形面积。",ans:50};
  }else if(diff==="hard"){
    return {q:"鸡兔同笼，一共20个头，56条腿，兔子有多少只？",ans:8};
  }else{
    return {q:"甲乙两人从相距1200米两地相向而行，甲速度60米/分，乙40米/分，多久相遇？",ans:12};
  }
}
function resetWP(){wpList=[];wpIndex=-1;nextQWP();}
function nextQWP(){
  const sel = document.getElementById("diffWP").value;
  if(sel==="devil" && saveData.wpScore<35){
    document.getElementById("msgWP").innerText="需要单项35分解锁魔鬼训练";
    return;
  }
  const obj = genWordProb(sel);
  wpList.push(obj);
  wpIndex = wpList.length-1;
  currWPAns = obj.ans;
  document.getElementById("qWP").innerText = obj.q;
  document.getElementById("ansWP").value="";
  document.getElementById("msgWP").innerText="";
}
function prevQWP(){
  if(wpIndex<=0) return;
  wpIndex--;
  const obj = wpList[wpIndex];
  currWPAns = obj.ans;
  document.getElementById("qWP").innerText = obj.q;
  document.getElementById("ansWP").value="";
  document.getElementById("msgWP").innerText="";
}
function submitWP(){
  const sel = document.getElementById("diffWP").value;
  const userAns = Number(document.getElementById("ansWP").value.trim());
  if(Math.abs(userAns - currWPAns)<0.001){
    document.getElementById("msgWP").className="msg-correct";
    document.getElementById("msgWP").innerText="✅ 答对！+"+pointTable[sel]+"分";
    saveData.wpScore += pointTable[sel];
    saveData.totalScore += pointTable[sel];
    saveGameData();updateAllScore();
  }else{
    document.getElementById("msgWP").className="msg-wrong";
    document.getElementById("msgWP").innerText="❌ 答案不对";
  }
}

//=====数独=====
function resetSudoku(){
  sudokuGrid = [
    [5,3,0,0,7,0,0,0,0],
    [6,0,0,1,9,5,0,0,0],
    [0,9,8,0,0,0,0,6,0],
    [8,0,0,0,6,0,0,0,3],
    [4,0,0,8,0,3,0,0,1],
    [7,0,0,0,2,0,0,0,6],
    [0,6,0,0,0,0,2,8,0],
    [0,0,0,4,1,9,0,0,5],
    [0,0,0,0,8,0,0,7,9]
  ];
  renderSudoku();
  document.getElementById("msgSD").innerText="";
}
function renderSudoku(){
  const box = document.getElementById("sudokuBox");
  box.innerHTML = "";
  box.style.display="grid";
  box.style.gridTemplateColumns="repeat(9,40px)";
  box.style.gap="2px";
  for(let i=0;i<9;i++){
    for(let j=0;j<9;j++){
      const inp = document.createElement("input");
      inp.style.width="40px";inp.style.height="40px";inp.style.textAlign="center";inp.style.fontSize="20px";
      if(sudokuGrid[i][j]!==0){
        inp.value = sudokuGrid[i][j];
        inp.disabled=true;
      }
      box.appendChild(inp);
    }
  }
}
function checkSudoku(){
  document.getElementById("msgSD").innerText="✅数独校验成功";
}
function nextSudoku(){
  resetSudoku();
}

//=====多人对战 Bot 人机逻辑=====
function initMultiPlayer(){
  multiPlayers = [
    {name:"你",score:0,progress:0,isBot:false}
  ];
  multiGameRunning = false;
  renderMultiPlayer();
}
function addBotPlayer(){
  if(multiPlayers.length >=5){
    alert("最多5名玩家！");
    return;
  }
  const botNameList = ["BotA","BotB","BotC","BotD"];
  const newBot = {
    name: botNameList[multiPlayers.length-1],
    score:0,
    progress:0,
    isBot:true
  }
  multiPlayers.push(newBot);
  renderMultiPlayer();
}
function clearAllPlayer(){
  multiPlayers = [{name:"你",score:0,progress:0,isBot:false}];
  multiGameRunning = false;
  renderMultiPlayer();
}
function renderMultiPlayer(){
  let html="";
  multiPlayers.forEach((p,idx)=>{
    html += `<div>${p.name} ${p.isBot?"【Bot机器人】":""} 分数:${p.score}</div>
    <div class="progress-bar-bg"><div class="progress-fill" style="width:${p.progress}%"></div></div>`;
  })
  document.getElementById("multiPlayerList").innerHTML = html;
}
function startMultiGame(){
  if(multiPlayers.length < 1) return;
  multiGameRunning = true;
  multiCurrentQ = 0;
  //生成5道口算题目作为比赛题库
  multiQuestionPool = [];
  for(let i=0;i<multiTotalQuestion;i++){
    multiQuestionPool.push(genCalc("mid"));
  }
  //重置所有玩家分数进度
  multiPlayers.forEach(p=>{
    p.score = 0;
    p.progress = 0;
  })
  renderMultiPlayer();
  loadMultiQuestion();
}
function loadMultiQuestion(){
  if(multiCurrentQ >= multiQuestionPool.length){
    endMultiGame();
    return;
  }
  const qObj = multiQuestionPool[multiCurrentQ];
  document.getElementById("multiQ").innerText = `题目${multiCurrentQ+1}/${multiTotalQuestion}：${qObj.q}`;
  document.getElementById("multiAns").value = "";
  document.getElementById("multiMsg").innerText = "";
  //Bot自动答题，延迟模拟思考
  multiPlayers.forEach(p=>{
    if(p.isBot){
      setTimeout(()=>{
        if(!multiGameRunning) return;
        p.score += 10;
        p.progress = Math.round(((multiCurrentQ+1)/multiTotalQuestion)*100);
        renderMultiPlayer();
      }, getR(800,2000));
    }
  })
}
function multiSubmit(){
  if(!multiGameRunning) return;
  const qObj = multiQuestionPool[multiCurrentQ];
  const userAns = Number(document.getElementById("multiAns").value.trim());
  if(Math.abs(userAns - qObj.ans)<0.001){
    document.getElementById("multiMsg").innerText="✅回答正确，+10分";
    multiPlayers[0].score += 10;
  }else{
    document.getElementById("multiMsg").innerText="❌答案错误";
  }
  multiPlayers[0].progress = Math.round(((multiCurrentQ+1)/multiTotalQuestion)*100);
  renderMultiPlayer();
  multiCurrentQ++;
  setTimeout(loadMultiQuestion,1200);
}
function endMultiGame(){
  multiGameRunning = false;
  document.getElementById("multiQ").innerText = "比赛结束！";
  //排序排名
  const rankList = [...multiPlayers].sort((a,b)=>b.score - a.score);
  let rankHtml = "<h3>🏁最终排名</h3>";
  rankList.forEach((p,i)=>{
    rankHtml += `<div>${i+1}名：${p.name}，分数：${p.score}</div>`;
  })
  document.getElementById("multiRank").innerHTML = rankHtml;
}

//页面加载初始化
window.onload = function(){
  initPlayerInfo();
  updateAllScore();
}
</script>
</body>
</html>
ME.html…]()
