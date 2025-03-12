```
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Manager</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Task Manager</h1>
    <input type="text" id="taskInput" placeholder="Enter a new task">
    <button onclick="addTask()">Add Task</button>
    <ul id="taskList"></ul>
    <script src="script.js"></script>
</body>
</html>

/* styles.css */
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin: 20px;
}

h1 {
    color: #333;
}

button {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}

/* script.js */
async function fetchTasks() {
    const response = await fetch("<API_GATEWAY_URL>/tasks");
    const tasks = await response.json();
    const taskList = document.getElementById("taskList");
    taskList.innerHTML = "";
    tasks.forEach(task => {
        let li = document.createElement("li");
        li.innerText = `${task.title} - ${task.status}`;
        taskList.appendChild(li);
    });
}

async function addTask() {
    const taskInput = document.getElementById("taskInput").value;
    if (!taskInput) return;
    await fetch("<API_GATEWAY_URL>/tasks", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ taskId: Date.now().toString(), title: taskInput, status: "Pending" })
    });
    fetchTasks();
}

document.addEventListener("DOMContentLoaded", fetchTasks);
```
