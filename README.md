# 🎮 Super Markdown Pelikeskus

<style>

body{
font-family:Arial;
background:#0f172a;
color:white;
text-align:center;
}

section{
background:#1e293b;
padding:20px;
margin:20px auto;
width:350px;
border-radius:12px;
box-shadow:0 0 10px black;
}

button{
padding:8px 12px;
margin:4px;
border:none;
border-radius:6px;
background:#38bdf8;
cursor:pointer;
}

button:hover{
background:#0ea5e9;
}

canvas{
background:black;
margin-top:10px;
border-radius:6px;
}

</style>

# 🧮 Laskin

<section>

<input id="calc" readonly style="width:200px;font-size:18px">

<br>

<button onclick="add('7')">7</button> <button onclick="add('8')">8</button> <button onclick="add('9')">9</button> <button onclick="add('/')">÷</button>

<br>

<button onclick="add('4')">4</button> <button onclick="add('5')">5</button> <button onclick="add('6')">6</button> <button onclick="add('*')">×</button>

<br>

<button onclick="add('1')">1</button> <button onclick="add('2')">2</button> <button onclick="add('3')">3</button> <button onclick="add('-')">−</button>

<br>

<button onclick="add('0')">0</button> <button onclick="calcRes()">=</button> <button onclick="calcClear()">C</button> <button onclick="add('+')">+</button>

</section>

---

# 🧠 Tietovisa

<section>

<p id="q"></p>
<div id="a"></div>
<p id="score"></p>

</section>

---

# 🐍 Snake

<section>

<p>Pisteet: <span id="snakeScore">0</span></p>

<canvas id="snake" width="320" height="320"></canvas>

</section>

---

# 🧱 Breakout

<section>

<canvas id="breakout" width="320" height="240"></canvas>

</section>

---

# 🎨 Piirto-ohjelma

<section>

<canvas id="draw" width="300" height="200"></canvas>

<br>

<button onclick="clearDraw()">Tyhjennä</button>

</section>

<script>

/* LASKIN */

function add(v){
calc.value+=v
}

function calcRes(){
calc.value=eval(calc.value)
}

function calcClear(){
calc.value=""
}

/* TIETOVISA */

let questions=[
{q:"2+2?",a:[3,4,5],c:4},
{q:"Suomen pääkaupunki?",a:["Turku","Helsinki","Oulu"],c:"Helsinki"},
{q:"3*3?",a:[6,9,12],c:9}
]

let qi=0
let sc=0

function showQ(){

let qu=questions[qi]

q.innerText=qu.q
a.innerHTML=""

qu.a.forEach(v=>{

let b=document.createElement("button")
b.innerText=v

b.onclick=function(){

if(v==qu.c){
sc++
alert("Oikein!")
}else{
alert("Väärin")
}

qi++

if(qi<questions.length){
showQ()
}else{
q.innerText="Valmis!"
a.innerHTML=""
score.innerText="Pisteet: "+sc
}

}

a.appendChild(b)

})

}

showQ()

/* SNAKE */

const s=document.getElementById("snake")
const sctx=s.getContext("2d")

let snake=[{x:10,y:10}]
let food={x:5,y:5}
let dir="RIGHT"

document.addEventListener("keydown",e=>{
if(e.key=="ArrowUp")dir="UP"
if(e.key=="ArrowDown")dir="DOWN"
if(e.key=="ArrowLeft")dir="LEFT"
if(e.key=="ArrowRight")dir="RIGHT"
})

function snakeGame(){

let h={...snake[0]}

if(dir=="UP")h.y--
if(dir=="DOWN")h.y++
if(dir=="LEFT")h.x--
if(dir=="RIGHT")h.x++

snake.unshift(h)

if(h.x==food.x&&h.y==food.y){

food={
x:Math.floor(Math.random()*16),
y:Math.floor(Math.random()*16)
}

snakeScore.innerText=parseInt(snakeScore.innerText)+1

}else{
snake.pop()
}

sctx.clearRect(0,0,320,320)

sctx.fillStyle="lime"

snake.forEach(p=>{
sctx.fillRect(p.x*20,p.y*20,18,18)
})

sctx.fillStyle="red"
sctx.fillRect(food.x*20,food.y*20,18,18)

}

setInterval(snakeGame,120)

/* BREAKOUT */

const b=document.getElementById("breakout")
const bctx=b.getContext("2d")

let bx=160
let by=120
let vx=3
let vy=3
let paddleX=120

document.addEventListener("mousemove",e=>{
paddleX=e.offsetX
})

function breakoutGame(){

bx+=vx
by+=vy

if(bx<0||bx>320)vx=-vx
if(by<0)vy=-vy

if(by>220&&bx>paddleX&&bx<paddleX+80){
vy=-vy
}

bctx.clearRect(0,0,320,240)

bctx.fillRect(paddleX,220,80,10)

bctx.beginPath()
bctx.arc(bx,by,8,0,Math.PI*2)
bctx.fill()

}

setInterval(breakoutGame,20)

/* PIIRTO */

const d=document.getElementById("draw")
const dctx=d.getContext("2d")

let drawing=false

d.onmousedown=()=>drawing=true
d.onmouseup=()=>drawing=false

d.onmousemove=e=>{

if(!drawing)return

dctx.fillStyle="white"
dctx.fillRect(e.offsetX,e.offsetY,3,3)

}

function clearDraw(){
dctx.clearRect(0,0,300,200)
}

</script>
