# Gantt Chart Calculator (Servlet)

A web-based project scheduling tool made with Java Servlet and run on Apache Tomcat using IntelliJ IDEA.
The user enters the tasks of a project with their duration and dependencies. The program finds the start and
finish day of every task, the critical path, the total project duration and draws a Gantt chart.

**Name:** Shayla Akter
**ID:** IT24022
**Department:** Information and Communication Technology (ICT), MBSTU
**Session:** 2023-24 (2nd Year 2nd Semester)

---

## 1. About the Project

A Gantt chart shows the tasks of a project as horizontal bars on a timeline. It helps to see when each task
starts, when it finishes and which tasks depend on other tasks. This project calculates these values
automatically using the **Critical Path Method (CPM)** and shows the chart in the browser.

I made it as a servlet so that it runs on Tomcat like my other servlet project (Simple Calculator).

## 2. Features

- Add and remove any number of tasks
- Dependencies can be written with only the task letter (`A`, `B, C`) or the full task name
- Calculates earliest start, earliest finish and slack of each task
- Finds the critical path and the total project duration
- Draws a Gantt chart with day numbers on top
- Critical tasks are shown in **red**, other tasks in **blue**
- Checks the input and shows a message for:
  - duplicate task names
  - duration that is not a whole number or is less than 1
  - dependency on a task that does not exist
  - a task depending on itself
  - circular dependency (for example A needs B and B needs A)
- Sample data is loaded when the page opens (**Load Sample** button resets it)

## 3. Technologies Used

| Item | Details |
|---|---|
| Language | Java (17) |
| Technology | Java Servlet (Jakarta Servlet API 6.0) |
| Server | Apache Tomcat 10.1.61 |
| Build tool | Maven |
| IDE | IntelliJ IDEA Ultimate |
| Frontend | HTML, CSS and a little JavaScript (add/remove row) |

## 4. Project Structure

```
servlet-SimpleCalculator
├── pom.xml
└── src
    └── main
        ├── java
        │   └── com/example/calculator
        │       ├── CalculatorServlet.java
        │       └── GanttServlet.java      <- this project
        └── webapp
```

## 5. How to Run

1. Open the project in IntelliJ IDEA Ultimate.
2. In `pom.xml` check that Java version is `17`, then reload Maven.
3. Create `GanttServlet.java` inside `src/main/java/com/example/calculator` and paste the code.
4. Select **Tomcat 10.1.61** from the run configuration list and click **Run**.
5. Open this address in the browser:
   `http://localhost:8080/servlet_SimpleCalculator_war_exploded/gantt`
6. Press **Calculate Gantt Chart** to see the result.

If you get a 404 error, check the *Application context* in **Edit Configurations → Deployment** and use the same name in the URL.

## 6. How to Use

1. Write the task name. The first word of every task name should be a **unique letter**, like `A Requirements`, `B Board Design`.
2. Write the duration in days (whole number, 1 or more).
3. In **Depends on**, write the letters of the tasks that must finish before this task starts, separated by comma. Example: `B, C`. Leave it empty if the task has no dependency.
4. Use **Add Task** to add a new row and **X** to delete a row.
5. Press **Calculate Gantt Chart**.

## 7. How It Works (CPM Method)

1. **Sorting the tasks:** The tasks are arranged in a valid order so that a task always comes after the tasks it depends on (topological sort). If this is not possible there is a circular dependency, and an error is shown.
2. **Forward pass:**
   - Earliest Start (ES) = the biggest Earliest Finish (EF) of all its dependencies (0 if none)
   - Earliest Finish (EF) = ES + Duration
   - Project duration = the biggest EF of all tasks
3. **Backward pass** (going from the last task to the first):
   - Latest Finish (LF) = the smallest Latest Start (LS) of the tasks that come after it (project duration if none)
   - Latest Start (LS) = LF - Duration
4. **Slack** = LS - ES. This is how many days a task can be delayed without delaying the project.
5. Tasks with **slack = 0** are **critical tasks**. They make the critical path.
6. The Gantt chart draws every task as a bar. The left position of the bar is ES and the width is the Duration.

## 8. Sample Input and Output

**Input** (Sholo Guti and Bagh Chhagol game project)

| Task | Duration | Depends on |
|---|---|---|
| A Requirements | 2 | |
| B Board Design | 3 | A |
| C Graphics and Sound | 2 | A |
| D Sholo Guti Logic | 5 | B |
| E Bagh Chhagol Logic | 5 | B |
| F Computer AI | 4 | D, E |
| G Home Page and UI | 3 | B, C |
| H Testing | 3 | F, G |
| I Report Writing | 4 | D, E |
| J Slides and Banner | 2 | H, I |

**Output**

- Project duration: **19 days**
- Critical path: **A → B → D → E → F → H → J**

| Task | Start | Finish | Slack | Critical |
|---|---|---|---|---|
| A Requirements | 0 | 2 | 0 | YES |
| B Board Design | 2 | 5 | 0 | YES |
| C Graphics and Sound | 2 | 4 | 7 | no |
| D Sholo Guti Logic | 5 | 10 | 0 | YES |
| E Bagh Chhagol Logic | 5 | 10 | 0 | YES |
| F Computer AI | 10 | 14 | 0 | YES |
| G Home Page and UI | 5 | 8 | 6 | no |
| H Testing | 14 | 17 | 0 | YES |
| I Report Writing | 10 | 14 | 3 | no |
| J Slides and Banner | 17 | 19 | 0 | YES |

Here C, G and I have slack, so a small delay in these tasks does not delay the whole project.
D and E run at the same time and both take 5 days, so both are critical.

## 9. Screenshots

Task table:
<img width="1018" height="627" alt="photo_2026-11-08_08-15-03" src="https://github.com/user-attachments/assets/37e0dfa1-6d32-4a6e-8a47-152a6cde152c" />
Result table:
<img width="1020" height="511" alt="photo_2026-11-08_08-15-56" src="https://github.com/user-attachments/assets/8e721a5f-4254-4946-8b6a-cf7d2014ea91" />

Gantt chart:
<img width="1020" height="511" alt="photo_2026-11-08_08-15-56" src="https://github.com/user-attachments/assets/1bd955ed-4ced-440f-b29d-efcfb1e5628d" />


## 10. Problems I Faced

- My first version showed the error "depends on unknown task" because I typed extra spaces and a different task name in the Depends on column. I fixed it by ignoring extra spaces and capital/small letters, and by allowing only the task letter (`A`, `B`).
- The old *Hello World* page kept opening. I changed the URL in the Tomcat run configuration to fix this.

## 11. Limitations and Future Work

- Time is counted only in whole days. Real calendar dates and weekends are not used.
- Data is not saved after the page is closed. A database (JDBC) can be added to save projects.
- The chart can be downloaded as an image or PDF in future.
- Task cost and resource can be added.
