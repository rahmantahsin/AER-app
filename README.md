<!doctype html>
<html lang="bn">
<head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>Rojnamcha V3 • Goal Coach</title>
<style>
*{box-sizing:border-box}body{margin:0;background:#f5f5f1;color:#171717;font-family:Inter,system-ui,-apple-system,"Segoe UI",sans-serif}button,input,select{font:inherit}
.app{max-width:1100px;margin:auto;padding:15px}.top{display:flex;justify-content:space-between;align-items:center;margin-bottom:13px}.brand{font-size:25px;font-weight:850}.date{font-size:13px;color:#777}
.hero{display:grid;grid-template-columns:1.3fr 1fr;gap:14px}.card{background:#fff;border:1px solid #e8e8e3;border-radius:20px;padding:18px;margin-bottom:14px;box-shadow:0 5px 18px rgba(0,0,0,.035)}h1{font-size:31px;line-height:1.1;margin:8px 0}.muted{color:#777;font-size:13px}
.challenge{background:#171717;color:#fff}.challenge h2{font-size:25px;margin:6px 0}.challenge button,.primary{border:0;background:#fff;color:#111;border-radius:12px;padding:12px 15px;font-weight:800;cursor:pointer}.primary{background:#171717;color:#fff;width:100%}
.progress{height:9px;background:#eee;border-radius:9px;overflow:hidden;margin:14px 0 7px}.progress span{display:block;height:100%;width:0;background:#171717;transition:.25s}
.grid{display:grid;grid-template-columns:1.2fr .8fr;gap:14px}.title{display:flex;justify-content:space-between;align-items:center}.title h3{margin:0;font-size:17px}.add,.secondary{border:1px solid #ddd;background:#fff;border-radius:10px;padding:7px 10px;cursor:pointer}
.mission{padding:13px;border:1px solid #e5e5e0;border-radius:15px;margin:9px 0}.mission.active{border-color:#171717}.mission b{display:block;margin-bottom:5px}.check{width:22px;height:22px;border:1.5px solid #aaa;border-radius:7px;display:grid;place-items:center;cursor:pointer;float:right}.check.done{background:#171717;color:#fff;border-color:#171717}.mission.done{opacity:.55}.mission.done b{text-decoration:line-through}
.stat{display:flex;justify-content:space-between;padding:10px 0;border-bottom:1px solid #eee}.stat:last-child{border:0}.daychips{display:flex;gap:6px;overflow:auto;margin:12px 0}.chip{min-width:48px;padding:8px 7px;border:1px solid #ddd;background:#fff;border-radius:10px;text-align:center;cursor:pointer}.chip.active{background:#171717;color:#fff}.chip.done{border-style:solid}
.goalbox{display:flex;justify-content:space-between;gap:10px;align-items:center}.goalbox .icon{font-size:32px}
.modal{position:fixed;inset:0;background:rgba(0,0,0,.38);display:none;align-items:center;justify-content:center;padding:15px}.modal.show{display:flex}.box{background:#fff;width:min(470px,100%);border-radius:20px;padding:20px}.box h3{margin-top:0}.box input,.box select{width:100%;padding:11px;border:1px solid #ddd;border-radius:11px;margin:6px 0 11px}.row{display:flex;gap:8px}.row>*{flex:1}.toast{position:fixed;left:50%;bottom:70px;transform:translate(-50%,20px);background:#171717;color:#fff;padding:11px 15px;border-radius:11px;opacity:0;transition:.25s;pointer-events:none}.toast.show{opacity:1;transform:translate(-50%,0)}
.resource{border:1px solid #e7e7e2;border-radius:14px;padding:12px;margin-top:9px}.resource b{display:block}.resource a{display:inline-block;margin-top:8px;padding:8px 10px;background:#171717;color:#fff;text-decoration:none;border-radius:9px;font-size:13px}.steps{margin:9px 0 0;padding-left:20px}.time{font-size:12px;color:#777}
@media(max-width:720px){.app{padding:11px}.hero,.grid{grid-template-columns:1fr}.card{border-radius:17px;padding:15px}h1{font-size:27px}}
</style>
</head>
<body>
<div class="app">
<div class="top"><div class="brand">রোজনামচা <span style="font-weight:450">V3</span></div><div class="date" id="date"></div></div>

<div class="hero">
<section class="card">
<div class="muted">🎯 30-Day Goal</div><h1 id="goalName">English Speaking Improvement</h1>
<div class="muted" id="goalMeta">প্রতিদিন 45 মিনিট • Beginner</div>
<div class="progress"><span id="goalProgress"></span></div><div class="muted" id="goalPct"></div>
</section>
<section class="card challenge">
<div class="muted" style="color:#aaa">TODAY'S COACH</div><h2 id="coachTitle">আজকের Mission প্রস্তুত</h2><div id="coachText" style="opacity:.75;font-size:13px;margin-bottom:12px"></div>
<button onclick="startMission()">🎙️ Mission শুরু করি</button>
</section>
</div>

<section class="card">
<div class="title"><h3>📅 30-Day Journey</h3><button class="add" onclick="newGoal()">＋ New Goal</button></div>
<div class="daychips" id="days"></div>
</section>

<div class="grid">
<section class="card">
<div class="title"><h3 id="missionTitle">Day 1 Mission</h3><button class="add" onclick="resetDay()">Reset Day</button></div>
<div id="missions"></div>
</section>

<section>
<section class="card">
<div class="title"><h3>✨ এখন কী করব?</h3></div>
<div id="nextAction" style="font-size:18px;font-weight:800;margin:12px 0">Next action খোঁজা হচ্ছে...</div>
<div class="muted">একবারে একটি কাজ করুন। Complete করলে পরেরটা আসবে।</div>
</section>
<section class="card" id="resourceCard" style="display:none">
<div class="title"><h3>📚 আজকের Resources</h3><button class="add" onclick="closeResource()">×</button></div>
<div id="resourceContent"></div>
</section>
<section class="card">
<div class="title"><h3>📈 Progress</h3></div>
<div class="stat"><span>Days completed</span><b id="daysDone">0 / 30</b></div>
<div class="stat"><span>Today's tasks</span><b id="todayDone">0 / 0</b></div>
<div class="stat"><span>Consistency</span><b id="consistency">0%</b></div>
</section>
</section>
</div>
<button class="primary" onclick="openJournal()">📝 আজকের শেখা / Reflection</button>
</div>

<div class="modal" id="modal"><div class="box"><h3 id="modalTitle">30-Day Goal</h3><label>আপনার Goal</label><input id="gname" value="English Speaking Improvement"><label>প্রতিদিন কত মিনিট?</label><input id="minutes" type="number" value="45" min="5"><label>আপনার Level</label><select id="level"><option>Beginner</option><option>Intermediate</option><option>Advanced</option></select><div class="row"><button class="secondary" onclick="closeModal()">Cancel</button><button class="primary" onclick="createGoal()">Create 30-Day Plan</button></div></div></div>
<div class="modal" id="journal"><div class="box"><h3>📝 আজকের Reflection</h3><div class="muted" id="journalDay"></div><textarea id="note" style="width:100%;height:130px;padding:11px;border:1px solid #ddd;border-radius:11px;margin:10px 0;font:inherit" placeholder="আজ কী শিখলেন? কোথায় সমস্যা হলো?"></textarea><button class="primary" onclick="saveJournal()">Save Reflection</button></div></div>
<div class="toast" id="toast"></div>

<script>
const KEY='rojnamcha_v3_goalcoach';let S=JSON.parse(localStorage.getItem(KEY)||'null');
const defaultPlan=[
['নিজের পরিচয় English-এ 2 মিনিট বলুন','Speaking','2 min'],['20টি দৈনন্দিন বাক্য শিখে বলুন','Vocabulary','15 min'],['নিজের দিনের কথা English-এ বলুন','Speaking','5 min'],['10 মিনিট English শুনে 5টি বাক্য repeat করুন','Listening','10 min'],['আজকের topic নিয়ে 3 মিনিট self-talk','Speaking','5 min'],
['Question & Answer practice করুন','Conversation','10 min'],['নিজের ভুল 5টি লিখে আবার বলুন','Correction','10 min'],['একটি short English video শুনে summary বলুন','Listening','15 min'],['Family/Friend-এর সাথে 5 মিনিট English বলুন','Conversation','5 min'],['আজ 30টি useful phrase revise করুন','Vocabulary','15 min'],
['নিজের মতামত 3 মিনিট বলুন','Speaking','5 min'],['Picture দেখে English-এ describe করুন','Speaking','5 min'],['5 মিনিট story বলুন','Speaking','5 min'],['English-এ প্রশ্ন তৈরি করে নিজেই উত্তর দিন','Conversation','10 min'],['গত 14 দিনের শব্দ দিয়ে 10টি বাক্য বলুন','Vocabulary','15 min'],
['5 মিনিট না থেমে English বলুন','Speaking','5 min'],['একটি news/topic শুনে 5টি point বলুন','Listening','15 min'],['Role-play: দোকান/restaurant conversation','Conversation','10 min'],['নিজের কাজ/পেশা নিয়ে 5 মিনিট বলুন','Speaking','5 min'],['10 মিনিট English conversation simulation','Conversation','10 min'],
['Grammar না থেমে fluently বলার চেষ্টা করুন','Fluency','10 min'],['একটি topic-এ 7 মিনিট কথা বলুন','Speaking','7 min'],['শুনে সঙ্গে সঙ্গে repeat করুন','Listening','15 min'],['আজকের সব common mistake ঠিক করুন','Correction','15 min'],['Random topic: 5 মিনিট speaking','Speaking','5 min'],
['একজন বন্ধুকে English-এ 5টি প্রশ্ন করুন','Conversation','10 min'],['নিজের future plans নিয়ে 7 মিনিট বলুন','Speaking','7 min'],['10-minute conversation challenge','Conversation','10 min'],['নিজের Day 1 recording-এর সাথে compare করুন','Review','15 min'],['🎤 Final 10-minute speaking test','Final Test','10 min']
];

const resourceLibrary={
  Speaking:[
    {title:'Self-introduction template',kind:'📝 Template',time:'5 min',url:'https://learnenglish.britishcouncil.org/skills/speaking',desc:'নিজের নাম, কাজ/পড়াশোনা, আগ্রহ ও future plan নিয়ে 5টি simple sentence তৈরি করুন।'},
    {title:'Speaking practice',kind:'🗣️ Practice',time:'5 min',url:'https://www.youtube.com/results?search_query=beginner+english+self+introduction+speaking+practice',desc:'একবার শুনুন, তারপর না দেখে একই idea নিজের ভাষায় বলুন।'}
  ],
  Vocabulary:[
    {title:'Everyday English phrases',kind:'📚 Lesson',time:'10 min',url:'https://learnenglish.britishcouncil.org/vocabulary',desc:'10টি useful phrase বেছে নিয়ে প্রতিটির সঙ্গে নিজের একটি sentence বানান।'},
    {title:'Phrase repetition',kind:'🎧 Practice',time:'5 min',url:'https://www.youtube.com/results?search_query=english+daily+phrases+beginner',desc:'প্রতিটি phrase 3 বার জোরে বলুন।'}
  ],
  Listening:[
    {title:'British Council listening practice',kind:'🎧 Lesson',time:'10 min',url:'https://learnenglish.britishcouncil.org/skills/listening',desc:'একবার শুনুন, দ্বিতীয়বার keywords লিখুন, তৃতীয়বার pause করে repeat করুন।'}
  ],
  Conversation:[
    {title:'Speaking conversation practice',kind:'🗣️ Practice',time:'10 min',url:'https://learnenglish.britishcouncil.org/skills/speaking',desc:'প্রথমে প্রশ্নগুলো পড়ুন, তারপর উত্তর না দেখে বলুন।'}
  ],
  Correction:[
    {title:'Grammar reference',kind:'📖 Reference',time:'10 min',url:'https://learnenglish.britishcouncil.org/grammar',desc:'আজ নিজের 5টি ভুল sentence লিখে correct version তৈরি করুন।'}
  ],
  Fluency:[
    {title:'Fluency speaking practice',kind:'🗣️ Practice',time:'10 min',url:'https://learnenglish.britishcouncil.org/skills/speaking',desc:'থেমে গেলে পুরো sentence বন্ধ করবেন না। সহজ শব্দ দিয়ে idea চালিয়ে যান।'}
  ],
  Review:[
    {title:'Compare your speaking',kind:'🎤 Review',time:'15 min',url:'https://learnenglish.britishcouncil.org/skills/speaking',desc:'Day 1 recording-এর সঙ্গে আজকের recording compare করুন: clarity, vocabulary, confidence।'}
  ],
  'Final Test':[
    {title:'Final speaking challenge',kind:'🏆 Test',time:'10 min',url:'https://learnenglish.britishcouncil.org/skills/speaking',desc:'10 মিনিট একটি topic নিয়ে কথা বলুন। তারপর নিজের recording শুনে 3টি improvement point লিখুন।'}
  ]
};
function showResource(day,index){
  const x=S.plan.find(a=>a.day===day), resources=resourceLibrary[x.type]||resourceLibrary.Speaking;
  resourceContent.innerHTML=`<div style="font-weight:800;font-size:18px;margin-bottom:5px">${x.title}</div>
  <div class="muted">আজকের learning path: Resource → Practice → Complete</div>
  ${resources.map(r=>`<div class="resource"><b>${r.kind} ${r.title}</b><div class="time">⏱️ ${r.time}</div><div style="font-size:13px;margin-top:5px">${r.desc}</div><a href="${r.url}" target="_blank" rel="noopener">Open resource ↗</a></div>`).join('')}
  <div style="margin-top:12px;font-weight:750">🎯 Action steps</div>
  <ol class="steps"><li>Resource ব্যবহার করুন</li><li>নিজের ভাষায় practice করুন</li><li>সম্ভব হলে voice record করুন</li><li>তারপর Mission complete করুন ✅</li></ol>`;
  resourceCard.style.display='block'; resourceCard.scrollIntoView({behavior:'smooth',block:'center'});
}
function closeResource(){resourceCard.style.display='none'}
function build(){let old=S?.plan; if(!S){S={name:'English Speaking Improvement',minutes:45,level:'Beginner',day:1,done:{},notes:{},plan:defaultPlan.map((x,i)=>({day:i+1,title:x[0],type:x[1],time:x[2]}))}}else if(!S.plan||S.plan.length!==30)S.plan=defaultPlan.map((x,i)=>({day:i+1,title:x[0],type:x[1],time:x[2]}));save(false)}
function save(r=true){localStorage.setItem(KEY,JSON.stringify(S));if(r)render()}
function dayComplete(d){return (S.done[d]||[]).length>=3}
function render(){
date.textContent=new Date().toLocaleDateString('bn-BD',{weekday:'long',day:'numeric',month:'long',year:'numeric'});
goalName.textContent=S.name;goalMeta.textContent=`প্রতিদিন ${S.minutes} মিনিট • ${S.level}`;
let completedDays=0;for(let d=1;d<=30;d++)if(dayComplete(d))completedDays++;
let pct=Math.round(completedDays/30*100);goalProgress.style.width=pct+'%';goalPct.textContent=`${completedDays} / 30 দিন complete • ${pct}%`;
days.innerHTML=S.plan.map((x,i)=>`<button class="chip ${S.day===x.day?'active':''} ${dayComplete(x.day)?'done':''}" onclick="selectDay(${x.day})">D${x.day}<br>${dayComplete(x.day)?'✓':'○'}</button>`).join('');
let plan=S.plan.filter(x=>x.day===S.day);missionTitle.textContent=`Day ${S.day} Mission`;let done=S.done[S.day]||[];
missions.innerHTML=plan.map((x,i)=>`<div class="mission ${done.includes(i)?'done':''}" onclick="showResource(${S.day},${i})"><div class="check ${done.includes(i)?'done':''}" onclick="event.stopPropagation();toggleMission(${i})">${done.includes(i)?'✓':''}</div><b>${x.title}</b><span class="muted">${x.type} • ${x.time}</span><div class="muted" style="margin-top:7px">📚 Resources & action steps →</div></div>`).join('');
let next=plan.findIndex((x,i)=>!done.includes(i));if(next>=0){nextAction.textContent=plan[next].title;coachTitle.textContent=`Day ${S.day}: ${plan[next].type}`;coachText.textContent=`সময়: ${plan[next].time} • আজকের next best action`;}else{nextAction.textContent=S.day<30?'🎉 আজকের Mission complete! কাল Day '+(S.day+1)+' শুরু হবে।':'🏆 30-Day Challenge complete!';coachTitle.textContent='Mission Complete 🎉';coachText.textContent='আজকের সব 3টি action শেষ করেছেন।';}
daysDone.textContent=`${completedDays} / 30`;todayDone.textContent=`${done.length} / ${plan.length}`;consistency.textContent=`${pct}%`;
}
function selectDay(d){S.day=d;save()}
function toggleMission(i){let a=S.done[S.day]||[];if(a.includes(i))a=a.filter(x=>x!==i);else a.push(i);S.done[S.day]=a;save();toast(a.includes(i)?'Mission complete! 🎉':'Mission pending');}
function startMission(){let plan=S.plan.filter(x=>x.day===S.day),done=S.done[S.day]||[],i=plan.findIndex((x,j)=>!done.includes(j));if(i<0)return toast('আজকের সব Mission শেষ! 🏆');toast('Start: '+plan[i].title);document.querySelectorAll('.mission')[i]?.scrollIntoView({behavior:'smooth',block:'center'})}
function resetDay(){if(confirm('এই দিনের completion reset করবেন?')){delete S.done[S.day];save();toast('Day reset হয়েছে')}}
function newGoal(){openModal()}
function openModal(){modal.classList.add('show')};function closeModal(){modal.classList.remove('show')}
function createGoal(){let n=gname.value.trim();if(!n)return toast('Goal লিখুন');S={name:n,minutes:+minutes.value||45,level:level.value,day:1,done:{},notes:{},plan:defaultPlan.map((x,i)=>({day:i+1,title:adapt(x[0],n),type:x[1],time:x[2]}))};closeModal();save();toast('আপনার 30-Day Plan তৈরি হয়েছে 🚀')}
function adapt(t,n){if(n.toLowerCase().includes('english'))return t;return t.replace(/English/gi,n)}
function openJournal(){journalDay.textContent=`Day ${S.day} • ${S.name}`;note.value=S.notes[S.day]||'';journal.classList.add('show')}
function saveJournal(){S.notes[S.day]=note.value;journal.classList.remove('show');save();toast('Reflection saved ❤️')}
function toast(t){let x=document.getElementById('toast');x.textContent=t;x.classList.add('show');setTimeout(()=>x.classList.remove('show'),1900)}
build();render();
</script>
</body></html>
