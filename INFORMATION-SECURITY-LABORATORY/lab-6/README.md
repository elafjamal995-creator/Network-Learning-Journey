# SEED Labs - Buffer Overflow Attack Lab (Server Version)
## الشرح الشامل والكامل للمختبر والخطوات والتفاصيل الهندسية

---

## 1. Overview & General Concept / الفكرة العامة والهدف من اللاب

### العربية:
هذا اللاب هو جزء من سلسلة مختبرات **SEED Labs** الشهيرة المخصصة لأمن البرمجيات والشبكات (من تطوير البروفيسور Wenliang Du). يركز المختبر على شرح وتطبيق ثغرة **طفح المخزن المؤقت (Buffer Overflow Vulnerability)**.

* **تعريف الثغرة (Buffer Overflow):** هي حالة حدثية في البرمجيات عندما يحاول برنامج كتابة بيانات في مساحة خزن مؤقتة (Buffer) في الذاكرة تتجاوز الحجم المخصص لها، مما يؤدي إلى فيضان البيانات وتجاوز الحدود المحددة.
* **الهدف المباشر:** 
  1. **الاستغلال (Attack):** التفاعل مع 4 خوادم (Containers) تعمل كـ Root، وتصميم كود استغلال (Payload) يرسل عبر الشبكة للحصول على صلاحيات الجذر (**Root Shell**).
  2. **الحماية والدفاع (Countermeasures):** تجربة وتقييم وسائل الحماية المختلفة واختبار كفاءتها في التصدي للهجوم مع شرح الأسباب الهندسية.

### English:
This lab is part of the famous **SEED Labs** series developed by Prof. Wenliang Du. It focuses on demonstrating and exploiting the **Buffer Overflow Vulnerability**.

* **Vulnerability Definition:** Occurs when a program attempts to write data beyond the boundary of a buffer in memory, overflowing into adjacent memory spaces.
* **Lab Objectives:**
  1. **Attack Phase:** Exploit 4 target servers (Docker containers) running with root privileges by crafting and sending a payload over the network to obtain a **Root Shell**.
  2. **Defense Phase:** Test and evaluate different system countermeasures and protections to understand how they defend against such attacks and explain the mechanisms behind them.

---

## 2. Technical Concepts & Analogy / الشرح الفني وتوضيح المفاهيم

### مقارنة وتوضيح مهم (Buffer Overflow vs. Network Flooding):
* **في الشبكات (مثال: ARP Spoofing / MAC Flooding):**
  عند غمر السويتش أو الراوتر برسائل كثيرة، يمتلئ جدول العناوين (MAC Table)، مما يؤدي لتعطيل الجهاز أو تحويله لنظام البث المفتوح (Hub)، مما يسبب حجب الخدمة (Denial of Service - DoS).
* **في الذاكرة والبرمجيات (Buffer Overflow):**
  الـ Buffer هو مساحة متصلة في ذاكرة الـ RAM. عند إرسال بيانات أكبر من حجم الـ Buffer، تسيل البيانات الزائدة لتغطي على البيانات المجاورة لها في الـ Stack، وتحديداً تغطي **عنوان العودة (Return Address)**.
* **الفرق الجوهري:** الـ Buffer Overflow لا يكتفي بإيقاف البرنامج أو إحداث خلل، بل يمنح المهاجم القدرة على **إعادة توجيه مسار تنفيذ المعالج (Control Flow Hijacking)** لتنفيذ كود خبيث (Shellcode) والحصول على **صلاحيات كاملة (Root Access)** على السيرفر.

---

## 3. Vulnerable Code Analysis / تحليل الكود المصاب (`stack.c`)

### العربية:
الكود الأساسي المصاب بالثغرة موجود داخل ملف `stack.c` في مجلد `server-code`:

1. **دالة `main()`:**
   * تقوم بتخصيص مصفوفة `str` بحجم 517 بايت لقراءة مدخلات المستخدم عبر المدخل القياسي (`stdin`).
   * تقوم بتقليص وتقسيم المدخلات ثم تمريرها إلى دالة `bof(str)`.
