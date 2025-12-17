<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>AgriGenius 4.1</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #e8f5e9;
      margin: 0;
      padding: 20px;
    }
    h1 {
      color: #2e7d32;
      text-align: center;
    }
    .card {
      background: white;
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 16px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    button {
      background: #4CAF50;
      color: white;
      border: none;
      padding: 10px 14px;
      border-radius: 8px;
      cursor: pointer;
      margin-top: 8px;
    }
    .done {
      text-decoration: line-through;
      color: gray;
    }
    input {
      width: 100%;
      padding: 10px;
      border-radius: 8px;
      border: 1px solid #ccc;
    }
    ul {
      padding-left: 20px;
    }
  </style>
</head>
<body>

<h1>🌱 AgriGenius 4.1</h1>

<div class="card">
  <h3>📋 Công việc hôm nay</h3>
  <ul id="tasks"></ul>
</div>

<div class="card">
  <h3>🤖 Trợ lý nông dân</h3>
  <input id="chatInput" placeholder="Hỏi gì cũng được..." />
  <button onclick="chat()">Gửi</button>
  <p id="chatReply"></p>
</div>

<div class="card">
  <h3>🔊 Nhắc nhở</h3>
  <button onclick="speak()">Nhắc việc</button>
</div>

<script>
  const defaultTasks = [
    "💧 Tưới nước",
    "🌾 Bón phân",
    "🐛 Kiểm tra sâu bệnh"
  ];

  function loadTasks() {
    const saved = JSON.parse(localStorage.getItem("tasks")) || defaultTasks.map(t => ({ text: t, done: false }));
    const ul = document.getElementById("tasks");
    ul.innerHTML = "";
    saved.forEach((task, index) => {
      const li = document.createElement("li");
      li.textContent = task.text;
      if (task.done) li.classList.add("done");
      li.onclick = () => toggleTask(index);
      ul.appendChild(li);
    });
  }

  function toggleTask(index) {
    const tasks = JSON.parse(localStorage.getItem("tasks"));
    tasks[index].done = !tasks[index].done;
    localStorage.setItem("tasks", JSON.stringify(tasks));
    loadTasks();
  }

  if (!localStorage.getItem("tasks"))
    localStorage.setItem(
      "tasks",
      JSON.stringify(defaultTasks.map(t => ({ text: t, done: false })))
    );

  loadTasks();

  function chat() {
    const text = document.getElementById("chatInput").value.toLowerCase();
    let reply = "Ừ, làm từ từ thôi, nông nghiệp là chuyện đường dài 🌱";

    if (text.includes("mệt")) reply = "Mệt thì nghỉ xíu, cây còn đó mà.";
    if (text.includes("bón")) reply = "Bón phân nhớ coi thời tiết nghen, đừng mưa là uổng.";
    if (text.includes("tưới")) reply = "Tưới sáng sớm hoặc chiều mát là ngon nhất.";
    if (text.includes("bao lâu")) reply = "Cái này tùy cây, nhưng đều tay là ổn.";

    document.getElementById("chatReply").innerText = reply;
  }

  function speak() {
    const msg = new SpeechSynthesisUtterance(
      "Nhắc nhẹ nè, tới giờ tưới nước với kiểm tra sâu bệnh rồi đó."
    );
    msg.lang = "vi-VN";
    speechSynthesis.speak(msg);
  }
</script>

</body>
</html>