# Robert Chaves P. (r0b0t95)

### ---------- Simple To-Do List v.1 ----------

#### Simple To-Do List Demo
![[extension1.jpg]](Simple_To-Do_List/extension1.jpg)

![[extension2.jpg]](Simple_To-Do_List/extension2.jpg)

#### background,js
```js
function createNotification() {
     chrome.notifications.create({
         type: 'basic',
         iconUrl: 'icon.png',
         title: 'Simple To-Do List Inspected',
         message: 'You have inspected Simple To-Do List. Check the console for more information!',
         priority: 2
     });
 }


 chrome.runtime.onInstalled.addListener(() => {
     let todoListFigure = `
     +----------+
     |To-Do List|
     +----------+
     | 1. ***** |
     | 2. ***** |
     | 3. ***** |
     | 4. ***** |
     |__________|
     `;
     console.log(todoListFigure);
 });


 chrome.runtime.onStartup.addListener(() => {
     console.log("Simple To-Do List started.");
 });
```

#### manifest.json
```json
{
  "manifest_version": 3,
  "name": "Simple To-Do List",
  "version": "1.0",
  "description": "A simple to-do list extension.",
  "permissions": ["storage"],
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "64": "todolist64.png"
    }
  },
  "background":{
    "service_worker": "background.js"
  },
  "icons": {
    "64": "todolist64.png"
  },
  "developer": {
    "name": "Robert Chaves P. (r0b0t95)",
    "email": "robb950@hotmail.com",
    "github": "https://github.com/r0b0t95"
  }
}
```

#### popup.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body class="p-3 mb-2 bg-dark text-white">

    <div class="image-container" >
        <img src="todolist64.png" alt="">
    </div>
    <div class="input-container">
        <input type="text" id="taskInput" placeholder="New task...">
        <button id="addTaskBtn" class="add-btn">+</button>
    </div>
    <ul id="taskList" class="list-group"></ul>
    <script src="popup.js"></script>

</body>
</html>
```

#### popup.js
```js
function loadTasks() {
    chrome.storage.local.get(['tasks'], function (result) {
        const tasks = result.tasks || [];
        const taskList = document.getElementById('taskList');
        taskList.innerHTML = '';

        tasks.forEach((task, index) => {
            const li = document.createElement('li');
            li.className = "task-name"
            li.textContent = task;

            // Create the remove button
            const removeBtn = document.createElement('button');
            removeBtn.textContent = 'X: ' + (index + 1);
            removeBtn.className = 'remove-btn';
            removeBtn.addEventListener('click', () => removeTask(index));

            li.appendChild(removeBtn);
            taskList.appendChild(li);
        });
    });
}


function addTask() {
    const taskInput = document.getElementById('taskInput');
    const task = taskInput.value.trim();

    if (task) {
        chrome.storage.local.get(['tasks'], function (result) {
            const tasks = result.tasks || [];
            tasks.push(task);
            chrome.storage.local.set({ tasks }, function () {
                loadTasks();
                taskInput.value = ''; // Clear input field
            });
        });
    }
}

function removeTask(index) {
    chrome.storage.local.get(['tasks'], function (result) {
        const tasks = result.tasks || [];
        tasks.splice(index, 1); // Remove the task at the given index
        chrome.storage.local.set({ tasks }, loadTasks); // Save updated tasks and reload
    });
}
```

#### styles.css
```css
body {
     font-family: Arial, sans-serif;
     padding: 10px;
     background-color: #232323;
 }

 h1 {
     font-size: 16px;

 }

 button {
     padding: 10px;
     margin-top: 10px;
     cursor: pointer;
 }

 #status {
     margin-top: 10px;
     font-size: 14px;
 }

 .remove-btn{
    background-color: #dc3545;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 10px;
    margin: -2% 0% 0% 3%;
}

.add-btn{
    background-color: #28a745;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
    margin: -0.1% 1% 0% 3%;
}

.image-container{
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 5%;
}

.input-container {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 20px;
}

.task-name{
    list-style-type: decimal;
    color: #dddddd;
    margin-bottom: 4%;
}
```