2. **دالة `bof(char *str)` (سبب الثغرة):**
   * تمتلك مصفوفة محلية اسمها `buffer` بحجم أصغر (محدد بالثابت `BUF_SIZE` مثل 100 بايت).
   * تستخدم دالة النسخ `strcpy(buffer, str)`.
   * **مكمن الخلل:** دالة `strcpy` تنسخ البيانات دون الفحص عن الحدود (No Boundary Check)، مما يسبب طفح البيانات في الـ Stack فوق عنوان العودة.

### English:
The main vulnerable program resides in `stack.c` inside the `server-code` directory:

1. **The `main()` Function:**
   * Allocates an array `str` of 517 bytes and reads input from standard input (`stdin`).
   * Passes the input array to the function `bof(str)`.
2. **The `bof(char *str)` Function (Vulnerability Point):**
   * Declares a local buffer `buffer` of a smaller size defined by `BUF_SIZE` (e.g., 100 bytes).
   * Executes `strcpy(buffer, str)`.
   * **The Flaw:** `strcpy()` does not perform boundary checks, allowing data larger than `BUF_SIZE` to overwrite adjacent memory and hijack the **Return Address** on the stack.

---

## 4. Compilation Flags & Defense Mechanisms / خيارات التجميع وآليات الدفاع

### لماذا نستخدم Flags مخصصة أثناء التجميع؟
تتضمن الأنظمة الحديثة آليات دفاع متعددة تمنع الاستغلال التلقائي. لإتاحة دراسة الثغرة تعليمياً، نلغي هذه الحمايات باستخدام الخيارات التالية عند التجميع:

1. **`sudo /sbin/sysctl -w kernel.randomize_va_space=0`**
   * **الوظيفة:** إيقاف حماية عشوائية تخطيط مساحة العناوين (**ASLR**).
   * **السبب:** تثبيت عناوين الـ Stack في الذاكرة لتسهيل التنبؤ بعنون الـ Shellcode.

2. **`-fno-stack-protector`**
   * **الوظيفة:** إيقاف حارس الـ Stack (المعروف بـ **Stack Canary**).
   * **السبب:** الحارس يضع قيمة عشوائية قبل عنوان العودة ويكتشف الفيضان فوراً، وإيقافه يسمح للبيانات بالتسرب للعنوان.

3. **`-z execstack`**
   * **الوظيفة:** السماح بتنفيذ الأوامر والتعليمات البرمجية داخل الـ Stack.
   * **السبب:** الوضع الافتراضي يمنع تنفيذ الأكواد من الـ Stack (Non-Executable Stack)، ونحتاج لتفعيلها لتنفيذ الـ Shellcode المحقون.

4. **`-DBUF_SIZE=$(L1)`**
   * **الوظيفة:** ضبط حجم الـ Buffer ديناميكياً لتجهيز مستويات التحدي المختلفة ($L1, L2, L3, L4$).

---

## 5. Environment & Containers / بيئة العمل والحاويات

### لماذا يتم استخدام 4 خوادم (Docker Containers)؟
1. **محاكاة هجوم واقعي عبر الشبكة (Remote Exploitation):**
   يتصل برنامج السيرفر `server.c` بـ TCP Port `9090` ويربط اتصال الشبكة بالمدخلات القياسية للبرنامج المصاب `stack.c` ليحاكي بيئة سيرفرات حقيقية.
2. **التدرج في الصعوبة:**
   تختلف المستويات ($L1$ إلى $L4$) بحجم الـ `BUF_SIZE` ومعمارية الأنظمة (32-bit و 64-bit) لتحديد استراتيجية الهجوم المناسبة لكل مستوى.
3. **العزل والأمان (Isolation):**
   ضمان احتواء آثار الهجوم وانهيار البرامج داخل الحاوية دون التأثير على النظام الأب.

---

## 6. Execution Commands & Steps / خطوات وأوامر التنفيذ العملية

### Step 1: Disable ASLR Protection / إيقاف حماية العناوين العشوائية
```bash
sudo /sbin/sysctl -w kernel.randomize_va_space=0
```
* note: (ملاحظة: التأكد من ظهور الجملة kernel.randomize_va_space = 0 للتأكد من نجاح الإيقاف).

### Step 2: Compile & Install Server Binaries / تجميع الملفات وتثبيتها

