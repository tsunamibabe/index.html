<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Бюджет</title>

<style>
body{
font-family:Arial;
background:#111;
color:white;
padding:20px;
}
.card{
background:#222;
padding:15px;
border-radius:10px;
margin-bottom:10px;
}
input{
width:100%;
padding:10px;
margin:5px 0;
}
</style>
</head>

<body>

<h2>💸 Бюджет</h2>

<div class="card">
<h3>Кредитка</h3>
<input id="goal" placeholder="Цель">
<input id="paid" placeholder="Внесено">
<p id="left"></p>
</div>

<div class="card">
<h3>План</h3>
<div id="plan"></div>
</div>

<script>

let data = JSON.parse(localStorage.getItem("budget")) || {
goal:131000,
paid:0,
plan:[
{d:"10",t:"Квартира",a:25000},
{d:"15",t:"Сбер",a:4486},
{d:"25",t:"Коммуналка",a:5500}
]
};

function save(){
localStorage.setItem("budget",JSON.stringify(data));
render();
}

function render(){

document.getElementById("goal").value=data.goal;
document.getElementById("paid").value=data.paid;

document.getElementById("left").innerText =
"Осталось: "+(data.goal-data.paid);

let plan=document.getElementById("plan");
plan.innerHTML="";

data.plan.forEach((x,i)=>{
let div=document.createElement("div");

div.innerHTML=`
<b>${x.d}</b> ${x.t}
<input value="${x.a}" 
oninput="update(${i},this.value)">
`;

plan.appendChild(div);
});
}

function update(i,v){
data.plan[i].a=Number(v)||0;
save();
}

document.getElementById("goal").oninput=e=>{
data.goal=Number(e.target.value)||0;
save();
};

document.getElementById("paid").oninput=e=>{
data.paid=Number(e.target.value)||0;
save();
};

render();

</script>

</body>
</html>