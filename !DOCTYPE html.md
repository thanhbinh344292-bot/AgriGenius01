<!DOCTYPE html>  
<html lang="vi">  
<head>  
  <meta charset="UTF-8">  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
  <title>AgriGenius</title>  
  <style>  
    body { font-family: 'Roboto', sans-serif; background: #e8f5e9; text-align: center; padding: 15px; }  
    h1 { color: #4CAF50; font-size: 32px; }  
    button { margin: 5px; padding: 10px 20px; border-radius: 8px; border: none; background: #4CAF50; color: white; font-size: 16px; cursor: pointer; }  
    #messages { height: 250px; overflow-y: auto; background: #fff; padding: 10px; margin: 10px 0; border-radius: 12px; text-align: left; }  
    input { width: 70%; padding: 10px; border-radius: 5px; border: 1px solid #ccc; font-size: 16px; }  
    #taskList li { margin: 5px 0; font-size: 16px; }  
  </style>  
</head>  
<body>  
  
<script>  
let pass = prompt("Nhập mật khẩu để mở AgriGenius:");  
if(pass !== "abcd"){  
  alert("Sai mật khẩu! Không được mở.");  
  document.body.innerHTML="<h1>Không có quyền truy cập</h1>";  
}  
</script>  
  
<h1>🌱 AgriGenius</h1>  
  
<div>  
  <button onclick="addTask('Tưới nước')">💧 Tưới nước</button>  
  <button onclick="addTask('Bón phân')">🌾 Bón phân</button>  
  <button onclick="addTask('Kiểm tra sâu bệnh')">🐛 Kiểm tra sâu bệnh</button>  
</div>  
  
<h3>📋 Việc hôm nay</h3>  
<ul id="taskList"></ul>  
  
<h3>💬 Chat với trợ lý</h3>  
<div id="messages"></div>  
<input id="input" placeholder="Nhập câu hỏi..." />  
<button onclick="send()">Gửi</button>  
  
<script>  
const messages = document.getElementById("messages");  
const taskList = document.getElementById("taskList");  
let tasks = [];  
  
function speak(text) {  
  const u = new SpeechSynthesisUtterance(text);  
  u.lang = "vi-VN";  
  speechSynthesis.speak(u);  
}  
  
function addMessage(text, isBot=true) {  
  const div = document.createElement("div");  
  div.innerText = (isBot ? "🤖 " : "🧑 ") + text;  
  div.style.margin = "5px 0";  
  messages.appendChild(div);  
  messages.scrollTop = messages.scrollHeight;  
  if(isBot) speak(text);  
}  
  
function renderTasks() {  
  taskList.innerHTML = "";  
  tasks.forEach((t)=>{  
    const li = document.createElement("li");  
    const btn = document.createElement("button");  
    btn.innerText = t.done ? "✅" : "⬜";  
    btn.onclick = ()=>{ t.done=!t.done; renderTasks(); if(t.done) addMessage(`Xong việc ${t.name} rồi 👍`)};  
    li.appendChild(btn);  
    li.append(" "+t.name);  
    taskList.appendChild(li);  
  });  
}  
  
function addTask(name){  
  tasks.push({name, done:false});  
  renderTasks();  
  addMessage(`Đã thêm việc: ${name}`);  
}  
  
function send(){  
  const input = document.getElementById("input");  
  let text = input.value.trim();  
  if(!text) return;  
  input.value="";  
  addMessage(text,false);  
  
  let reply="";  
  text=text.toLowerCase();  
  
  if(text.includes("chào")) reply="Chào nhen, bữa nay khỏe hông?";  
  else if(text.includes("tưới")) reply="Nhớ tưới sáng sớm hoặc chiều mát nghen.";  
  else if(text.includes("phân")) reply="Bón vừa tay thôi, đừng bón lúc trưa nắng.";  
  else if(text.includes("mệt")||text.includes("buồn")) reply="Mệt thì nghỉ, cây còn đó mà, tui nói chuyện cho vui nè.";  
  else reply="Ừa tui nghe nè, nói thêm đi.";  
  
  addMessage(reply);  
}  
  
renderTasks();  
</script>  
  
</body>  
</html>  