```bash
cd server-code
make
make install
```
### Step 3: Run the Target Server Containers / تشغيل خوادم الهدف
```bash
cd ..
docker-compose up -d
```

---

## 7. Container Management & Useful Aliases / إدارة الحاويات والأوامر المختصرة

### Docker Compose Aliases / اختصارات إدارة البيئة:
* `dcbuild` : بديل لـ `docker-compose build` لبناء الصور.
* `dcup` : بديل لـ `docker-compose up` لتشغيل الحاويات.
* `dcdown` : بديل لـ `docker-compose down` لإيقاف الحاويات.

### Interacting with Containers / استعراض والدخول للحاويات:
1. **List running containers:**
   ```bash
   dockps
  ```
  * تستعرض الـ ID الخاص بكل حاوية واسمها والـ IP الخاص بها.
*Access a container's terminal (Shell):
```bash
docksh <container_id>
```

# Task 1: Shellcode Concept & Practical Guide
# التاسك الأول: مفهوم الـ Shellcode والتطبيق العملي

---

## 1. What is Shellcode? / ما هو الـ Shellcode؟

### English:
* **Definition:** Shellcode is a low-level piece of code, usually written in Assembly and converted into hexadecimal machine bytes.
* **How it Works:** It directly interacts with the CPU and system registers. During a Buffer Overflow attack, the malicious bytes are injected into memory, and the Return Address is hijacked to point to this code.
* **Execution:** The CPU reads these bytes as legitimate instructions and executes them with the privileges of the target program (e.g., Root privileges).

### العربية:
* **المفهوم:** الـ Shellcode هو كود منخفض المستوى (Low-level)، يُكتب عادةً بلغة التجميع (Assembly) ويُتحول إلى بايتات بلغة الآلة (Machine Code / Hexadecimal).
* **آلية العمل:** يتخاطب مباشرة مع المعالج (CPU) ومسجلات الذاكرة (Registers). عند نجاح ثغرة الـ Buffer Overflow، يتم حقن هذا الكود في الذاكرة وتغيير عنوان العودة (Return Address) ليرشد المعالج إليه.
* **التنفيذ:** يتعامل المعالج (CPU) مع هذه البايتات كأوامر رسمية وينفذها فوراً بنفس صلاحيات البرنامج المصاب (مثل صلاحيات Root).

---

## 2. Structure of the Provided Shellcode / تشريح الكود المعطى

### English:
The default Shellcode runs `/bin/bash` with specific arguments formatted in 3 main lines:
* **Line 1 (`/bin/bash`):** The target shell binary to execute.
* **Line 2 (`-c`):** Tells the shell to execute the command provided in the next line.
* **Line 3 (`/bin/ls -l; echo Hello; ...`):** The actual command string executed by the shell.
* **The Asterisk (`*`):** Serves as a position marker (Placeholder) that will be dynamically replaced by a null byte (`0x00`) during execution.

### العربية:
الـ Shellcode المرفق باللاب يقوم بتشغيل الباش `/bin/bash` ويمرر له وسائط مفسرة في 3 أسطر رئيسية:
* **السطر الأول (`/bin/bash`):** اسم البرنامج المراد تشغيله.
* **السطر الثاني (`-c`):** خيار يخبر الباش بتنفيذ الأمر المكتوب بعده مباشرة.
* **السطر الثالث (`/bin/ls -l; echo Hello; ...`):** سلسلة الأوامر الفعلية التي تتنفذ.
* **رمز النجمة (`*`):** خانة حجز مؤقتة (Placeholder) يتم استبدالها تلقائياً بـ Null Byte (`0x00`) أثناء التشغيل لتحديد نهاية النص.

---

## 3. Critical Modification Rule / شرط حتمي عند التعديل ⚠️

### English:
* **Fixed Memory Locations:** The Assembly code relies on fixed offsets to locate the array elements and placeholders.
* **Golden Rule:** When modifying **Line 3**, you MUST NOT change the overall length of the string.
* **How to Maintain Length:** If your new command is shorter than the original, add extra **Spaces** before the asterisk `*` to keep it in its exact original position.

