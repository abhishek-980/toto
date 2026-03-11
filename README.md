# toto
todo list
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Advanced ToDo Tracker</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
font-family:Arial;
background:#f3f4f6;
display:flex;
justify-content:center;
padding:40px;
}

.container{
background:white;
padding:25px;
width:650px;
border-radius:10px;
box-shadow:0 8px 20px rgba(0,0,0,0.15);
}

h1{
text-align:center;
}

.input-section{
display:flex;
gap:10px;
margin-bottom:20px;
}

input{
flex:1;
padding:10px;
border:1px solid #ccc;
border-radius:5px;
}

button{
padding:8px 12px;
border:none;
background:#2563eb;
color:white;
border-radius:5px;
cursor:pointer;
}

button:hover{
background:#1e40af;
}

.complete-btn{
background:#22c55e;
}

.delete-btn{
background:#ef4444;
}

.edit-btn{
background:#f59e0b;
}

ul{
list-style:none;
padding:0;
}

li{
display:flex;
justify-content:space-between;
align-items:center;
padding:10px;
border-bottom:1px solid #eee;
}

.completed{
text-decoration:line-through;
color:gray;
}

.task-buttons button{
margin-left:5px;
}

.stats{
margin-top:20px;
padding:15px;
background:#f9fafb;
border-radius:8px;
}

canvas{
margin-top:20px;
}

</style>
</head>

<body>

<div class="container">

<h1>To-Do Work Tracker</h1>

<div class="input-section">
<input id="taskInput" placeholder="Enter new task">
<button onclick="addTask()">Add</button>
</div>

<ul id="taskList"></ul>

<div class="stats">

<h3>Work Statistics</h3>

<p>Total Tasks: <span id="totalTasks">0</span></p>
<p>Completed: <span id="completedTasks">0</span></p>
<p>Pending: <span id="pendingTasks">0</span></p>
<p>Completion Rate: <span id="completionRate">0%</span></p>

<canvas id="taskChart"></canvas>

</div>

</div>

<script>

let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
let chart;

function saveTasks(){
localStorage.setItem("tasks",JSON.stringify(tasks));
}

function addTask(){

let input=document.getElementById("taskInput");

if(input.value.trim()=="") return;

tasks.push({
text:input.value,
completed:false
});

input.value="";
saveTasks();
renderTasks();
updateStats();

}

function deleteTask(index){

tasks.splice(index,1);
saveTasks();
renderTasks();
updateStats();

}

function completeTask(index){

tasks[index].completed=true;
saveTasks();
renderTasks();
updateStats();

}

function editTask(index){

let newTask=prompt("Edit task",tasks[index].text);

if(newTask){
tasks[index].text=newTask;
saveTasks();
renderTasks();
}

}

function renderTasks(){

let list=document.getElementById("taskList");
list.innerHTML="";

tasks.forEach((task,index)=>{

let li=document.createElement("li");

li.innerHTML=`

<span class="${task.completed ? 'completed':''}">
${task.text}
</span>

<div class="task-buttons">

<button class="complete-btn" onclick="completeTask(${index})">Complete</button>

<button class="edit-btn" onclick="editTask(${index})">Edit</button>

<button class="delete-btn" onclick="deleteTask(${index})">Delete</button>

</div>

`;

list.appendChild(li);

});

}

function updateStats(){

let total=tasks.length;
let completed=tasks.filter(t=>t.completed).length;
let pending=total-completed;

document.getElementById("totalTasks").innerText=total;
document.getElementById("completedTasks").innerText=completed;
document.getElementById("pendingTasks").innerText=pending;

let rate = total ? Math.round((completed/total)*100) : 0;
document.getElementById("completionRate").innerText=rate+"%";

drawChart(completed,pending);

}

function drawChart(completed,pending){

let ctx=document.getElementById("taskChart");

if(chart) chart.destroy();

chart=new Chart(ctx,{
type:"doughnut",
data:{
labels:["Completed","Pending"],
datasets:[{
data:[completed,pending],
backgroundColor:["#22c55e","#ef4444"]
}]
}
});

}

renderTasks();
updateStats();

</script>

</body>
</html>
