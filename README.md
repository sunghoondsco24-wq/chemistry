<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">

<title>N₂O₄ ⇌ 2NO₂ 평형 시뮬레이터</title>

<style>

body{
    font-family:Arial;
    background:#f5f7fa;
    text-align:center;
}

.container{
    max-width:1000px;
    margin:auto;
    background:white;
    padding:20px;
    border-radius:15px;
    box-shadow:0 0 15px rgba(0,0,0,.15);
}

h1{
    color:#2c3e50;
}

.control{
    margin:15px;
}

input[type=range]{
    width:300px;
}

canvas{
    border:1px solid #ccc;
    margin-top:20px;
}

.result{
    font-size:20px;
    margin-top:15px;
}

.state{
    font-size:24px;
    font-weight:bold;
}

</style>
</head>
<body>

<div class="container">

<h1>⚗️ N₂O₄(g) ⇌ 2NO₂(g)</h1>

<div class="control">
N₂O₄ 농도 :
<input id="n2o4" type="range" min="0.1" max="5" step="0.1" value="1">
<span id="n2o4Val">1.0</span> mol/L
</div>

<div class="control">
NO₂ 농도 :
<input id="no2" type="range" min="0.1" max="5" step="0.1" value="1">
<span id="no2Val">1.0</span> mol/L
</div>

<div class="control">
온도 :
<input id="temp" type="range" min="250" max="500" step="1" value="298">
<span id="tempVal">298</span> K
</div>

<div class="control">
압력 :
<input id="pressure" type="range" min="0.5" max="5" step="0.1" value="1">
<span id="pressureVal">1.0</span> atm
</div>

<div class="result">

Q =
<span id="qValue"></span>

<br><br>

K =
<span id="kValue"></span>

<br><br>

<div class="state" id="direction"></div>

</div>

<canvas id="graph" width="800" height="350"></canvas>

</div>

<script>

const n2o4Slider=document.getElementById("n2o4");
const no2Slider=document.getElementById("no2");
const tempSlider=document.getElementById("temp");
const pressureSlider=document.getElementById("pressure");

const ctx=document.getElementById("graph").getContext("2d");

function calculate(){

let N2O4=parseFloat(n2o4Slider.value);
let NO2=parseFloat(no2Slider.value);
let T=parseFloat(tempSlider.value);
let P=parseFloat(pressureSlider.value);

document.getElementById("n2o4Val").innerText=N2O4.toFixed(1);
document.getElementById("no2Val").innerText=NO2.toFixed(1);
document.getElementById("tempVal").innerText=T;
document.getElementById("pressureVal").innerText=P.toFixed(1);

let effectiveNO2=NO2*P;
let effectiveN2O4=N2O4*P;

let Q=(effectiveNO2**2)/effectiveN2O4;

let K298=0.15;

let deltaH=57000;

let R=8.314;

let K=
K298*Math.exp(
(-deltaH/R)*(1/T-1/298)
);

document.getElementById("qValue").innerText=
Q.toFixed(4);

document.getElementById("kValue").innerText=
K.toFixed(4);

let text="";

if(Math.abs(Q-K)<0.03*K){

text="✅ 평형 상태";

}
else if(Q<K){

text="➡ 정반응 진행 (NO₂ 증가)";

}
else{

text="⬅ 역반응 진행 (N₂O₄ 증가)";

}

document.getElementById("direction").innerText=text;

drawGraph(N2O4,NO2);
}

function drawGraph(n2o4,no2){

ctx.clearRect(0,0,800,350);

const values=[n2o4,no2];
const labels=["N₂O₄","NO₂"];

for(let i=0;i<2;i++){

let h=values[i]*50;

ctx.fillStyle=i==0?
"#3498db":
"#e74c3c";

ctx.fillRect(
200+i*250,
300-h,
120,
h
);

ctx.fillStyle="black";

ctx.font="20px Arial";

ctx.fillText(
labels[i],
220+i*250,
325
);

ctx.fillText(
values[i].toFixed(2),
220+i*250,
280-h
);

}

ctx.font="18px Arial";
ctx.fillText(
"농도 비교 그래프",
20,
30
);
}

document.querySelectorAll("input")
.forEach(slider=>{
slider.addEventListener(
"input",
calculate
);
});

calculate();

</script>

</body>
</html>