### العربية:
* **عناوين ثابتة:** كود الأسمبلي المترجم يعتمد على مواقع وعناوين ذاكرة ثابتة لتحديد نهاية الأصوص.
* **القاعدة الذهبية:** عند تعديل **السطر الثالث**، يجب ألا يتغير الطول الإجمالي للنص نهائياً!
* **المحافظة على الطول:** إذا كان أمرك الجديد أقصر من الأمر القديم، أضف **مسافات (Spaces)** قبل علامة النجمة `*` لتعويض الفرق ولتبقى النجمة في نفس مكانها الأصلي بالضبط.

---

## 4. Task Requirement / المطلوب في هذا التاسك

### English:
Modify the Shellcode script (`shellcode_32.py` or `shellcode_64.py`) to execute a command that **deletes a file** (e.g., using `rm`), verify its execution, and document it with screenshots.

### العربية:
تعديل سكريبت الـ Shellcode ليعمل على **حذف ملف معين** (باستخدام أمر `rm` أو `/bin/rm`)، واختبار تنفيذه والتأكد من حذف الملف، وأخذ لقطات شاشة (Screenshots) للتقرير.

---

## 5. Execution Commands / الأوامر والخطوات العملية

```bash
# Step 1: Navigate to the shellcode folder
# الخطوة 1: الانتقال إلى مجلد الشيل كود
cd ~/Desktop/Labsetup/shellcode

# Step 2: Create a dummy file to test deletion
# الخطوة 2: إنشاء ملف تجريبي لتجربة الحذف عليه
touch myfile.txt

# Step 3: Edit the shellcode script (e.g., 32-bit version)
# الخطوة 3: فتح ملف الشيل كود والتعديل عليه
nano shellcode_32.py

# --- Inside the file, modify Line 3 to be: ---
# --- داخل الملف، عدلي السطر الثالث ليصبح كالتالي (مع إضافة مسافات لتعويض الطول): ---
# "/bin/rm myfile.txt                                *"

# Step 4: Generate the binary codefile
# الخطوة 4: توليد ملف الباينري
./shellcode_32.py

# Step 5: Compile the testing wrapper (call_shellcode.c)
# الخطوة 5: تجميع برنامج الاختبار
make

# Step 6: Test the modified shellcode
# الخطوة 6: تشغيل واختبار الـ Shellcode
./a32.out

# Step 7: Verify that the file was deleted
# الخطوة 7: التأكد من اختفاء الملف وحذفه
ls myfile.txt
```

# Task 2: Level-1 Remote Buffer Overflow Attack
# التاسك الثاني: تنفيذ هجوم طفح الذاكرة عبر الشبكة (المستوى الأول)

---

## 1. Overview & Goal / نظرة عامة والهدف

### English:
* **Goal:** Exploit a 32-bit vulnerable server (`10.9.0.5:9090`) to hijack execution flow using a Buffer Overflow payload (`badfile`).
* **Environment Requirement:** Must have active network connection (e.g., Huawei Connect) and running Docker containers via `dcup`.

### العربية:
* **الهدف:** استغلال ثغرة Buffer Overflow في سيرفر يعمل ببيئة 32-bit على العنوان (`10.9.0.5:9090`) للسيطرة على سريان تنفيذ الكود باستخدام حمولة خبيثة (`badfile`).
* **المتطلبات الأساسية:** تفعيل توصيل الشبكة (Huawei Connect) وتشغيل الحاويات باستخدام `dcup`.

---

## 2. Live Extracted Memory Values / عناوين الذاكرة المستخرجة من السيرفر

### Values Captured / القيم المستخرجة:
* **Frame Pointer (`ebp`):** `0xffffd248`
* **Buffer Address:** `0xffffd188`

---

## 3. Mathematical Calculations / الحسابات الرياضية للثغرة

### A. Offset Calculation / حساب الإزاحة لتحديد موقع الـ EIP:
$$\text{Difference} = \text{ebp} - \text{Buffer Address}$$
$$\text{Difference} = \text{0xffffd248} - \text{0xffffd188} = \text{0xC0} = 192 \text{ bytes}$$

$$\text{Offset} = \text{Difference} + 4 = 192 + 4 = 196 \text{ bytes}$$

