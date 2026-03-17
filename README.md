# Ex03 To-Do List using JavaScript

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
html:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do Application</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="todo-container">
        <h1>To-Do List</h1>
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="What needs to be done?">
            <button id="addBtn">Add Task</button>
        </div>
        <div class="filters">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="active">Active</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
        </div>
        <ul id="taskList"></ul>
    </div>
    <script src="script.js"></script>
</body>
</html>
```
css:
```
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f4f7f6;
    display: flex;
    justify-content: center;
    padding-top: 50px;
}

.todo-container {
    background: white;
    width: 450px;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
}

h1 {
    text-align: center;
    color: #333;
    margin-bottom: 25px;
}

.input-section {
    display: flex;
    gap: 10px;
    margin-bottom: 25px;
}

#taskInput {
    flex: 1;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 6px;
    font-size: 16px;
    outline: none;
    transition: border-color 0.3s;
}

#taskInput:focus {
    border-color: #007bff;
}

#addBtn {
    padding: 12px 20px;
    background: #28a745;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.3s;
}

#addBtn:hover {
    background: #218838;
}

.filters {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    gap: 10px;
}

.filter-btn {
    flex: 1;
    padding: 8px;
    border: none;
    background: #e9ecef;
    cursor: pointer;
    border-radius: 6px;
    transition: 0.3s;
    font-weight: 600;
    color: #495057;
}

.filter-btn.active {
    background: #007bff;
    color: white;
}

ul {
    list-style: none;
}

li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    background: #f8f9fa;
    border: 1px solid #eee;
    border-radius: 6px;
    margin-bottom: 10px;
    transition: all 0.3s ease;
}

li.completed span {
    text-decoration: line-through;
    color: #adb5bd;
}

.task-actions {
    display: flex;
    gap: 5px;
}

.task-actions button {
    padding: 6px 12px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    transition: opacity 0.3s;
}

.task-actions button:hover {
    opacity: 0.8;
}

.toggle-btn {
    background: #17a2b8;
    color: white;
}

.edit-btn {
    background: #ffc107;
    color: #212529;
}

.delete-btn {
    background: #dc3545;
    color: white;
}
```
js:
```
document.addEventListener('DOMContentLoaded', () => {
    const taskInput = document.getElementById('taskInput');
    const addBtn = document.getElementById('addBtn');
    const taskList = document.getElementById('taskList');
    const filterBtns = document.querySelectorAll('.filter-btn');

    
    let tasks = JSON.parse(localStorage.getItem('todo_tasks')) || [];
    let currentFilter = 'all';

    
    function saveTasks() {
        localStorage.setItem('todo_tasks', JSON.stringify(tasks));
    }

    
    function renderTasks() {
        taskList.innerHTML = '';
        let filteredTasks = tasks;
        
        if (currentFilter === 'active') {
            filteredTasks = tasks.filter(t => !t.completed);
        } else if (currentFilter === 'completed') {
            filteredTasks = tasks.filter(t => t.completed);
        }

        filteredTasks.forEach(task => {
            const li = document.createElement('li');
            if (task.completed) li.classList.add('completed');

            const span = document.createElement('span');
            span.textContent = task.text;
            li.appendChild(span);

            const actionsDiv = document.createElement('div');
            actionsDiv.className = 'task-actions';

            const toggleBtn = document.createElement('button');
            toggleBtn.textContent = task.completed ? 'Undo' : 'Complete';
            toggleBtn.className = 'toggle-btn';
            toggleBtn.onclick = () => toggleTask(task.id);

            const editBtn = document.createElement('button');
            editBtn.textContent = 'Edit';
            editBtn.className = 'edit-btn';
            editBtn.onclick = () => editTask(task.id);

            const deleteBtn = document.createElement('button');
            deleteBtn.textContent = 'Delete';
            deleteBtn.className = 'delete-btn';
            deleteBtn.onclick = () => deleteTask(task.id);

            actionsDiv.appendChild(toggleBtn);
            actionsDiv.appendChild(editBtn);
            actionsDiv.appendChild(deleteBtn);
            
            li.appendChild(actionsDiv);
            taskList.appendChild(li);
        });
    }

    
    function addTask() {
        const text = taskInput.value.trim();
        if (!text) return; // Prevent creating empty tasks
        
        tasks.push({ id: Date.now(), text, completed: false });
        taskInput.value = '';
        
        saveTasks();
        renderTasks();
    }

    
    function toggleTask(id) {
        tasks = tasks.map(t => t.id === id ? { ...t, completed: !t.completed } : t);
        saveTasks();
        renderTasks();
    }

    
    function deleteTask(id) {
        tasks = tasks.filter(t => t.id !== id);
        saveTasks();
        renderTasks();
    }

    
    function editTask(id) {
        const task = tasks.find(t => t.id === id);
        const newText = prompt('Edit your task:', task.text);
        
        if (newText !== null && newText.trim() !== '') {
            task.text = newText.trim();
            saveTasks();
            renderTasks();
        }
    }

    
    addBtn.addEventListener('click', addTask);
    
    taskInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') addTask();
    });

    filterBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            filterBtns.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            currentFilter = btn.getAttribute('data-filter');
            renderTasks();
        });
    });

    renderTasks();
});
```
## OUTPUT

<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/3c198f36-27f2-48ba-bdad-7c1bb88769e5" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
