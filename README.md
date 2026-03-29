# Week 5: {Project Title}

## Author
- **Name:** Ethen Mutiso
- **GitHub:**iyf-s10-week-05-kaduskotoxic-driod
- **Date:** Month Day, Year

## Project Description
Brief description of what you built and why.

## Technologies Used
- HTML5
- CSS3
- JavaScript
- (list all technologies)

## Features
- Feature 1
- Feature 2
- Feature 3

## How to Run
1. Clone this repository
2. Open `index.html` in your browser
   OR
   Run `npm install` then `npm start`

## Lessons Learned
What did you learn while building this project?

## Challenges Faced
What problems did you encounter and how did you solve them?

## Screenshots (optional)
![Screenshot description](path/to/screenshot.png)


* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(135deg, #667eea, #764ba2);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* Container */
.container {
  background: #fff;
  padding: 25px;
  border-radius: 15px;
  width: 400px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

/* Title */
h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

/* Form */
form {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

form input {
  flex: 1;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
  transition: 0.3s;
}

form input:focus {
  border-color: #667eea;
}

form button {
  padding: 10px 15px;
  border: none;
  background: #667eea;
  color: white;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.3s;
}

form button:hover {
  background: #5563d6;
}

/* Filters */
.filters {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
}

.filter {
  flex: 1;
  padding: 8px;
  margin: 0 3px;
  border: none;
  border-radius: 8px;
  background: #eee;
  cursor: pointer;
  transition: 0.3s;
}

.filter.active {
  background: #667eea;
  color: white;
}

.filter:hover {
  background: #dcdcdc;
}

/* Task list */
ul {
  list-style: none;
  max-height: 200px;
  overflow-y: auto;
}

li {
  background: #f4f6fb;
  padding: 12px;
  margin-bottom: 10px;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: 0.2s;
}

li:hover {
  transform: scale(1.02);
}

/* Completed task */
li.completed {
  text-decoration: line-through;
  color: #888;
  background: #e2e8f0;
}

/* Buttons inside task */
li button {
  border: none;
  background: #ff5c5c;
  color: white;
  padding: 5px 8px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 12px;
}

li button:hover {
  background: #e04848;
}

/* Stats section */
.stats {
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
  font-size: 14px;
  color: #555;
}

#clear-completed {
  border: none;
  background: transparent;
  color: #667eea;
  cursor: pointer;
  transition: 0.2s;
}

#clear-completed:hover {
  text-decoration: underline;
}


const form = document.getElementById("todo-form");
const input = document.getElementById("todo-input");
const list = document.getElementById("todo-list");
const filters = document.querySelectorAll(".filter");
const itemsLeft = document.getElementById("items-left");
const clearCompletedBtn = document.getElementById("clear-completed");

let todos = JSON.parse(localStorage.getItem("todos")) || [];
let currentFilter = "all";

// Save to localStorage
function saveTodos() {
    localStorage.setItem("todos", JSON.stringify(todos));
}

// ➕ Add task
form.addEventListener("submit", function (e) {
    e.preventDefault();

    const text = input.value.trim();
    if (text === "") return;

    const todo = {
        id: Date.now(),
        text: text,
        completed: false
    };

    todos.push(todo);
    input.value = "";

    saveTodos();
    renderTodos();
});

// 🔄 Render tasks
function renderTodos() {
    list.innerHTML = "";

    let filteredTodos = todos;

    if (currentFilter === "active") {
        filteredTodos = todos.filter(todo => !todo.completed);
    } else if (currentFilter === "completed") {
        filteredTodos = todos.filter(todo => todo.completed);
    }

    filteredTodos.forEach(todo => {
        const li = document.createElement("li");

        li.innerHTML = `
            <input type="checkbox" class="check" ${todo.completed ? "checked" : ""}>
            <span class="${todo.completed ? "completed" : ""}">
                ${todo.text}
            </span>
            <button class="delete">X</button>
        `;

        // ✅ Toggle complete
        const checkbox = li.querySelector(".check");
        checkbox.addEventListener("change", () => {
            todo.completed = checkbox.checked;
            saveTodos();
            renderTodos();
        });

        // ❌ Delete task
        li.querySelector(".delete").addEventListener("click", () => {
            todos = todos.filter(t => t.id !== todo.id);
            saveTodos();
            renderTodos();
        });

        list.appendChild(li);
    });

    updateItemsLeft();
}

// 📊 Update items left
function updateItemsLeft() {
    const count = todos.filter(todo => !todo.completed).length;
    itemsLeft.textContent = `${count} item${count !== 1 ? "s" : ""} left`;
}

// 🔍 Filters
filters.forEach(button => {
    button.addEventListener("click", () => {
        document.querySelector(".filter.active").classList.remove("active");
        button.classList.add("active");

        currentFilter = button.dataset.filter;
        renderTodos();
    });
});

// 🧹 Clear completed
clearCompletedBtn.addEventListener("click", () => {
    todos = todos.filter(todo => !todo.completed);
    saveTodos();
    renderTodos();
});

// 🚀 Load tasks when page opens
renderTodos();

## Live Demo (if deployed)
[View Live Demo]( https://kaduskotoxic-droid.github.io/iyf-s10-week-04-kaduskotoxic-droid/)
