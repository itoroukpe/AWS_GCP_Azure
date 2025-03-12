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
    <div id="task-form">
        <input type="text" id="taskInput" placeholder="Enter a task">
        <button onclick="addTask()">Add Task</button>
    </div>
    <ul id="taskList"></ul>
    <script src="script.js"></script>
</body>
</html>
```
---
```
/* styles.css */
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #f4f4f4;
}
#task-form {
    margin: 20px auto;
    width: 300px;
    display: flex;
    gap: 10px;
}
input {
    flex: 1;
    padding: 8px;
}
button {
    padding: 8px;
    background-color: #28a745;
    color: white;
    border: none;
    cursor: pointer;
}
button:hover {
    background-color: #218838;
}
#taskList {
    list-style: none;
    padding: 0;
}
li {
    background: white;
    padding: 10px;
    margin: 5px auto;
    width: 300px;
    display: flex;
    justify-content: space-between;
    border-radius: 4px;
}
```
---
```
/* script.js */
function addTask() {
    let taskInput = document.getElementById("taskInput");
    let taskList = document.getElementById("taskList");
    if (taskInput.value.trim() === "") return;
    let li = document.createElement("li");
    li.textContent = taskInput.value;
    let deleteBtn = document.createElement("button");
    deleteBtn.textContent = "X";
    deleteBtn.style.backgroundColor = "#dc3545";
    deleteBtn.style.color = "white";
    deleteBtn.onclick = function() {
        taskList.removeChild(li);
    };
    li.appendChild(deleteBtn);
    taskList.appendChild(li);
    taskInput.value = "";
}
