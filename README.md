# 📊 Student Performance Analytics — Power BI

A data-driven Power BI solution to monitor and improve student academic performance.

---

## 🚩 Project Overview

This repository contains the Power BI data model, transformation logic (DAX), and report pages built to analyze student grades across courses, instructors, departments and semesters. The goal is to provide an interactive, easy-to-interpret dashboard to help academic staff identify trends, at-risk students, and areas for pedagogical improvement.

### 🔍 Problem Statement

The institution has observed fluctuating academic performance between semesters and lacks a consistent reporting system to detect trends and problems in real time. This project centralizes grade data and provides visual analytics to support evidence-based decisions.

### 🎯 Objectives

* Measure average grades per course and semester.
* Calculate pass rate and percent of students passing exams.
* Compare performance across departments and instructors.
* Deliver an interactive, clean Power BI report for leadership and academic staff.

---

## 🗂️ Data Model

The solution uses a relational model with five main tables (One-to-Many relationships):

* Departments: `DepartmentID`, `DepartmentName`
* Instructors: `InstructorID`, `FirstName`, `LastName`, `DepartmentID`
* Students: `StudentID`, `FirstName`, `LastName`, `Gender`, `DateOfBirth`, `EnrollmentDate`
* Courses: `CourseID`, `CourseName`, `Credits`, `DepartmentID`
* Grades: `GradeID`, `StudentID`, `CourseID`, `InstructorID`, `Grade`, `Semester`

Relationships (1 : M)

* Students ↔ Grades (StudentID)
* Courses ↔ Grades (CourseID)
* Instructors ↔ Grades (InstructorID)
* Departments ↔ Instructors (DepartmentID)

This structure enforces data integrity and enables complex analytical queries.

---

## 🔧 Transformations & Modeling (DAX / Power Query)

### Calculated Columns (DAX)

```dax
Grades[IsPassed] = IF(Grades[Grade] >= 50, "Yes", "No")
Courses[DifficultyLevel] = SWITCH(TRUE(), Courses[Credits] >= 5, "High", Courses[Credits] >= 3, "Medium", "Low")
-- Age in Students (Power Query example):
-- Age = Date.Year(DateTime.LocalNow()) - Date.Year([DateOfBirth])
```

### Measures (DAX)

```dax
AverageGrade = AVERAGE(Grades[Grade])
PassRate = DIVIDE(CALCULATE(COUNTROWS(Grades), Grades[IsPassed] = "Yes"), COUNTROWS(Grades), 0)
```

> Tip: add time intelligence measures (e.g., semester-to-semester change) to spot trends.

---

## 📈 Report Pages & Visuals

Key visuals included in the Power BI report:

* Bar Chart — Average Grade by Course

  * X: `CourseName`, Y: `AverageGrade`
  * Helps compare courses and identify those needing intervention.

* Pie Chart — Pass vs. Fail

  * Legend: `IsPassed`, Values: `Count(GradeID)`
  * Visual snapshot of overall pass rates.

* Card — Pass Rate (%)

  * A single-number KPI representing overall success.

* Slicer — Semester

  * Filter the report by semester to analyze temporal changes.

* Department/Instructor Comparison Tables

  * Show average grades and pass rates per department and instructor.

---

## 🔎 Key Findings (Example)

* Courses like *Operating Systems* and *Algorithms* show very high averages (\~92).
* *Linear Algebra* shows the lowest average (\~48) and requires attention.
* Overall pass rate is high (\~95%), but low-performing courses still exist and may hide at-risk students.
