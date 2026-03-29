# 🎓 Ncube University of Coders — Application System

> A console-based C++ application simulating a real-world university admissions portal, featuring student registration, APS-based eligibility screening, and a formatted application summary.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Demo](#demo)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Run Online](#run-online)
  - [Run Locally](#run-locally)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Skills Demonstrated](#skills-demonstrated)
- [Sample Output](#sample-output)
- [Future Improvements](#future-improvements)
- [Author](#author)

---

## Overview

The **Ncube University of Coders Application System** is a structured C++ console application that replicates a university admissions workflow. Prospective students enter their personal details and APS score, select a programming specialisation and campus, and receive a professional formatted application summary.

This project was built to demonstrate practical programming fundamentals including input validation, conditional logic, struct-based data modelling, and clean console output — making it a strong portfolio piece for entry-level software development roles.

---

## Features

| Feature | Description |
|---|---|
| 🔢 Auto-generated Student Number | Unique 7-digit ID generated per applicant using `rand()` and `srand(time(0))` |
| ✅ Age Validation | Applicants under 18 are automatically rejected with a clear status message |
| 📊 APS Eligibility Check | APS score validated against range (0–50); determines qualification tier |
| 💻 Language Selection | Qualified students (APS ≥ 30) choose from Java, JavaScript, Python, or C++ |
| 🏫 Campus Selection | Choose from three campuses: JHCB, MTCV, or KLPG |
| 🛡️ Input Validation | Robust error handling for all numeric and string inputs |
| 📄 Formatted Summary | Application details displayed in a clean, aligned table |

---

## Demo

```
🎓 Welcome to Ncube University of Coders

Enter your name: Sipho
Enter your surname: Ncube
Enter your Age: 21
Your Student Number is: 4823917
Enter your APS score: 35

Choose your language:
1. Java
2. JavaScript
3. Python
4. C++
Enter option (1-4): 3

Choose your Campus:
1. JHCB
2. MTCV
3. KLPG
Enter option (1-3): 1

================ APPLICATION SUMMARY ================
Name           Surname        Age       Student No      Status                    Campus
-------------------------------------------------------------
Sipho          Ncube          21        4823917         Python selected           JHCB
```

---

## Screenshots

**Successful application — APS ≥ 30 (language selection unlocked)**

![Accepted applicant terminal output](screenshots/terminal_accepted.svg)

---

**Rejected application — underage applicant**

![Rejected underage applicant terminal output](screenshots/terminal_rejected_age.svg)

---

**Input validation — graceful recovery from invalid input**

![Invalid input handling terminal output](screenshots/terminal_invalid_input.svg)

---

## Getting Started

### Prerequisites

- A C++ compiler (e.g., GCC, Clang, MSVC) **or** an online compiler

### Run Online

No setup required. Use the Programiz Online C++ Compiler:

1. Open [Programiz Online C++ Compiler](https://www.programiz.com/cpp-programming/online-compiler/)
2. Paste the source code from `main.cpp`
3. Click **Run**
4. Follow the on-screen prompts

### Run Locally

```bash
# Clone the repository
git clone https://github.com/your-username/ncube-university-app.git
cd ncube-university-app

# Compile
g++ -o university_app main.cpp

# Run
./university_app
```

> **Windows users:** Replace `./university_app` with `university_app.exe`

---

## How It Works

```
Start
  │
  ├─ Enter Name & Surname
  │
  ├─ Enter Age
  │     └── Age < 18 → Rejected (Age) → Display Summary
  │
  ├─ Generate Student Number
  │
  ├─ Enter APS Score (0–50)
  │     └── Out of range → Rejected (Invalid APS) → Display Summary
  │
  ├─ APS < 30  → Status: "Qualified for C# and C++"
  │   APS ≥ 30 → Language Selection (Java / JavaScript / Python / C++)
  │
  ├─ Campus Selection (JHCB / MTCV / KLPG)
  │
  └─ Display Formatted Application Summary
```

---

## Project Structure

```
ncube-university-app/
├── main.cpp        # Full application source code
└── README.md       # Project documentation
```

---

## Source Code

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <string>
#include <iomanip>
using namespace std;

struct student
{
    string name;
    string surname;
    int age;
    int studentNumber;
    string campus;
    string Status;
    int Aps;
};

int getValidInt(string message)
{
    int value;
    while (true)
    {
        cout << message;
        cin >> value;
        if (cin.fail())
        {
            cin.clear();
            cin.ignore(1000, '\n');
            cout << "Invalid input! Please enter a number.\n";
        }
        else
        {
            return value;
        }
    }
}

string getValidString(string message)
{
    string value;
    cout << message;
    cin >> value;
    while (value.empty())
    {
        cout << "Input cannot be empty. Try again: ";
        cin >> value;
    }
    return value;
}

student applicationDetails()
{
    student s;
    s.name = getValidString("Enter your name: ");
    s.surname = getValidString("Enter your surname: ");
    s.age = getValidInt("Enter your Age: ");

    if (s.age < 18)
    {
        cout << "Not Qualified! Must be 18+\n";
        s.Status = "Rejected (Age)";
        return s;
    }

    srand(time(0));
    s.studentNumber = 1000000 + rand() % 9000000;
    cout << "Your Student Number is: " << s.studentNumber << endl;

    s.Aps = getValidInt("Enter your APS score: ");
    if (s.Aps < 0 || s.Aps > 50)
    {
        cout << "Invalid APS! Must be between 0-50.\n";
        s.Status = "Rejected (Invalid APS)";
        return s;
    }

    if (s.Aps < 30)
        s.Status = "Qualified for C# and C++";
    else
    {
        int option;
        do
        {
            cout << "\nChoose your language:\n";
            cout << "1. Java\n2. JavaScript\n3. Python\n4. C++\n";
            option = getValidInt("Enter option (1-4): ");
            switch (option)
            {
                case 1: s.Status = "Java selected"; break;
                case 2: s.Status = "JavaScript selected"; break;
                case 3: s.Status = "Python selected"; break;
                case 4: s.Status = "C++ selected"; break;
                default: cout << "Invalid choice, try again.\n"; break;
            }
        } while (option < 1 || option > 4);
    }

    int choice;
    do
    {
        cout << "\nChoose your Campus:\n";
        cout << "1. JHCB\n2. MTCV\n3. KLPG\n";
        choice = getValidInt("Enter option (1-3): ");
        switch (choice)
        {
            case 1: s.campus = "JHCB"; break;
            case 2: s.campus = "MTCV"; break;
            case 3: s.campus = "KLPG"; break;
            default: cout << "Invalid choice, try again.\n"; break;
        }
    } while (choice < 1 || choice > 3);

    return s;
}

void displayStudent(const student &s)
{
    cout << "\n================ APPLICATION SUMMARY ================\n";
    cout << left << setw(15) << "Name"
         << setw(15) << "Surname"
         << setw(10) << "Age"
         << setw(15) << "Student No"
         << setw(25) << "Status"
         << setw(10) << "Campus" << endl;
    cout << "-------------------------------------------------------------\n";
    cout << left << setw(15) << s.name
         << setw(15) << s.surname
         << setw(10) << s.age
         << setw(15) << s.studentNumber
         << setw(25) << s.Status
         << setw(10) << s.campus << endl;
}

int main()
{
    cout << "🎓 Welcome to Ncube University of Coders\n";
    student s = applicationDetails();
    displayStudent(s);
    return 0;
}
```

### Key Components

| Component | Purpose |
|---|---|
| `struct student` | Data model holding all applicant information |
| `getValidInt()` | Reusable function for validated integer input |
| `getValidString()` | Reusable function for non-empty string input |
| `applicationDetails()` | Core logic: collects data, validates, and builds the student record |
| `displayStudent()` | Renders the formatted application summary table |
| `main()` | Entry point; orchestrates the application flow |

---

## Skills Demonstrated

- **Structs** — modelling real-world entities with grouped data fields
- **Functions** — reusable, single-responsibility helper functions
- **Input Validation** — `cin.fail()` handling with stream clearing and recovery
- **Conditional Logic** — multi-tier APS eligibility branching
- **Loop Constructs** — `do-while` loops for menu-driven input with re-prompting
- **Random Number Generation** — `rand()` / `srand()` for unique student IDs
- **Output Formatting** — `setw()` and `left` from `<iomanip>` for aligned table display
- **Error Handling** — graceful rejection flows with descriptive status messages

---

## Sample Output

**Accepted applicant (APS ≥ 30):**
```
================ APPLICATION SUMMARY ================
Name           Surname        Age       Student No      Status                    Campus
-------------------------------------------------------------
Sipho          Ncube          21        4823917         Python selected           JHCB
```

**Rejected applicant (underage):**
```
Not Qualified! Must be 18+

================ APPLICATION SUMMARY ================
Name           Surname        Age       Student No      Status                    Campus
-------------------------------------------------------------
Lebo           Mokoena        16        0               Rejected (Age)
```

---

## Future Improvements

- [ ] Save application records to a file (file I/O)
- [ ] Support multiple applicants in a single session (arrays / vectors)
- [ ] Add a login system with password validation
- [ ] Migrate to object-oriented design using classes
- [ ] Build a GUI front-end or web interface

---

## Author

**Ncube** — IT Student & Aspiring Software Developer


---

*Built as a portfolio project to demonstrate structured C++ programming fundamentals.*
