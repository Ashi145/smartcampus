# Smart Campus Student Management System

A console-based Java application for the "Smart Campus Student Management and
Academic Information System" coursework. No database server or extra libraries
needed — it saves everything to plain CSV files in the `data/` folder, so it
runs with nothing more than a JDK.

## How to run it in VS Code

1. Open the `SmartCampus` folder in VS Code (`File > Open Folder`).
2. Install the **Extension Pack for Java** (by Microsoft) if you don't have it —
   VS Code will usually prompt you to install it automatically when it sees a
   `.java` file.
3. Open `src/com/smartcampus/Main.java`.
4. Click the **Run** button above `public static void main` (or press `F5`).

That's it — a terminal panel opens inside VS Code with the menu.

## How to run it without VS Code (plain terminal)

```bash
cd SmartCampus
javac -d out $(find src -name "*.java")   # compile
java -cp out com.smartcampus.Main         # run
```

On Windows PowerShell, compile with:
```powershell
javac -d out (Get-ChildItem -Recurse -Filter *.java src | ForEach-Object { $_.FullName })
java -cp out com.smartcampus.Main
```

## First login

The very first time you run it, an admin account is created automatically:

```
username: admin
password: admin123
```

Log in as admin first to add a department, a course, a lecturer and a student —
then log out and log back in as the lecturer or student to try their menus.

## Suggested demo flow

1. Login as `admin` → Add Department → Add Course → Add Lecturer → Add Student
   → Assign Lecturer to Course.
2. Login as the lecturer you created → Record Attendance → Enter Marks.
3. Login as the student you created → Register for a Course → View Attendance
   → View Results/GPA → View/Save Transcript.

## Project structure (maps to OOP concepts for your report)

```
src/com/smartcampus/
  model/       User (abstract), Admin, Lecturer, Student, Department,
               Course, Enrollment, AttendanceRecord, Mark
               -> INHERITANCE: Admin/Lecturer/Student extend User
               -> POLYMORPHISM: getRole() overridden per subclass;
                  Main routes on the same User reference via instanceof
               -> ENCAPSULATION: private fields, public getters/setters

  service/     AuthService, AcademicService, EnrollmentService,
               AttendanceService, GradeService, TranscriptService
               -> business logic and file persistence, kept separate
                  from the model classes and from Main (separation of concerns)

  util/        CsvUtil - shared file read/write helper

  Main.java    console menu, one method per menu action

data/          CSV "database" files, created automatically on first run
```

## Where the grading logic lives

`GradeService.java`:
- Total = 30% coursework + 70% exam
- Grade: A ≥ 80, B ≥ 70, C ≥ 60, D ≥ 50, F < 50
- Grade points (5.0 scale): A=5, B=4, C=3, D=2, F=0
- GPA = credit-unit-weighted average across all courses with a mark

These numbers are just constants near the top of the file — change them if
your department uses a different weighting or scale.

## Known simplifications (worth mentioning in your report)

- No password hashing — passwords are stored in plain text in the CSV files,
  which is fine for a coursework demo but never for a real deployed system.
- CSV values can't contain commas (a course title with a comma would break a row).
- No concurrent-user handling — it's a single-user console session.