> **Note / ملاحظة:** We add `4` bytes because the Return Address (`EIP`) is stored right after the `ebp` register in 32-bit architecture.
> (نضيف 4 بايت لأن عنوان العودة يقع مباشرة بعد مسجل الـ `ebp` في المعماريات ذات الـ 32 بايت).

### B. Target Return Address / عنوان العودة الجديد:
$$\text{Target Address} = \text{Buffer Address} + \text{Safe Offset (e.g., 0x100 / 256 bytes)}$$
$$\text{Target Address} = \text{0xffffd188} + \text{0x100} = \text{0xffffd288}$$

---

## 4. Full Execution Workflow & Commands / خطة العمل والأوامر الكاملة

```bash
# ==============================================================================
# Step 1: Ensure Prerequisites (VPN & Containers)
# الخطوة 1: التأكد من تشغيل الشبكة والحاويات في الترمينال الأول
# ==============================================================================
# (In Terminal 1 / في الترمينال الأول):
cd ~/Desktop/Labsetup
dcup

# ==============================================================================
# Step 2: Open a New Terminal Window & Navigate to Attack Directory
# الخطوة 2: فتح ترمينال جديد والانتقال لمجلد الهجوم
# ==============================================================================
cd ~/Desktop/Labsetup/attack-code

# ==============================================================================
# Step 3: Modify exploit.py with calculated values
# الخطوة 3: تعديل ملف exploit.py بالقيم التي حسبناها
# ==============================================================================
nano exploit.py

# Inside exploit.py update the following parameters:
# داخل الملف قم بتحديث المتغيرات التالية:
# shellcode = ... (Ensure your 32-bit shellcode is uncommented)
# start = 517 - len(shellcode)
# ret = 0xffffd288      # Target Address (Buffer + 0x100)
# offset = 196          # Calculated Offset (ebp - buffer + 4)

# Save & Exit nano: Press Ctrl+O -> Enter -> Ctrl+X

# ==============================================================================
# Step 4: Generate Payload & Launch Attack
# الخطوة 4: إنشاء ملف الحمولة وإرساله للسيرفر
# ==============================================================================
# Generate the badfile payload / توليد ملف الـ Payload
./exploit.py

# Send payload to vulnerable container / إرسال الـ Payload للهدف
cat badfile | nc 10.9.0.5 9090

# ==============================================================================
# Troubleshooting / حل المشاكل
# ==============================================================================
# If nc hangs indefinitely: Verify Huawei Connect is active and containers are running.
# إذا علّق أمر nc: تأكد من تشغيل Huawei Connect وأن الحاويات تعمل بنجاح.


# Section 4.2 & Reverse Shell Concepts (Task 2 Continuation)
# التكملة النظرية للتاسك الثاني ومفهوم الـ Reverse Shell

---

## 1. Overview & Concepts / المفاهيم الأساسية

### English:
* **The Challenge:** Executing `/bin/sh` on a remote vulnerable server does not automatically provide an interactive terminal for the attacker.
* **The Solution (Reverse Shell):** Instruct the victim server to initiate an outbound network connection back to the attacker's listener machine, delivering an interactive root shell prompt.

### العربية:
* **التحدي:** تنفيذ أمر `/bin/sh` على سيرفر بعيد لا يعطي المهاجم واجهة أوامر تفاعلية بشكل مباشر.
* **الحل (الـ Reverse Shell):** إجبار السيرفر المصاب على إنشاء اتصال شبكي عكسي يتصل بجهاز المهاجم عبر منفذ محدد، مما يمنح المهاجم صلاحية التحكم الكامل بالـ Root.

---

## 2. Key Payload Components / مكونات سكريبت الهجوم

* **NOP Sled (`0x90`):** Buffer padding that safely channels the CPU execution pointer to the shellcode.
* **Return Address (`ret`):** Pointer calculated to override `EIP` and jump into the NOP Sled.
* **Offset:** Exact memory distance calculated via $Offset = (ebp - Buffer) + 4$.
* **Shellcode String:** Assembly bytes carrying the instructions to initiate the reverse connection.

---

## 3. Conceptual Workflow / تسلسل العمل النظري

1. **Set up Listener:** Attacker starts a socket listener on a designated IP and port.
2. **Extract Addresses:** Capture current `ebp` and `Buffer` values from target server logs.
3. **Calculate Parameters:** Determine offset and target return address ($Buffer + 0x100$).
4. **Generate & Deliver Payload:** Run `exploit.py` to construct `badfile` and send it via `nc`.

ملف السكريبت (attack.sh)
```bash

