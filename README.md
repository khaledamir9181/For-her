# For-her
<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">

<title>90 Days of Us ❤️</title>

<style>

*{box-sizing:border-box}

body{margin:0;min-height:100vh;font-family:Arial,sans-serif;color:#704957;background:#fff0f6}

body:before{content:"";position:fixed;inset:0;background-image:var(--bg);background-size:cover;background-position:center;z-index:-2;transition:background-image .5s}

body:after{content:"";position:fixed;inset:0;background:linear-gradient(rgba(255,240,246,.38),rgba(255,235,244,.70));z-index:-1}

.container{min-height:100vh;padding:28px 18px 110px;text-align:center;display:flex;flex-direction:column;align-items:center}

.decor{font-size:24px;letter-spacing:5px}.title{font-size:32px;font-weight:bold;color:#d65382;margin:10px 0}

.tag{color:#9c7181;margin-bottom:14px}.day{font-weight:bold;color:#d65382;background:rgba(255,255,255,.78);padding:7px 15px;border-radius:999px;margin-bottom:18px}

button{border:0;border-radius:999px;padding:14px 26px;background:#ff9fc4;color:white;font-size:16px;font-weight:bold;box-shadow:0 8px 22px rgba(200,92,134,.18)}

.message{display:none;width:min(560px,92vw);margin-top:18px;padding:22px;background:rgba(255,248,251,.15);backdrop-filter:blur(4px);-webkit-backdrop-filter:blur(4px);border:1px solid rgba(255,213,228,0.6);border-radius:14px;text-align:left;color:#704957;white-space:pre-wrap}

.bottom{position:fixed;left:10px;right:10px;bottom:10px;padding:10px;background:rgba(255,250,252,.90);border-radius:18px;text-align:center;font-size:14px;color:#9c7181;cursor:pointer}

.count{margin-left:12px;font-variant-numeric:tabular-nums}.hint{display:block;font-size:11px;margin-top:4px;opacity:.75}

</style>

</head>

<body>

<div class="container">

<div class="decor">🌸 🎀 🐱 💗 🌸</div>

<div class="title">90 Days of Us</div>

<div class="tag">You will be chosen every day ❤️✨️</div>

<div class="day" id="day"></div>

<button id="today">💌 Today's Message</button><button id="musicBtn" style="display:none;margin-top:10px;background:rgba(255,255,255,.72);color:#c85c86;border:1px solid #ffd5e4;box-shadow:none;font-size:14px;padding:10px 16px;border-radius:999px">🎵 Play Music</button>

<div id="msg" class="message"></div><audio id="audio" loop preload="auto"></audio>

</div>

<div class="bottom" id="tomorrow">

🌷 <b>Tomorrow's Message</b><span class="count" id="count">00:00:00</span>

<span class="hint" id="hint">Tap after 8:00 AM Egypt time for the next day</span>

</div>

<script>

const KEY="jenna_90_days_editor_v6";

const FALLBACK_MESSAGE=`Hiii baby ❤️

So this is the first note, and well… it’s gonna be a little longer than the others. 🥹

This morning, as always, I woke up and chose you — as my partner, as my wife, and as the person I want by my side for life. You’ve become a part of me now, and I can see it everywhere. I can feel [...]

The whole point of these notes is that, in case we don’t get the chance to talk for any reason — whether it’s family, work, or even if one of us is pissed at the other 😂 — you can always co[...]

No matter what happens, no matter how far we are, and no matter what kind of day we’re having, I want you to always have a little piece of my love waiting for you here. ❤️🌹

That’s it for the first one. I’m gonna try to make the others even better! 🥹

Love you, and happy birthday, Jenna. ❤️✨️`;

const FALLBACK_BG="";

const FALLBACK_SONG="";

const TZ="Africa/Cairo";

const START="2026-08-20T08:00:00+02:00";

function getData(){

  try{

    const saved=localStorage.getItem(KEY);

    if(saved){

      const d=JSON.parse(saved);

      if(Array.isArray(d.messages)&&Array.isArray(d.backgrounds))

        return {

          messages:d.messages,

          backgrounds:d.backgrounds,

          songs:Array.isArray(d.songs)?d.songs:Array(90).fill("")

        };

    }

  }catch(e){}

  return {messages:[FALLBACK_MESSAGE],backgrounds:[FALLBACK_BG],songs:[FALLBACK_SONG]};

}

function egyptParts(){

  const p=new Intl.DateTimeFormat("en-US",{timeZone:TZ,year:"numeric",month:"2-digit",day:"2-digit",hour:"2-digit",minute:"2-digit",second:"2-digit",hour12:false}).formatToParts(new Date());

  const o={};p.forEach(x=>{if(x.type!=="literal")o[x.type]=x.value});

  return {y:+o.year,m:+o.month,d:+o.day,h:+o.hour,min:+o.minute,s:+o.second};

}

function dayIndex(){

  const e=egyptParts();

  const now=Date.UTC(e.y,e.m-1,e.d,e.h,e.min,e.s);

  const start=new Date(START).getTime();

  return Math.max(0,Math.min(89,Math.floor((now-start)/86400000)));

}

function next8(){

  const e=egyptParts();

  let d=new Date(Date.UTC(e.y,e.m-1,e.d,8,0,0));

  if(e.h>=8)d.setUTCDate(d.getUTCDate()+1);

  return d;

}

function refresh(){

  const data=getData(), i=dayIndex();

  document.getElementById("day").textContent="Day "+(i+1)+" / 90";

  const bg=data.backgrounds?.[i]||"";

  document.body.style.setProperty("--bg",bg?`url("${bg}")`:"none");

  let diff=Math.max(0,next8()-Date.now()),t=Math.floor(diff/1000);

  document.getElementById("count").textContent=

    String(Math.floor(t/3600)).padStart(2,"0")+":"+

    String(Math.floor(t%3600/60)).padStart(2,"0")+":"+

    String(t%60).padStart(2,"0");

  document.getElementById("hint").textContent=i<89?

    "Tap after 8:00 AM Egypt time for the next day":"You reached Day 90 ❤️";

}

function show(i){

  const data=getData();

  document.getElementById("day").textContent="Day "+(i+1)+" / 90";

  document.body.style.setProperty("--bg",data.backgrounds?.[i]?`url("${data.backgrounds[i]}")`:"none");

  const msg=document.getElementById("msg");

  msg.style.opacity="0"; msg.style.transform="translateY(14px)";

  msg.style.display="block";

  msg.textContent=data.messages?.[i]||"A little reminder that I love you. ❤️";

  requestAnimationFrame(()=>requestAnimationFrame(()=>{msg.style.opacity="1";msg.style.transform="translateY(0)";}));

  const audio=document.getElementById("audio"), musicBtn=document.getElementById("musicBtn");

  audio.pause(); audio.currentTime=0;

  const song=data.songs?.[i]||"";

  if(song){

    audio.src=song;

    musicBtn.style.display="inline-block";

    musicBtn.textContent="🎵 Play Music";

    audio.play().then(()=>{musicBtn.textContent="⏸️ Pause Music";}).catch(()=>{});

  }else{

    audio.removeAttribute("src"); audio.load();

    musicBtn.style.display="none";

  }

}

document.getElementById("musicBtn").onclick=()=>{

  const audio=document.getElementById("audio"), btn=document.getElementById("musicBtn");

  if(audio.paused){audio.play();btn.textContent="⏸️ Pause Music";}

  else{audio.pause();btn.textContent="🎵 Play Music";}

};

document.getElementById("today").onclick=()=>show(dayIndex());

document.getElementById("tomorrow").onclick=()=>{

  const i=dayIndex();

  if(i<89 && Date.now()>=next8().getTime()) show(i+1);

};

refresh();

setInterval(refresh,1000);

// If you edit the private editor in another tab, this refreshes the viewer.

window.addEventListener("storage",refresh);

</script>

</body>

</html>