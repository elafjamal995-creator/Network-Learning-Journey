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