echo "[+] Generating badfile using exploit.py..."
./exploit.py

echo "[+] Sending badfile to the target container (10.9.0.5:9090)..."
cat badfile | nc 10.9.0.5 9090

echo "[+] Done! Check your listener terminal or server logs."
```
طريقة التشغيل بأسلوب مباشر:
نشئي الملف وافتحيه للتعديل:

```Bash
nano attack.sh
chmod +x attack.sh
./attack.sh
```




# SEED Labs: Buffer Overflow Lab - Task 3 (Level-2 Attack)

## 📌 Overview / نظرة عامة
* **English:** Task 3 introduces a higher difficulty level by hiding the exact buffer size and frame pointer value. The buffer size is known to be within a range of **[100, 300]** bytes, and the goal is to construct a **single, robust payload** that succeeds for any size within this range without relying on brute-force methods.
* **العربية:** يقدم "Task 3" مستوى صعوبة أعلى عبر إخفاء الحجم الدقيق للـ Buffer وقيمة الـ frame pointer. من المعروف أن حجم المخزن المؤقت محصور في نطاق **[100, 300]** بايت، والهدف هو بناء **بايلود واحد فعال** ينجح لأي حجم ضمن هذا النطاق دون الحاجة لأساليب التخمين العشوائي (Brute-force).

---

## 🎯 Objectives / الأهداف
* **English:** 
  1. Handle unknown buffer sizes within the [100, 300] byte range.
  2. Implement range-based addressing strategies.
  3. Ensure a single payload execution successfully reaches the target shellcode.
* **العربية:** 
  1. التعامل مع أحجام الـ Buffer المجهولة ضمن مدى [100, 300] بايت.
  2. تطبيق استراتيجيات العنونة المعتمدة على النطاقات.
  3. ضمان نجاح تنفيذ بايلود واحد للوصول إلى الـ shellcode المطلوب.

---

## 🛠️ Methodology & Strategy / الآلية والاستراتيجية
* **English:**
  * **NOP Sled:** Prepending a large sequence of `NOP` (`0x90`) instructions to widen the successful landing zone for the instruction pointer ($EIP$).
  * **Range Coverage:** Designing the exploit script to cover potential stack locations across the 100 to 300 byte boundary, ensuring memory alignment and accounting for the 4-byte multiples of 32-bit architecture frame pointers.
* **العربية:**
  * **ممر الـ NOP (`NOP Sled`):** وضع سلسلة طويلة من تعليمات `NOP` (`0x90`) لتوسيع مساحة الهبوط الناجحة لمؤشر التعليمات ($EIP$).
  * **تغطية النطاق:** تصميم سكريبت الاستغلال لتغطية مواقع الـ Stack المحتملة عبر حدود الـ 100 إلى 300 بايت، مع مراعاة محاذاة الذاكرة ومضاعفات الأربعة بايت الخاصة بمعمارية الـ 32-bit.

---

## ⚙️ Execution Steps / خطوات التنفيذ
* **English:**
  1. Edit and generate the payload configuration using the exploit script (`nano exploit.py` then `./exploit.py`).
  2. Send the constructed `badfile` to the vulnerable target container via Netcat (`cat badfile | nc 10.9.0.6 9090`).
  3. Verify the execution state and confirm proper returning or shell access without triggering defensive blocks.
* **العربية:**
  1. تعديل وإنشاء إعدادات البايلود باستخدام سكريبت الاستغلال (`nano exploit.py` ثم `./exploit.py`).
  2. إرسال ملف `badfile` الناتج إلى الحاوية المستهدفة عبر أداة Netcat (`cat badfile | nc 10.9.0.6 9090`).
  3. التحقق من حالة التنفيذ والتأكد من العودة السليمة أو الحصول على الـ shell دون تفعيل أنظمة الحماية.
أعطيه صلاحية التنفيذ وشغليه بضغطة واحدة:
