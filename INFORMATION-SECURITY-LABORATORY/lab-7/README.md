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




















 
