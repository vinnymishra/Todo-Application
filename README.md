# Todo-Application

A simple Full Stack Todo Application built using Java, Spring Boot, Thymeleaf, MySQL, Bootstrap, and JPA/Hibernate.
This project helped me understand the fundamentals of Full Stack Java Development, including database connectivity, CRUD operations, MVC architecture, and dynamic web pages using Thymeleaf.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Project Overview

The Todo Application allows users to:
- Add new tasks
- View all tasks
- Mark tasks as completed
- Toggle task status
- Delete tasks
- Store task data in a MySQL database

The application follows the MVC (Model-View-Controller) architecture and uses Spring Boot for backend development.


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Application Preview

Todo Application
```
[ Add a new task... ]  [ Add ]

Task 1                [Toggle] [Delete]
Task 2                [Toggle] [Delete]
Task 3                [Toggle] [Delete] 
```
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Technologies used

# Backend
- Java
- Spring Boot
- Spring MVC
- Spring Data JPA
- Hibernate

# Frontend
- HTML
- Thymeleaf
- Bootstrap 5

# Database
- MySQL

# Build Tool
- Maven

The project uses Spring Boot starters for Web MVC, Thymeleaf, and JPA along with the MySQL JDBC driver.


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Project Structure
```
src
│
├── controller
│   └── TaskController
│
├── services
│   └── TaskService
│
├── repository
│   └── TaskRepository
│
├── models
│   └── Task
│
├── templates
│   └── tasks.html
│
└── resources
    └── application.properties
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Features Implemented

# Add Tasks
Users can enter a task title and click the Add button.
```
@PostMapping
public String getTasks(@RequestParam String title)
```
#TaskController.java
```
package com.app.todoapp.controller;

import com.app.todoapp.models.Task;
import com.app.todoapp.services.TaskService;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import java.util.List;

@Controller
//@RequestMapping("/tasks")
public class TaskController {

    private final TaskService taskService;

    public TaskController(TaskService taskService) {
        this.taskService = taskService;
    }

    @GetMapping
    public String getTasks(Model model) {
        System.out.println("Controller Hit");
        List<Task> tasks = taskService.getAllTasks();
        System.out.println(tasks);
        model.addAttribute("tasks", tasks);
        return "tasks";

    }

    @PostMapping
    public String getTasks(@RequestParam String title) {
        System.out.println("Controller Hit");
        taskService.createTask(title);
        return "redirect:/";
    }

    @GetMapping("/{id}/delete")
    public String deleteTask(@PathVariable Long id) {
        System.out.println("Controller Hit");
        taskService.deleteTask(id);
        return "redirect:/";
    }

    @GetMapping("/{id}/toggle")
    public String toggleTask(@PathVariable Long id) {
        System.out.println("Controller Hit");
        taskService.toggleTask(id);
        return "redirect:/";
    }




}
```


# View Tasks
All tasks are fetched from the database and displayed dynamically using Thymeleaf.

```
taskRepository.findAll()
```

The tasks are loaded through the service layer and passed to the frontend.

# TaskService.java
```
package com.app.todoapp.services;

import com.app.todoapp.models.Task;
import com.app.todoapp.repository.TaskRepository;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class TaskService {

    private final TaskRepository taskRepository;

    public TaskService(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }

    public List<Task> getAllTasks(){
        return taskRepository.findAll();
    }
    public void createTask(String title){
      Task task = new Task();
      task.setTitle(title);
      task.setCompleted(false);
      taskRepository.save(task);
    }

    public void deleteTask(Long id) {
        taskRepository.deleteById(id);
    }

    public void toggleTask(Long id) {
        Task task = taskRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("Invalid Task id"));
        task.setCompleted(!task.isCompleted());
        taskRepository.save(task);
    }
}
```



