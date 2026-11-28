# Experiment 15: Django To-Do Application

**Student Name:** Divyanshu  
**SAP ID:** 590018926  
**Batch:** 05 (Core)  
**Course:** Backend Development (CSFS3008P)  
**Faculty:** Prateek Raj  

---

## 1. Aim & Objective
To develop a dynamic web application using the **Django Framework** implementing Model-View-Template (MVT) architecture, SQLite database connectivity, and dynamic HTML rendering through the **Django Template Language (DTL)**.

---

## 2. Django Template Language (DTL) Implementation

Django utilizes its own template syntax to dynamically populate HTML pages using variables and control structures:
- `{{ variable }}` for displaying context data and model fields.
- `{% ... %}` for control tags (template inheritance, loops, conditionals, CSRF tokens, and URL routing).

> **Note on Jekyll / GitHub Pages Compatibility:**  
> When documenting Django projects on GitHub Pages, Django template expressions can conflict with Jekyll's Liquid engine. All template snippets below are escaped using Liquid's `{% raw %}` and `{% endraw %}` tags to prevent Jekyll build and syntax errors.

---

### A. Base Layout (`base.html`)
Defines the site skeleton and loads static assets:

{% raw %}
```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}To-Do App{% endblock %}</title>
    <link rel="stylesheet" href="{% static 'todo/style.css' %}">
</head>
<body>
    {% include 'todo/navbar.html' %}
    <div class="container">
        {% block content %}{% endblock %}
    </div>
</body>
</html>
```
{% endraw %}

---

### B. Task List View (`task_list.html`)
Demonstrates template inheritance, loops, and conditional fallbacks using `{% for ... %}` and `{% empty %}`:

{% raw %}
```html
{% extends 'todo/base.html' %}

{% block title %}To-Do List{% endblock %}

{% block content %}
    <h1>My To-Do List</h1>
    <a class="btn" href="{% url 'add_task' %}">Add New Task</a>
    <ul>
        {% for task in tasks %}
            {% include 'todo/task_item.html' %}
        {% empty %}
            <li>No tasks yet.</li>
        {% endfor %}
    </ul>
{% endblock %}
```
{% endraw %}

---

### C. Task Item Partial (`task_item.html`)
Renders an individual task with conditional strikethrough based on completion state:

{% raw %}
```html
<li>
    {% if task.completed %}
        <s>{{ task.title }}</s>
    {% else %}
        {{ task.title }}
    {% endif %}
    <div>
        <a href="{% url 'toggle_task' task.id %}">Toggle</a>
        <a href="{% url 'delete_task' task.id %}">Delete</a>
    </div>
</li>
```
{% endraw %}

---

### D. Add Task Form View (`add_task.html`)
Demonstrates CSRF token protection and POST form submission in Django:

{% raw %}
```html
{% extends 'todo/base.html' %}

{% block title %}Add Task{% endblock %}

{% block content %}
    <h1>Add a New Task</h1>
    <form method="POST">
        {% csrf_token %}
        <input type="text" name="title" placeholder="Enter task title" required>
        <button type="submit">Add Task</button>
    </form>
    <a href="{% url 'task_list' %}">Back to List</a>
{% endblock %}
```
{% endraw %}

---

## 3. Application Structure

```text
django_todo_app/
├── manage.py
├── db.sqlite3
├── todo_project/
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
└── todo/
    ├── models.py          # Task(title, completed)
    ├── views.py           # task_list, add_task, toggle_task, delete_task
    ├── urls.py            # App route patterns
    ├── static/todo/
    │   └── style.css      # Custom stylesheet
    └── templates/todo/
        ├── base.html      # Layout template
        ├── navbar.html    # Navigation header
        ├── task_list.html # List view
        ├── task_item.html # Partial template
        └── add_task.html  # Creation form
```

---

## 4. How to Run Locally

1. Navigate to the project directory:
   ```bash
   cd Lab/Experiment-15/django_todo_app
   ```
2. Apply database migrations:
   ```bash
   python manage.py migrate
   ```
3. Start the Django development server:
   ```bash
   python manage.py runserver
   ```
4. Open your browser at `http://127.0.0.1:8000/`.
