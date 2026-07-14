# ใบงานการทดลองที่ 2-1
# Dart Programming Fundamentals
### วิชา: การพัฒนาซอฟต์แวร์สำหรับอุปกรณ์เคลื่อนที่

| | |
|--|--|
| **สัปดาห์ที่** | 2 |
| **ใบงานที่** | 2-1 จาก 2 |
| **เวลา** | 2 ชั่วโมง 30 นาที |
| **เครื่องมือ** | DartPad (dartpad.dev) |

---

## วัตถุประสงค์

เมื่อสิ้นสุดการทดลอง นักศึกษาสามารถ:

1. เขียนโปรแกรม Dart ที่มี Type System, Null Safety, Collection ได้ถูกต้อง
2. ออกแบบและเขียน Function ทั้งแบบ Named Parameter, Optional Parameter และ Higher-order Function ได้
3. ออกแบบ Class ที่มี Constructor, Getter, Inheritance, Abstract Class และ Mixin ได้
4. เขียนโปรแกรม Async ด้วย Future, async/await และ Future.wait ได้ พร้อมจัดการ Error
5. อ่าน Error Message ของ Dart และแก้ไขได้ด้วยตนเอง

---

## การเตรียมตัวก่อนทดลอง

ใบงานนี้ใช้ **DartPad** ทั้งหมด ไม่จำเป็นต้องติดตั้งโปรแกรมใดๆ เพิ่มเติม

1. เปิด Browser (แนะนำ Chrome หรือ Edge)
2. ไปที่ **https://dartpad.dev**
3. ตรวจสอบว่าหน้าตาเป็นดังนี้

```
┌─────────────────────────────────────────────────────┐
│  DartPad                              [Dart ▼] [Run]│
├───────────────────────────┬─────────────────────────┤
│                           │                         │
│   บริเวณเขียนโค้ด            │   บริเวณแสดงผล           │
│   (Code Editor)           │   (Console Output)      │
│                           │                         │
└───────────────────────────┴─────────────────────────┘
```

> **หมายเหตุ:** ตรวจสอบว่าเลือก **Dart** (ไม่ใช่ Flutter) ที่ Dropdown มุมบนขวา ก่อนเริ่มทุกการทดลอง

---

## ส่วนที่ 1 — ทฤษฎีและการทดลอง: Variables, Types และ Null Safety

### ทฤษฎี 1.1 — ระบบชนิดข้อมูล (Type System)

Dart เป็นภาษา **Strongly Typed** หมายความว่าทุกตัวแปรมีชนิดข้อมูลที่แน่นอน และชนิดนั้นไม่เปลี่ยนตลอดอายุการใช้งาน ซึ่งต่างจากภาษา Dynamic เช่น Python ที่ตัวแปรเดียวกันสามารถเป็นได้ทั้ง String และ Number

**ชนิดข้อมูลพื้นฐานใน Dart:**

| ชนิด | ความหมาย | ตัวอย่างค่า |
|------|----------|-----------|
| `int` | จำนวนเต็ม | `0`, `42`, `-10` |
| `double` | จำนวนทศนิยม | `3.14`, `2.0`, `-0.5` |
| `String` | ข้อความ | `"สวัสดี"`, `'hello'` |
| `bool` | ค่าจริง/เท็จ | `true`, `false` |
| `List<T>` | รายการ (Array) | `[1, 2, 3]` |
| `Map<K,V>` | คู่ Key-Value | `{"name": "ชาย"}` |
| `Set<T>` | ชุดไม่ซ้ำ | `{1, 2, 3}` |

**การประกาศตัวแปร:**

```dart
// แบบที่ 1: ระบุ Type ชัดเจน — อ่านง่าย รู้ Type ทันที
String name = "สมชาย";
int age = 20;
double gpa = 3.75;

// แบบที่ 2: ใช้ var — Dart อนุมาน Type จากค่าที่กำหนด (Type Inference)
var city = "กรุงเทพฯ";  // Dart รู้ว่าเป็น String
var score = 95;           // Dart รู้ว่าเป็น int

// แบบที่ 3: final — กำหนดค่าได้ครั้งเดียว (Runtime Constant)
final birthYear = 2004;    // เปลี่ยนหลังกำหนดไม่ได้
final now = DateTime.now(); // ค่าถูกกำหนด ณ Runtime

// แบบที่ 4: const — ค่าคงที่ Compile Time (ต้องรู้ค่าก่อนรัน)
const pi = 3.14159;
const maxScore = 100;
// const now = DateTime.now(); ← ❌ Error! DateTime.now() ไม่รู้ก่อนรัน
```

**String Interpolation** — การฝังตัวแปรใน String:

```dart
String name = "สมชาย";
int age = 20;

// วิธีที่ 1: $variable — ใช้กับตัวแปรเดี่ยว
print("ชื่อ: $name");           // → ชื่อ: สมชาย

// วิธีที่ 2: ${expression} — ใช้กับ Expression ที่ซับซ้อน
print("อายุ: ${age} ปี");       // → อายุ: 20 ปี
print("ปีเกิด: ${2025 - age}"); // → ปีเกิด: 2005
print("ชื่อ: ${name.toUpperCase()}"); // → ชื่อ: สมชาย (ตัวพิมพ์ใหญ่)
```

---

### ทฤษฎี 1.2 — Null Safety

ก่อนที่ Dart จะมี Null Safety โปรแกรมเมอร์มักพบ Error ที่ทำให้แอป Crash แบบนี้:

```
Unhandled Exception: Null check operator used on a null value
```

Dart 2.12+ แก้ปัญหานี้ด้วยระบบ **Sound Null Safety** — Compiler จะ**ไม่ยอม**ให้โค้ดที่อาจเกิด Null Error ผ่านได้

```
ตัวแปร Dart แบ่งเป็น 2 ประเภท:

Non-nullable (default):          Nullable (เพิ่ม ?):
┌─────────────────────┐         ┌─────────────────────┐
│  String name        │         │  String? nickname   │
│  ┌───────────────┐  │         │  ┌───────────────┐  │
│  │ ต้องมีค่า        │  │         │  │ มีค่า หรือ       │  │
│  │ เสมอ!         │  │         │  │ null ก็ได้      │  │
│  └───────────────┘  │         │  └───────────────┘  │
└─────────────────────┘         └─────────────────────┘
```

**Null-aware Operators — เครื่องมือจัดการ Null อย่างปลอดภัย:**

```dart
String? nickname = null;

// 1. ?? (Null Coalescing) — "ถ้า null ให้ใช้ค่าขวาแทน"
String display = nickname ?? "ไม่มีชื่อเล่น";
print(display); // → ไม่มีชื่อเล่น

// 2. ?. (Null-aware method call) — "เรียก method เฉพาะเมื่อไม่ null"
int? length = nickname?.length;
print(length);  // → null (ไม่ Crash)

// 3. ??= (Null-aware assignment) — "กำหนดค่าเฉพาะถ้าตัวแปรเป็น null"
nickname ??= "ชื่อเล่นเริ่มต้น";
print(nickname); // → ชื่อเล่นเริ่มต้น

// 4. ! (Null Assertion) — "ฉันมั่นใจว่าไม่ null" ⚠️ ใช้ด้วยความระวัง
String definitelyNotNull = "ค่าจริง";
String? maybeNull = definitelyNotNull;
print(maybeNull!.length); // → 8 (ถ้าผิดพลาดและเป็น null จะ Crash)
```

**Collections:**

```dart
// List — รายการที่มีลำดับ เพิ่ม/ลบได้
List<String> fruits = ["แอปเปิล", "กล้วย", "ส้ม"];
fruits.add("มะม่วง");
fruits.remove("กล้วย");
print(fruits.length);    // → 3
print(fruits[0]);        // → แอปเปิล
print(fruits.first);     // → แอปเปิล
print(fruits.last);      // → ส้ม

// Map — คู่ Key-Value
Map<String, int> scores = {
  "คณิตศาสตร์": 85,
  "วิทยาศาสตร์": 92,
  "ภาษาไทย": 78,
};
scores["ภาษาอังกฤษ"] = 88;  // เพิ่ม entry ใหม่
print(scores["คณิตศาสตร์"]); // → 85
print(scores["ชีววิทยา"]);   // → null (ไม่มี Key นี้)

// วนซ้ำใน Map
scores.forEach((subject, score) {
  print("$subject: $score");
});

// Set — ชุดข้อมูลที่ไม่มีซ้ำ
Set<String> tags = {"dart", "flutter", "mobile"};
tags.add("dart");  // ไม่เพิ่ม เพราะซ้ำอยู่แล้ว
print(tags.length); // → 3
```

---

### การทดลอง 1.1 — Variables, Types และ Collections

**⏱ เวลา:** 20 นาที

**ขั้นตอนที่ 1** เปิด dartpad.dev เลือก Dart แล้วล้างโค้ดเดิมออก

**ขั้นตอนที่ 2** พิมพ์โค้ดต่อไปนี้ทีละบล็อก อ่านทำความเข้าใจก่อนกด Run

```dart
void main() {
  // === บล็อกที่ 1: ชนิดข้อมูลพื้นฐาน ===
  String studentName = "สมชาย ดีใจ";
  int studentAge = 20;
  double gpa = 3.75;
  bool isEnrolled = true;

  print("=== ข้อมูลนักศึกษา ===");
  print("ชื่อ: $studentName");
  print("อายุ: $studentAge ปี");
  print("GPA: $gpa");
  print("ลงทะเบียนแล้ว: $isEnrolled");
  print("ปีเกิด (ประมาณ): ${2026 - studentAge}");
}
```

**ขั้นตอนที่ 3** กด **Run** ตรวจสอบผลลัพธ์ที่ได้

**ขั้นตอนที่ 4** เพิ่มโค้ดต่อไปนี้ **ต่อท้าย** ภายใน `main()` ก่อนปิด `}`

```dart
  // === บล็อกที่ 2: Null Safety ===
  print("\n=== Null Safety ===");
  String? nickname = null;
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ไม่มี

  nickname = "ชาย";
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ชาย
  print("ความยาว: ${nickname?.length}");       // → 3
  print("ตัวพิมพ์ใหญ่: ${nickname?.toUpperCase()}"); // → ชาย
```

**ขั้นตอนที่ 5** กด Run อีกครั้ง สังเกตผลลัพธ์ที่เพิ่มขึ้น

**ขั้นตอนที่ 6** เพิ่มโค้ด Collections ต่อท้าย

```dart
  // === บล็อกที่ 3: List ===
  print("\n=== รายวิชาที่ลงทะเบียน ===");
  List<String> courses = ["Mobile Dev", "Web Dev", "AI"];
  Map<String, int> courseScores = {
    "Mobile Dev": 90,
    "Web Dev": 85,
    "AI": 92,
  };

  // วนซ้ำแสดงรายวิชาและคะแนน
  for (int i = 0; i < courses.length; i++) {
    String course = courses[i];
    int? score = courseScores[course];
    print("${i + 1}. $course: ${score ?? 'ยังไม่มีคะแนน'} คะแนน");
  }

  // คำนวณเฉลี่ย
  int total = courseScores.values.reduce((a, b) => a + b);
  double avg = total / courseScores.length;
  print("คะแนนเฉลี่ย: ${avg.toStringAsFixed(2)}");
```

**ขั้นตอนที่ 7** กด Run และบันทึกผลลัพธ์ทั้งหมด
<img width="1912" height="1032" alt="image" src="https://github.com/user-attachments/assets/d7584229-8f09-4f33-b9f1-c99e0685c3c9" />

---

### 🎯 โจทย์ฝึกทำ 1.1 — แก้ไขและเพิ่มเติมโค้ดด้วยตนเอง

แก้ไขโค้ดที่มีอยู่ให้ทำสิ่งต่อไปนี้ได้ครบ:

1. เพิ่มรายวิชา "Database" ที่มีคะแนน 88 ลงใน `courses` และ `courseScores`
2. หาวิชาที่มีคะแนนสูงสุดโดยใช้ `courseScores.entries` และ `.reduce()` แล้วพิมพ์ว่า "วิชาที่ได้คะแนนสูงสุด: ..."
3. นับจำนวนวิชาที่ได้คะแนน >= 90 แล้วพิมพ์ผล
4. สร้าง `Set<String>` ชื่อ `passedCourses` ที่เก็บเฉพาะวิชาที่ได้คะแนน >= 80 แล้วพิมพ์รายการ

**ผลลัพธ์ที่คาดหวัง (ตัวอย่าง):**
```
วิชาที่ได้คะแนนสูงสุด: AI (92 คะแนน)
จำนวนวิชาที่ได้ >= 90: 2 วิชา
วิชาที่ผ่าน: {Mobile Dev, Web Dev, AI, Database}
```
**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
void main() {
  // === บล็อกที่ 1: ชนิดข้อมูลพื้นฐาน ===
  String studentName = "สมชาย ดีใจ";
  int studentAge = 20;
  double gpa = 3.75;
  bool isEnrolled = true;

  print("=== ข้อมูลนักศึกษา ===");
  print("ชื่อ: $studentName");
  print("อายุ: $studentAge ปี");
  print("GPA: $gpa");
  print("ลงทะเบียนแล้ว: $isEnrolled");
  print("ปีเกิด (ประมาณ): ${2026 - studentAge}");
  
  // === บล็อกที่ 2: Null Safety ===
  print("\n=== Null Safety ===");
  String? nickname = null;
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ไม่มี

  nickname = "ชาย";
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ชาย
  print("ความยาว: ${nickname?.length}");       // → 3
  print("ตัวพิมพ์ใหญ่: ${nickname?.toUpperCase()}"); // → ชาย
  
  // === บล็อกที่ 3: List ===
  print("\n=== รายวิชาที่ลงทะเบียน ===");
  List<String> courses = ["Mobile Dev", "Web Dev", "AI"];
  Map<String, int> courseScores = {
    "Mobile Dev": 90,
    "Web Dev": 85,
    "AI": 92,
  };

  // ----------------------------------------------------
  // จุดที่ 1 เพิ่ม Database ลงใน List และ Map
  // ----------------------------------------------------
  courses.add("Database");
  courseScores["Database"] = 88;

  // วนซ้ำแสดงรายวิชาและคะแนน
  for (int i = 0; i < courses.length; i++) {
    String course = courses[i];
    int? score = courseScores[course];
    print("${i + 1}. $course: ${score ?? 'ยังไม่มีคะแนน'} คะแนน");
  }

  // คำนวณเฉลี่ย
  int total = courseScores.values.reduce((a, b) => a + b);
  double avg = total / courseScores.length;
  print("คะแนนเฉลี่ย: ${avg.toStringAsFixed(2)}");

  // ----------------------------------------------------
  // จุดที่ 2 เพิ่มฟังก์ชันวิเคราะห์ข้อมูลเพิ่มเติมก่อนปิด main
  // ----------------------------------------------------
  print(""); // เว้นบรรทัดให้อ่านง่าย

  // หาวิชาที่มีคะแนนสูงสุดด้วย entries และ reduce
  var highestCourse = courseScores.entries.reduce((a, b) => a.value > b.value ? a : b);
  print("วิชาที่ได้คะแนนสูงสุด: ${highestCourse.key} (${highestCourse.value} คะแนน)");

  // นับจำนวนวิชาที่ได้คะแนน >= 90
  int count90 = courseScores.values.where((score) => score >= 90).length;
  print("จำนวนวิชาที่ได้ >= 90: $count90 วิชา");

  // สร้าง Set<String> ของวิชาที่ผ่าน (คะแนน >= 80)
  Set<String> passedCourses = courseScores.entries
      .where((entry) => entry.value >= 80)
      .map((entry) => entry.key)
      .toSet();
  print("วิชาที่ผ่าน: $passedCourses");
}

```
<img width="1906" height="1078" alt="image" src="https://github.com/user-attachments/assets/72e59248-d371-4da0-a291-9023e3c6d615" />

---

## ส่วนที่ 2 — ทฤษฎีและการทดลอง: Functions

### ทฤษฎี 2.1 — รูปแบบ Function ใน Dart

Function คือหน่วยของโค้ดที่แยกออกมาเพื่อทำงานเฉพาะอย่าง ช่วยให้ไม่ต้องเขียนโค้ดซ้ำ

**รูปแบบ Function พื้นฐาน:**

```dart
// โครงสร้าง: ReturnType functionName(ParameterType paramName) { ... }

// Function ที่คืนค่า String
String greet(String name) {
  return "สวัสดี $name!";
}

// Function ที่ไม่คืนค่า (void)
void printDivider(int length) {
  print("─" * length);
}

// Arrow Function: ย่อเมื่อ body มีแค่ return expression เดียว
String greetArrow(String name) => "สวัสดี $name!";
double square(double x) => x * x;
bool isAdult(int age) => age >= 18;
```

**Positional Parameters — ต้องส่งตามลำดับ:**

```dart
// Required positional — ต้องส่งครบทุกตัว
double calculateBMI(double weight, double height) {
  return weight / (height * height);
}
print(calculateBMI(70, 1.75)); // → 22.86 ✅
// print(calculateBMI(1.75, 70)); // ✅ รัน แต่ผิดความหมาย!

// Optional positional — ใส่ [] รอบ Parameter ที่ไม่บังคับ
String formatName(String firstName, [String? lastName, String title = ""]) {
  if (lastName != null) {
    return "$title $firstName $lastName".trim();
  }
  return "$title $firstName".trim();
}
print(formatName("สมชาย"));              // → สมชาย
print(formatName("สมชาย", "ดีใจ"));      // → สมชาย ดีใจ
print(formatName("สมชาย", "ดีใจ", "นาย")); // → นาย สมชาย ดีใจ
```

**Named Parameters — ต้องระบุชื่อเมื่อเรียก:**

```dart
// Named parameters ใส่ {} รอบ — ลำดับไม่สำคัญ
void createProfile({
  required String name,    // required = บังคับส่ง
  required String email,   // required = บังคับส่ง
  int age = 0,             // optional + default value
  String? bio,             // optional nullable
}) {
  print("ชื่อ: $name");
  print("Email: $email");
  print("อายุ: $age ปี");
  if (bio != null) print("Bio: $bio");
}

// เรียกใช้ — ไม่ต้องสนใจลำดับ แต่ต้องระบุชื่อ
createProfile(
  name: "สมชาย",
  email: "somchai@example.com",
  age: 20,
  bio: "นักศึกษา KMITL",
);

createProfile(
  email: "guest@example.com",  // ลำดับต่างกันก็ได้
  name: "ผู้เยี่ยมชม",
  // age ไม่ส่งก็ได้ มีค่า default เป็น 0
);
```

---

### ทฤษฎี 2.2 — Higher-Order Functions

**Higher-order Function** คือ Function ที่รับ Function อื่นเป็น Parameter หรือคืนค่าเป็น Function ซึ่งทำให้เขียนโค้ดที่ยืดหยุ่นและกระชับมากขึ้น

```
แนวคิด:
ปกติเราส่ง ตัวเลข, ข้อความ เป็น Parameter
Higher-order Function ส่ง Function เป็น Parameter ได้ด้วย!

f(x) = x * 2          ← Function ธรรมดา
g(f, x) = f(x) + 1    ← Higher-order Function (รับ f เป็น parameter)
```

**Collection Methods ที่ใช้บ่อย:**

```dart
List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// where() — กรองรายการตามเงื่อนไข (คืน Iterable)
var evens = numbers.where((n) => n % 2 == 0).toList();
print(evens); // → [2, 4, 6, 8, 10]

// map() — แปลงทุก element (คืน Iterable ของ Type ใหม่)
var doubled = numbers.map((n) => n * 2).toList();
print(doubled); // → [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

var asString = numbers.map((n) => "No.$n").toList();
print(asString); // → [No.1, No.2, ...]

// reduce() — รวมทั้งหมดเป็นค่าเดียว
int sum = numbers.reduce((acc, n) => acc + n);
print(sum); // → 55

int max = numbers.reduce((a, b) => a > b ? a : b);
print(max); // → 10

// any() — มี element ใดสอดคล้องเงื่อนไขไหม?
bool hasNegative = numbers.any((n) => n < 0);
print(hasNegative); // → false

// every() — ทุก element สอดคล้องเงื่อนไขไหม?
bool allPositive = numbers.every((n) => n > 0);
print(allPositive); // → true

// sort() — เรียงลำดับ (แก้ List เดิม)
List<int> scores = [85, 42, 96, 71, 58];
scores.sort((a, b) => b.compareTo(a)); // เรียงจากมากไปน้อย
print(scores); // → [96, 85, 71, 58, 42]
```

**Function เป็น Variable:**

```dart
// เก็บ Function ในตัวแปร
String Function(String) shout = (s) => s.toUpperCase() + "!!!";
print(shout("hello")); // → HELLO!!!

// ส่ง Function เป็น Parameter
void applyToList(List<int> list, void Function(int) action) {
  for (var item in list) {
    action(item);
  }
}

applyToList([1, 2, 3], (n) => print("ค่า: $n"));
// → ค่า: 1
// → ค่า: 2
// → ค่า: 3
```

---

### การทดลอง 2.1 — Functions พื้นฐาน

**⏱ เวลา:** 20 นาที

**ขั้นตอนที่ 1** ล้างโค้ดใน DartPad แล้วพิมพ์โค้ดต่อไปนี้

```dart
// === Function พื้นฐาน ===
String gradeLabel(double gpa) {
  if (gpa >= 3.5) return "เกียรตินิยมอันดับ 1";
  if (gpa >= 3.25) return "เกียรตินิยมอันดับ 2";
  if (gpa >= 3.0) return "ดีมาก";
  if (gpa >= 2.5) return "ดี";
  if (gpa >= 2.0) return "พอใช้";
  return "ต่ำกว่าเกณฑ์";
}

// === Arrow Function ===
double average(List<double> nums) =>
    nums.reduce((a, b) => a + b) / nums.length;

bool isHonors(double gpa) => gpa >= 3.25;

// === Named Parameters ===
void printStudent({
  required String name,
  required double gpa,
  int year = 1,
  String? major,
}) {
  print("─────────────────────────");
  print("ชื่อ: $name (ปีที่ $year)");
  if (major != null) print("สาขา: $major");
  print("GPA: $gpa → ${gradeLabel(gpa)}");
  print("เกียรตินิยม: ${isHonors(gpa) ? "✅ ใช่" : "❌ ไม่"}");
}

void main() {
  print("=== รายชื่อนักศึกษา ===\n");

  printStudent(name: "สมชาย", gpa: 3.75, year: 3, major: "เทคโนโลยีคอมพิวเตอร์");
  printStudent(name: "สมหญิง", gpa: 2.90, year: 2);
  printStudent(name: "สมศักดิ์", gpa: 3.30, year: 4, major: "เทคโนโลยีคอมพิวเตอร์");

  print("\n=== GPA เฉลี่ยทั้งชั้น ===");
  List<double> allGpas = [3.75, 2.90, 3.30];
  print("เฉลี่ย: ${average(allGpas).toStringAsFixed(2)}");
}
```

**ขั้นตอนที่ 2** กด Run สังเกตผลลัพธ์

**ขั้นตอนที่ 3** ทดลองเปลี่ยนค่า `gpa` ในแต่ละ `printStudent()` เพื่อดูว่า Label เปลี่ยนอย่างไร

---

### การทดลอง 2.2 — Higher-Order Functions

**⏱ เวลา:** 25 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ดต่อไปนี้

```dart
void main() {
  List<Map<String, dynamic>> students = [
    {"name": "สมชาย",  "gpa": 3.75, "year": 3, "faculty": "วิศวกรรม"},
    {"name": "สมหญิง", "gpa": 2.50, "year": 1, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศักดิ์","gpa": 3.10, "year": 2, "faculty": "วิศวกรรม"},
    {"name": "สมใจ",  "gpa": 1.80, "year": 4, "faculty": "บริหาร"},
    {"name": "สมปอง", "gpa": 3.50, "year": 2, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศรี", "gpa": 2.90, "year": 3, "faculty": "บริหาร"},
  ];

  // === where() — กรองนักศึกษาที่ GPA >= 3.0 ===
  print("=== นักศึกษาที่ GPA >= 3.0 ===");
  var honorStudents = students
      .where((s) => (s["gpa"] as double) >= 3.0)
      .toList();
  for (var s in honorStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }

  // === map() — แปลงเป็น String รายงาน ===
  print("\n=== รายงานนักศึกษา ===");
  var report = students
      .map((s) => "${s["name"]} (${s["faculty"]}) GPA: ${s["gpa"]}")
      .toList();
  report.forEach(print);

  // === sort() + reduce() ===
  print("\n=== วิเคราะห์คะแนน ===");
  List<double> gpas = students.map((s) => s["gpa"] as double).toList();

  double maxGpa = gpas.reduce((a, b) => a > b ? a : b);
  double minGpa = gpas.reduce((a, b) => a < b ? a : b);
  double avgGpa = gpas.reduce((a, b) => a + b) / gpas.length;

  print("GPA สูงสุด: $maxGpa");
  print("GPA ต่ำสุด: $minGpa");
  print("GPA เฉลี่ย: ${avgGpa.toStringAsFixed(2)}");

  // === any() และ every() ===
  bool anyFailing = students.any((s) => (s["gpa"] as double) < 2.0);
  bool allPassing = students.every((s) => (s["gpa"] as double) >= 2.0);
  print("มีนักศึกษาที่ GPA < 2.0: $anyFailing");
  print("ทุกคน GPA >= 2.0: $allPassing");
}
```

**ขั้นตอนที่ 2** กด Run และอ่านผลลัพธ์ทุกส่วน

**ขั้นตอนที่ 3** เพิ่มโค้ดต่อท้ายใน `main()` เพื่อกรองนักศึกษาเฉพาะคณะ "วิศวกรรม" แล้วแสดงผล

```dart
  // ทดลองเพิ่มเอง: กรองเฉพาะคณะวิศวกรรม
  print("\n=== นักศึกษาคณะวิศวกรรม ===");
  var engineeringStudents = students
      .where((s) => s["faculty"] == "วิศวกรรม")
      .toList();
  // พิมพ์ชื่อและ GPA ของนักศึกษาแต่ละคน
  for (var s in engineeringStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }
```

---

### 🎯 โจทย์ฝึกทำ 2 — เขียน Function ด้วยตนเอง

เขียนโค้ดโดยใช้ List ของนักศึกษาจากการทดลอง 2.2 เพิ่มเติม:

1. เขียน Function `findTopStudentByFaculty(List students, String faculty)` ที่คืนชื่อนักศึกษาที่ GPA สูงสุดในคณะที่ระบุ
2. เขียน Function `groupByFaculty(List students)` ที่คืน `Map<String, List>` โดยจัดกลุ่มนักศึกษาตามคณะ
3. ใช้ `sort()` เรียงนักศึกษาตาม GPA จากสูงไปต่ำ แล้วพิมพ์ข้อมูลนักศึกษาที่มี GPA สูงสุด 3 อันดับแรก

**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
void main() {
  List<Map<String, dynamic>> students = [
    {"name": "สมชาย",  "gpa": 3.75, "year": 3, "faculty": "วิศวกรรม"},
    {"name": "สมหญิง", "gpa": 2.50, "year": 1, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศักดิ์","gpa": 3.10, "year": 2, "faculty": "วิศวกรรม"},
    {"name": "สมใจ",  "gpa": 1.80, "year": 4, "faculty": "บริหาร"},
    {"name": "สมปอง", "gpa": 3.50, "year": 2, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศรี", "gpa": 2.90, "year": 3, "faculty": "บริหาร"},
  ];

  // === where() — กรองนักศึกษาที่ GPA >= 3.0 ===
  print("=== นักศึกษาที่ GPA >= 3.0 ===");
  var honorStudents = students
      .where((s) => (s["gpa"] as double) >= 3.0)
      .toList();
  for (var s in honorStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }

  // === map() — แปลงเป็น String รายงาน ===
  print("\n=== รายงานนักศึกษา ===");
  var report = students
      .map((s) => "${s["name"]} (${s["faculty"]}) GPA: ${s["gpa"]}")
      .toList();
  report.forEach(print);

  // === sort() + reduce() ===
  print("\n=== วิเคราะห์คะแนน ===");
  List<double> gpas = students.map((s) => s["gpa"] as double).toList();

  double maxGpa = gpas.reduce((a, b) => a > b ? a : b);
  double minGpa = gpas.reduce((a, b) => a < b ? a : b);
  double avgGpa = gpas.reduce((a, b) => a + b) / gpas.length;

  print("GPA สูงสุด: $maxGpa");
  print("GPA ต่ำสุด: $minGpa");
  print("GPA เฉลี่ย: ${avgGpa.toStringAsFixed(2)}");

  // === any() และ every() ===
  bool anyFailing = students.any((s) => (s["gpa"] as double) < 2.0);
  bool allPassing = students.every((s) => (s["gpa"] as double) >= 2.0);
  print("มีนักศึกษาที่ GPA < 2.0: $anyFailing");
  print("ทุกคน GPA >= 2.0: $allPassing");

  // =========================================================================
  // เพิ่มเติมส่วนที่โจทย์ต้องการ
  // =========================================================================
  print("\n=== ค้นหาและจัดกลุ่มข้อมูล (ฟังก์ชันเพิ่มเติม) ===");
  
  // 1. ทดสอบใช้งาน findTopStudentByFaculty
  String targetFaculty = "วิทยาศาสตร์";
  String? topStudent = findTopStudentByFaculty(students, targetFaculty);
  print("นักศึกษาที่มี GPA สูงสุดในคณะ $targetFaculty คือ: ${topStudent ?? 'ไม่พบข้อมูล'}");

  // 2. ทดสอบใช้งาน groupByFaculty
  print("\n=== จัดกลุ่มนักศึกษาตามคณะ ===");
  Map<String, List<Map<String, dynamic>>> groupedData = groupByFaculty(students);
  groupedData.forEach((faculty, list) {
    print("คณะ $faculty:");
    for (var s in list) {
      print("  - ${s['name']} (GPA: ${s['gpa']})");
    }
  });

  // 3. ใช้ sort() เรียงนักศึกษาตาม GPA จากสูงไปต่ำ แล้วพิมพ์ 3 อันดับแรก
  print("\n=== นักศึกษาที่มี GPA สูงสุด 3 อันดับแรก ===");
  // ใช้การ .toList() ซ้อน เพื่อไม่ให้ไปเรียงสลับตำแหน่งใน List ตัวแปรต้นฉบับ (students)
  var sortedStudents = List<Map<String, dynamic>>.from(students);
  sortedStudents.sort((a, b) => (b["gpa"] as double).compareTo(a["gpa"] as double));

  // ดึงข้อมูล 3 อันดับแรกโดยใช้ .take(3)
  var topThree = sortedStudents.take(3);
  int rank = 1;
  for (var s in topThree) {
    print("อันดับ $rank: ${s["name"]} (คณะ ${s["faculty"]}) GPA: ${s["gpa"]}");
    rank++;
  }
}

// -------------------------------------------------------------------------
// ฟังก์ชันที่เพิ่มเข้ามาตามเงื่อนไขโจทย์
// -------------------------------------------------------------------------

/// คืนชื่อนักศึกษาที่ GPA สูงสุดในคณะที่ระบุ
String? findTopStudentByFaculty(List<Map<String, dynamic>> students, String faculty) {
  // กรองเอาเฉพาะนักศึกษาในคณะที่ต้องการ
  var facultyStudents = students.where((s) => s["faculty"] == faculty).toList();
  
  if (facultyStudents.isEmpty) return null;

  // ใช้ reduce เปรียบเทียบหาคนที่มี GPA สูงที่สุด
  var topStudent = facultyStudents.reduce((a, b) => 
    (a["gpa"] as double) > (b["gpa"] as double) ? a : b
  );
  
  return topStudent["name"];
}

/// จัดกลุ่มนักศึกษาตามคณะ คืนค่ากลับมาเป็น Map<String, List>
Map<String, List<Map<String, dynamic>>> groupByFaculty(List<Map<String, dynamic>> students) {
  Map<String, List<Map<String, dynamic>>> map = {};
  
  for (var student in students) {
    String faculty = student["faculty"];
    // ถ้ายังไม่มี Key ของคณะนี้ใน Map ให้สร้าง List ว่างรอก่อน
    if (!map.containsKey(faculty)) {
      map[faculty] = [];
    }
    // เพิ่มข้อมูลนักศึกษาลงในคณะนั้นๆ
    map[faculty]!.add(student);
  }
  
  return map;
}

```
<img width="1912" height="978" alt="image" src="https://github.com/user-attachments/assets/dce554ab-16b6-4447-8487-64875ced7167" />

---

## ส่วนที่ 3 — ทฤษฎีและการทดลอง: OOP

### ทฤษฎี 3.1 — Class และ Object

**Class** คือ "แบบพิมพ์" หรือ "Blueprint" ของ Object ส่วน **Object** คือ "สิ่งของ" ที่สร้างจาก Class นั้น

```
Class (แบบพิมพ์):          Object (สิ่งของ):
┌───────────────────┐      ┌───────────────────┐
│  class Student {  │  →   │  student1         │
│    String name;   │      │    name: "สมชาย"  │
│    int age;       │      │    age: 20        │
│    study() {...}  │      │    study() → ทำงาน│
│  }                │      └───────────────────┘
└───────────────────┘
                           ┌───────────────────┐
                       →   │  student2         │
                           │    name: "สมหญิง"  │
                           │    age: 21        │
                           └───────────────────┘
```

**โครงสร้าง Class ใน Dart:**

```dart
class BankAccount {
  // === Fields (ตัวแปรของ Object) ===
  final String accountNumber;   // final = เปลี่ยนหลัง Constructor ไม่ได้
  final String ownerName;
  double _balance;               // _ นำหน้า = private (ใช้ได้ใน Class นี้เท่านั้น)
  List<String> _transactions = [];

  // === Constructor (สร้าง Object) ===
  // this.xxx = กำหนดค่า field โดยตรง ไม่ต้องเขียน body
  BankAccount({
    required this.accountNumber,
    required this.ownerName,
    double initialBalance = 0,
  }) : _balance = initialBalance; // initializer list

  // Named Constructor — สร้าง Object แบบพิเศษ
  BankAccount.savings({required String owner})
      : accountNumber = "SAV${DateTime.now().millisecondsSinceEpoch}",
        ownerName = owner,
        _balance = 0;

  // === Getter — อ่านค่าแบบ Property ===
  double get balance => _balance;
  bool get isEmpty => _balance == 0;
  List<String> get transactions => List.unmodifiable(_transactions);

  // === Methods ===
  bool deposit(double amount) {
    if (amount <= 0) return false;
    _balance += amount;
    _transactions.add("ฝาก: +${amount.toStringAsFixed(2)}");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0 || amount > _balance) return false;
    _balance -= amount;
    _transactions.add("ถอน: -${amount.toStringAsFixed(2)}");
    return true;
  }

  // toString — แสดงเมื่อ print(object)
  @override
  String toString() =>
      "บัญชี[$accountNumber] ของ $ownerName ยอด: ${_balance.toStringAsFixed(2)}";
}
```

---

### ทฤษฎี 3.2 — Inheritance (การสืบทอด) และ Abstract Class

**Inheritance** คือการสร้าง Class ใหม่จาก Class เดิม โดยสืบทอดทุกอย่างมา แล้วเพิ่มหรือแก้ไขเฉพาะส่วนที่ต่าง

**Abstract Class** คือ Class ที่กำหนด "สัญญา" ว่า Subclass ต้อง implement อะไรบ้าง ไม่สามารถสร้าง Object โดยตรงได้

```
Abstract Class (สัญญา):
┌───────────────────────────────────────────┐
│  abstract class Shape {                   │
│    double get area;       ← ต้อง implement │
│    double get perimeter;  ← ต้อง implement │
│    void describe() {...}  ← มาให้แล้ว       │
│  }                                        │
└───────────────────────────────────────────┘
              ↑ extends
   ┌──────────┴──────────┐
   │                     │
Circle               Rectangle
┌──────────┐       ┌──────────────┐
│ radius   │       │ width,height │
│ area=πr² │       │ area=w*h     │
│ perim=2πr│       │ perim=2(w+h) │
└──────────┘       └──────────────┘
```

```dart
abstract class Shape {
  // Abstract getter — Subclass ต้อง implement
  double get area;
  double get perimeter;

  // Concrete method — มาให้แล้ว Subclass ใช้ได้เลย
  void describe() {
    print("${runtimeType}:");
    print("  พื้นที่: ${area.toStringAsFixed(2)} ตร.หน่วย");
    print("  เส้นรอบรูป: ${perimeter.toStringAsFixed(2)} หน่วย");
  }

  bool isLargerThan(Shape other) => area > other.area;
}

class Circle extends Shape {
  final double radius;
  Circle(this.radius);

  @override
  double get area => 3.14159 * radius * radius;

  @override
  double get perimeter => 2 * 3.14159 * radius;
}

class Rectangle extends Shape {
  final double width;
  final double height;
  Rectangle(this.width, this.height);

  @override
  double get area => width * height;

  @override
  double get perimeter => 2 * (width + height);

  // Subclass สามารถเพิ่ม method เองได้
  bool get isSquare => width == height;
}
```

---

### ทฤษฎี 3.3 — Mixin

**Mixin** คือชุด Method ที่เพิ่มให้กับ Class ได้ โดยไม่ต้อง Inherit (เหมาะเมื่อต้องการเพิ่ม Behavior หลายชุดให้กับ Class เดียว)

```
ปัญหา: Dart ให้ extends ได้แค่ 1 Class
แต่บางครั้งต้องการ Behavior จากหลายที่

Mixin แก้ปัญหาโดย:
class Duck extends Animal with Swimmable, Flyable {
  // Duck ได้ทั้ง swim() และ fly() โดยไม่ต้อง inherit สองชั้น
}
```

```dart
mixin Printable {
  // Mixin สามารถมี Method และ Getter
  void printInfo() {
    print(toString()); // เรียก toString() ของ Class ที่ใช้ Mixin
  }
}

mixin Saveable {
  Map<String, dynamic> toJson(); // บังคับให้ Class ที่ใช้ implement

  String toJsonString() {
    var json = toJson();
    return json.entries.map((e) => '"${e.key}": "${e.value}"').join(", ");
  }
}

// ใช้ Mixin หลายตัวพร้อมกัน
class Product with Printable, Saveable {
  final String name;
  final double price;
  final int stock;

  Product({required this.name, required this.price, required this.stock});

  @override
  Map<String, dynamic> toJson() => {
    "name": name,
    "price": price,
    "stock": stock,
  };

  @override
  String toString() => "$name (฿${price.toStringAsFixed(2)}) เหลือ $stock ชิ้น";
}
```

---

### การทดลอง 3.1 — Class และ Inheritance

**⏱ เวลา:** 30 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ด BankAccount:

```dart
class BankAccount {
  final String ownerName;
  double _balance;
  List<String> _history = [];

  BankAccount({required this.ownerName, double initial = 0})
      : _balance = initial;

  double get balance => _balance;
  List<String> get history => List.unmodifiable(_history);

  bool deposit(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    _balance += amount;
    _history.add("+ ฝาก ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ฝาก ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    if (amount > _balance) {
      print("❌ ยอดเงินไม่เพียงพอ (มี ${_balance.toStringAsFixed(2)} บาท)");
      return false;
    }
    _balance -= amount;
    _history.add("- ถอน ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ถอน ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  void printStatement() {
    print("\n=== สรุปบัญชี: $ownerName ===");
    print("ยอดปัจจุบัน: ${_balance.toStringAsFixed(2)} บาท");
    print("ประวัติรายการ:");
    if (_history.isEmpty) {
      print("  (ยังไม่มีรายการ)");
    } else {
      _history.forEach((h) => print("  $h"));
    }
  }

  @override
  String toString() => "BankAccount(${ownerName}, ยอด: ${_balance.toStringAsFixed(2)})";
}
```

**ขั้นตอนที่ 2** เพิ่ม Subclass SavingsAccount ที่มีดอกเบี้ย:

```dart
class SavingsAccount extends BankAccount {
  final double interestRate; // อัตราดอกเบี้ยต่อปี เช่น 0.03 = 3%

  SavingsAccount({
    required String ownerName,
    required this.interestRate,
    double initial = 0,
  }) : super(ownerName: ownerName, initial: initial);

  // Override withdraw เพื่อเพิ่มกฎพิเศษ
  @override
  bool withdraw(double amount) {
    if (_balance - amount < 500) {
      print("❌ บัญชีออมทรัพย์ต้องมียอดขั้นต่ำ 500 บาท");
      return false;
    }
    return super.withdraw(amount); // เรียก withdraw() ของ BankAccount
  }

  // Method พิเศษของ SavingsAccount
  void applyMonthlyInterest() {
    double interest = _balance * interestRate / 12;
    _balance += interest;
    _history.add("+ ดอกเบี้ยรายเดือน ${interest.toStringAsFixed(2)} บาท");
    print("✅ ดอกเบี้ยเดือนนี้: ${interest.toStringAsFixed(2)} บาท");
  }
}
```

**ขั้นตอนที่ 3** เพิ่ม `main()` และรัน:

```dart
void main() {
  print("=== ทดสอบ BankAccount ===\n");
  var acc = BankAccount(ownerName: "สมชาย", initial: 1000);

  acc.deposit(500);
  acc.withdraw(200);
  acc.withdraw(2000); // เกินยอด
  acc.withdraw(-100); // ค่าไม่ถูก
  acc.printStatement();

  print("\n=== ทดสอบ SavingsAccount ===\n");
  var savings = SavingsAccount(
    ownerName: "สมหญิง",
    interestRate: 0.03,
    initial: 1000,
  );

  savings.deposit(5000);
  savings.withdraw(5600); // เหลือน้อยกว่า 500
  savings.withdraw(3000); // ได้
  savings.applyMonthlyInterest();
  savings.printStatement();

  // Polymorphism — ใช้ BankAccount แทนทั้งคู่ได้
  print("\n=== Polymorphism ===");
  List<BankAccount> accounts = [acc, savings];
  for (var account in accounts) {
    print(account); // เรียก toString() ของแต่ละ Object
  }
}
```

**ขั้นตอนที่ 4** กด Run และอ่านผลลัพธ์ทุกบรรทัด

---

### 🎯 โจทย์ฝึกทำ 3 — เขียน Class ด้วยตนเอง

1. สร้าง `CheckingAccount extends BankAccount` ที่อนุญาตให้ถอนเกินยอดได้ไม่เกิน 500 บาท (Overdraft) และคิดค่าธรรมเนียม 50 บาท เมื่อ Overdraft

2. สร้าง Abstract Class `Vehicle` ที่มี Abstract getter `fuelEfficiency` (กม./ลิตร), method `refuel(double liters)`, method `drive(double km)` ที่คำนวณการใช้น้ำมัน จากนั้นสร้าง `Car` และ `Truck` ที่ extend `Vehicle` โดยมี `fuelEfficiency` ต่างกัน

3. สร้าง Mixin `Discountable` ที่มี method `applyDiscount(double percent)` แล้วนำไปใช้กับ Class `Product` ที่มี `name` และ `price`


**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
// =========================================================================
// ส่วนที่ 1: การจัดการบัญชีธนาคาร (BankAccount, SavingsAccount, CheckingAccount)
// =========================================================================

class BankAccount {
  final String ownerName;
  double _balance;
  List<String> _history = [];

  BankAccount({required this.ownerName, double initial = 0})
      : _balance = initial;

  double get balance => _balance;
  List<String> get history => List.unmodifiable(_history);

  bool deposit(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    _balance += amount;
    _history.add("+ ฝาก ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ฝาก ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    if (amount > _balance) {
      print("❌ ยอดเงินไม่เพียงพอ (มี ${_balance.toStringAsFixed(2)} บาท)");
      return false;
    }
    _balance -= amount;
    _history.add("- ถอน ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ถอน ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  void printStatement() {
    print("\n=== สรุปบัญชี: $ownerName ===");
    print("ยอดปัจจุบัน: ${_balance.toStringAsFixed(2)} บาท");
    print("ประวัติรายการ:");
    if (_history.isEmpty) {
      print("  (ยังไม่มีรายการ)");
    } else {
      _history.forEach((h) => print("  $h"));
    }
  }

  @override
  String toString() => "BankAccount(${ownerName}, ยอด: ${_balance.toStringAsFixed(2)})";
}

class SavingsAccount extends BankAccount {
  final double interestRate; // อัตราดอกเบี้ยต่อปี เช่น 0.03 = 3%

  SavingsAccount({
    required String ownerName,
    required this.interestRate,
    double initial = 0,
  }) : super(ownerName: ownerName, initial: initial);

  @override
  bool withdraw(double amount) {
    if (_balance - amount < 500) {
      print("❌ บัญชีออมทรัพย์ต้องมียอดขั้นต่ำ 500 บาท");
      return false;
    }
    return super.withdraw(amount);
  }

  void applyMonthlyInterest() {
    double interest = _balance * interestRate / 12;
    _balance += interest;
    _history.add("+ ดอกเบี้ยรายเดือน ${interest.toStringAsFixed(2)} บาท");
    print("✅ ดอกเบี้ยเดือนนี้: ${interest.toStringAsFixed(2)} บาท");
  }
}

/// 1. สร้าง CheckingAccount extends BankAccount
class CheckingAccount extends BankAccount {
  CheckingAccount({required String ownerName, double initial = 0})
      : super(ownerName: ownerName, initial: initial);

  @override
  bool withdraw(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }

    // กรณีที่ 1: มียอดเงินในบัญชีเพียงพอสำหรับถอนปกติ (ไม่เกิด Overdraft)
    if (amount <= _balance) {
      return super.withdraw(amount);
    } 
    
    // กรณีที่ 2: ถอนเกินบัญชี (Overdraft) แต่มีค่าธรรมเนียม 50 บาทเพิ่มเข้ามา
    double totalDeduction = amount + 50.0;
    if (_balance - totalDeduction < -500.0) {
      print("❌ ปฏิเสธรายการ: ยอดถอนเกินวงเงิน Overdraft สูงสุด 500 บาท (ยอดเงินปัจจุบัน: ${_balance.toStringAsFixed(2)} บาท, ยอดถอนที่ขอ: ${amount.toStringAsFixed(2)} บาท + ค่าธรรมเนียม 50.00 บาท)");
      return false;
    }

    // ทำการหักยอดเงินจริง (ยอดจะติดลบ)
    _balance -= totalDeduction;
    _history.add("- ถอน (Overdraft) ${amount.toStringAsFixed(2)} บาท [บวกค่าธรรมเนียม 50.00 บาท] (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ถอนเงินเกินบัญชีสำเร็จ (คิดค่าธรรมเนียม Overdraft 50.00 บาท) ยอดคงเหลือปัจจุบันติดลบ: ${_balance.toStringAsFixed(2)} บาท");
    return true;
  }
}

// =========================================================================
// ส่วนที่ 2: ยานพาหนะด้วย Abstract Class (Vehicle, Car, Truck)
// =========================================================================

/// 2. สร้าง Abstract Class Vehicle
abstract class Vehicle {
  double fuel = 0.0; // ปริมาณน้ำมันคงเหลือในถัง (ลิตร)

  // Abstract getter สำหรับอัตราการประหยัดน้ำมัน (กิโลเมตรต่อลิตร)
  double get fuelEfficiency; 

  void refuel(double liters) {
    if (liters <= 0) {
      print("❌ จำนวนน้ำมันที่เติมต้องมากกว่า 0");
      return;
    }
    fuel += liters;
    print("⛽ เติมน้ำมัน +${liters.toStringAsFixed(2)} ลิตร (น้ำมันคงเหลือในถัง: ${fuel.toStringAsFixed(2)} ลิตร)");
  }

  void drive(double km) {
    double fuelNeeded = km / fuelEfficiency;
    if (fuel >= fuelNeeded) {
      fuel -= fuelNeeded;
      print("🚗 เดินทางได้ $km กม. (ใช้น้ำมันไป ${fuelNeeded.toStringAsFixed(2)} ลิตร, น้ำมันเหลือ ${fuel.toStringAsFixed(2)} ลิตร)");
    } else {
      print("❌ น้ำมันไม่พอสำหรับการเดินทาง $km กม. (ต้องใช้ ${fuelNeeded.toStringAsFixed(2)} ลิตร แต่เหลือในถังเพียง ${fuel.toStringAsFixed(2)} ลิตร)");
    }
  }
}

// สร้างคลาส Car (สมมติให้อัตราประหยัดน้ำมันคือ 15 กม./ลิตร)
class Car extends Vehicle {
  @override
  double get fuelEfficiency => 15.0; 
}

// สร้างคลาส Truck (สมมติให้อัตราประหยัดน้ำมันคือ 8 กม./ลิตร)
class Truck extends Vehicle {
  @override
  double get fuelEfficiency => 8.0; 
}

// =========================================================================
// ส่วนที่ 3: ส่วนลดสินค้าด้วย Mixin (Discountable, Product)
// =========================================================================

/// 3. สร้าง Mixin Discountable
mixin Discountable {
  double price = 0.0;

  void applyDiscount(double percent) {
    if (percent < 0 || percent > 100) {
      print("❌ เปอร์เซ็นต์ส่วนลดต้องอยู่ระหว่าง 0 - 100");
      return;
    }
    double discountAmount = price * (percent / 100);
    price -= discountAmount;
    print("🏷️ ได้รับส่วนลด $percent% (ลดลง ${discountAmount.toStringAsFixed(2)} บาท, ราคาหลังหักส่วนลด: ${price.toStringAsFixed(2)} บาท)");
  }
}

// สร้างคลาส Product ที่ใช้ Mixin Discountable
class Product with Discountable {
  final String name;

  Product({required this.name, required double initialPrice}) {
    price = initialPrice;
  }
}

// =========================================================================
// ส่วนหลักในการรันโปรแกรมเพื่อทดสอบระบบ (main)
// =========================================================================

void main() {
  print("=========================================");
  print("🛠️ ทดสอบที่ 1: CheckingAccount (Overdraft)");
  print("=========================================");
  var checkAcc = CheckingAccount(ownerName: "สมรัก", initial: 1000);

  checkAcc.deposit(500); // ยอดปัจจุบัน 1500
  checkAcc.withdraw(1200); // ถอนปกติ เหลือ 300
  checkAcc.withdraw(500); // ยอดไม่พอถอนปกติ แต่เข้าเงื่อนไข Overdraft (300 - 500 - 50 = -250 บาท) -> ผ่าน
  checkAcc.withdraw(300); // ลองถอนเพิ่มอีก (-250 - 300 - 50 = -600 บาท) -> เกิน -500 บาท -> ล้มเหลว
  checkAcc.printStatement();

  print("\n=========================================");
  print("🚗 ทดสอบที่ 2: Abstract Class & Vehicle (Car/Truck)");
  print("=========================================");
  print("[ทดสอบรถยนต์ (Car) - อัตราสิ้นเปลือง 15 กม./ลิตร]");
  var myCar = Car();
  myCar.refuel(20);
  myCar.drive(150); // วิ่ง 150 กม. ใช้น้ำมัน 10 ลิตร -> ผ่าน
  myCar.drive(200); // วิ่งอีก 200 กม. ใช้น้ำมัน 13.33 ลิตร แต่น้ำมันเหลือ 10 ลิตร -> ไม่ผ่าน

  print("\n[ทดสอบรถบรรทุก (Truck) - อัตราสิ้นเปลือง 8 กม./ลิตร]");
  var myTruck = Truck();
  myTruck.refuel(30);
  myTruck.drive(160); // วิ่ง 160 กม. ใช้น้ำมัน 20 ลิตร -> ผ่าน

  print("\n=========================================");
  print("🏷️ ทดสอบที่ 3: Mixin Discountable & Product");
  print("=========================================");
  var lapTop = Product(name: "Gaming Laptop", initialPrice: 35000);
  print("สินค้า: ${lapTop.name} | ราคาตั้งต้น: ${lapTop.price.toStringAsFixed(2)} บาท");
  lapTop.applyDiscount(10); // ลด 10%
  lapTop.applyDiscount(5);  // ลดเพิ่มอีก 5% จากราคาใหม่
}


```
<img width="1912" height="1022" alt="image" src="https://github.com/user-attachments/assets/2d56e582-0b93-4c85-8321-950d30913873" />

---

## ส่วนที่ 4 — ทฤษฎีและการทดลอง: Async/Await และ Future

### ทฤษฎี 4.1 — ทำไม Mobile App ถึงต้องมี Async?

ลองนึกถึงแอปสั่งอาหาร เมื่อผู้ใช้กด "สั่งอาหาร" แอปต้องส่งข้อมูลไปยัง Server แล้วรอการยืนยัน ซึ่งอาจใช้เวลา 1-3 วินาที

```
ถ้าใช้ Synchronous (รอแบบบล็อก):
────────────────────────────────────────────────────
Main Thread: [วาด UI][รอ Server 3 วินาที.........][วาด UI]
                          ↑
             ผู้ใช้กดอะไรก็ไม่ตอบสนอง
             ระบบแจ้ง "App Not Responding"

ถ้าใช้ Asynchronous (รอแบบไม่บล็อก):
────────────────────────────────────────────────────
Main Thread: [วาด][แสดง Loading][วาด][วาด][แสดงผล]
Background:  [ส่งไป Server──────────►รับผล]
```

**Future คืออะไร:**

```
Future<T> = "คำสัญญาว่าจะได้ T ในอนาคต"

Future<String>  → จะได้ String ในภายหลัง
Future<int>     → จะได้ int ในภายหลัง
Future<void>    → จะเสร็จในภายหลัง (ไม่มีค่าคืน)

สถานะของ Future:
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│  Pending    │ →  │  Completed   │ or │   Error     │
│  (รอผล)     │    │  (มีผลแล้ว)    │    │  (เกิดข้อ     │
│             │    │              │    │   ผิดพลาด)   │
└─────────────┘    └──────────────┘    └─────────────┘
```

---

### ทฤษฎี 4.2 — async/await และ Error Handling

```dart
// ประกาศ async function
Future<String> fetchUserName(int userId) async {
  // await หยุดรอ Future โดยไม่บล็อก Thread หลัก
  await Future.delayed(Duration(seconds: 1)); // จำลองการรอ Network

  if (userId <= 0) {
    // throw ส่ง Error ออกไป
    throw ArgumentError("userId ต้องมากกว่า 0");
  }
  return "ผู้ใช้ #$userId";
}

// เรียกใช้ด้วย await
void main() async {
  // try/catch จัดการ Error
  try {
    String name = await fetchUserName(1);
    print("ได้รับ: $name");

    // จะ throw ArgumentError
    await fetchUserName(-1);

  } on ArgumentError catch (e) {
    // จับ Error เฉพาะประเภท
    print("Argument Error: $e");
  } catch (e, stackTrace) {
    // จับ Error ทุกประเภท
    print("Error: $e");
    print("Stack: $stackTrace");
  } finally {
    // รันเสมอ ไม่ว่าจะ Error หรือไม่
    print("เสร็จสิ้น");
  }
}
```

**Sequential vs Parallel Execution:**

```dart
// Sequential — รอทีละอย่าง (เสียเวลา)
Future<void> loadSequential() async {
  var user    = await fetchUser(1);      // รอ 1 วินาที
  var posts   = await fetchPosts(1);     // รออีก 0.8 วินาที
  var friends = await fetchFriends(1);   // รออีก 0.5 วินาที
  // รวม ~2.3 วินาที
}

// Parallel — รอพร้อมกัน (เร็วกว่า)
Future<void> loadParallel() async {
  var results = await Future.wait([
    fetchUser(1),       // ╗
    fetchPosts(1),      // ╠═ รันพร้อมกันทั้งหมด
    fetchFriends(1),    // ╝
  ]);
  // รวม ~1 วินาที (เท่ากับ task ที่นานสุด)
  var user    = results[0];
  var posts   = results[1];
  var friends = results[2];
}
```

**Stream — ข้อมูลที่ไหลต่อเนื่อง:**

```dart
// Stream คือ "ท่อ" ที่ส่งข้อมูลหลายๆ ครั้งตามเวลา
Stream<int> countDown(int from) async* {
  for (int i = from; i >= 0; i--) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // ส่งค่าออกทาง Stream
  }
}

// รับข้อมูลจาก Stream
void main() async {
  await for (int count in countDown(5)) {
    print("นับถอยหลัง: $count");
  }
  print("🚀 ปล่อย!");
}
```

---

### การทดลอง 4.1 — Future และ async/await

**⏱ เวลา:** 25 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ดจำลอง API:

```dart
import 'dart:async';

// จำลอง Database/API functions
Future<Map<String, dynamic>> fetchUser(int id) async {
  print("  [API] กำลังดึงข้อมูล User $id...");
  await Future.delayed(Duration(milliseconds: 800)); // จำลอง Network delay

  if (id <= 0) throw Exception("User ID ไม่ถูกต้อง");

  return {
    "id": id,
    "name": "ผู้ใช้ที่ $id",
    "email": "user$id@example.com",
    "role": id == 1 ? "admin" : "user",
  };
}

Future<List<String>> fetchUserPosts(int userId) async {
  print("  [API] กำลังดึง Posts ของ User $userId...");
  await Future.delayed(Duration(milliseconds: 600));

  return [
    "โพสต์ที่ 1 ของ User $userId",
    "โพสต์ที่ 2 ของ User $userId",
    "โพสต์ที่ 3 ของ User $userId",
  ];
}

Future<int> fetchUserFollowers(int userId) async {
  print("  [API] กำลังดึงจำนวน Follower ของ User $userId...");
  await Future.delayed(Duration(milliseconds: 500));
  return userId * 42; // จำลอง
}
```

**ขั้นตอนที่ 2** เพิ่ม `main()` ทดสอบแบบ Sequential:

```dart
void main() async {
  print("=== Sequential (รอทีละอย่าง) ===");
  var stopwatch = Stopwatch()..start();

  var user      = await fetchUser(1);
  var posts     = await fetchUserPosts(1);
  var followers = await fetchUserFollowers(1);

  stopwatch.stop();
  print("ชื่อ: ${user['name']}");
  print("จำนวนโพสต์: ${posts.length}");
  print("Followers: $followers คน");
  print("เวลาที่ใช้: ${stopwatch.elapsedMilliseconds}ms\n");

  // === Parallel ===
  print("=== Parallel (Future.wait) ===");
  stopwatch = Stopwatch()..start();

  var results = await Future.wait([
    fetchUser(2),
    fetchUserPosts(2),
    fetchUserFollowers(2),
  ]);

  stopwatch.stop();
  var user2      = results[0] as Map<String, dynamic>;
  var posts2     = results[1] as List<String>;
  var followers2 = results[2] as int;

  print("ชื่อ: ${user2['name']}");
  print("จำนวนโพสต์: ${posts2.length}");
  print("Followers: $followers2 คน");
  print("เวลาที่ใช้: ${stopwatch.elapsedMilliseconds}ms");
}
```

**ขั้นตอนที่ 3** กด Run สังเกตความแตกต่างของเวลา

**ขั้นตอนที่ 4** เพิ่ม Error Handling ต่อท้าย `main()`:

```dart
  // === Error Handling ===
  print("\n=== Error Handling ===");

  try {
    print("ลองดึง User ID = -1:");
    var badUser = await fetchUser(-1);
    print("ได้รับ: $badUser"); // ไม่ถึงบรรทัดนี้
  } on Exception catch (e) {
    print("❌ Exception: $e");
  }

  print("โปรแกรมยังทำงานต่อได้หลัง Error ✅");
```

**ขั้นตอนที่ 5** กด Run อีกครั้ง บันทึกผลเวลาของ Sequential vs Parallel

<img width="1898" height="967" alt="image" src="https://github.com/user-attachments/assets/c5c28738-931c-4f0c-ab4f-8c9e6762b595" />


---

### การทดลอง 4.2 — Stream

**⏱ เวลา:** 15 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์:

```dart
import 'dart:async';

// Stream Generator ด้วย async*
Stream<double> simulateStockPrice(String symbol) async* {
  double price = 100.0;
  int ticks = 0;

  while (ticks < 5) {
    await Future.delayed(Duration(milliseconds: 500));

    // จำลองการเปลี่ยนราคา
    double change = (ticks % 2 == 0) ? 2.5 : -1.5;
    price += change;
    ticks++;

    yield price; // ส่งค่าออกทาง Stream
  }
}

void main() async {
  print("=== ราคาหุ้น (Stream) ===");
  print("Symbol: DART\n");

  double? lastPrice;

  await for (double price in simulateStockPrice("DART")) {
    String direction = "";
    if (lastPrice != null) {
      direction = price > lastPrice! ? "📈 ขึ้น" : "📉 ลง";
    }
    print("ราคา: ${price.toStringAsFixed(2)} บาท  $direction");
    lastPrice = price;
  }

  print("\nสิ้นสุดการแสดงราคา");
}
```

**ขั้นตอนที่ 2** กด Run สังเกตว่าราคาออกมาทีละค่า ไม่ใช่ทั้งหมดพร้อมกัน

---

### 🎯 โจทย์ฝึกทำ 4 — เขียน Async ด้วยตนเอง

1. สร้าง `Future<double> calculateTax(double income)` ที่มี delay 0.5 วินาที คืนค่าภาษีตามอัตราก้าวหน้า (income <= 150,000 → 0%, <= 300,000 → 5%, <= 500,000 → 10%, อื่นๆ → 20%)

2. เขียน `main()` ที่ดึงข้อมูลรายได้ของผู้ใช้ 3 คนพร้อมกัน (ใช้ `Future.wait`) แล้วคำนวณภาษีแต่ละคน และแสดงผลรวมภาษีทั้งหมด

3. สร้าง `Stream<String>` ที่จำลองการส่ง Chat Message ทุก 1 วินาที เป็นเวลา 5 ครั้ง แล้วแสดงผลผ่าน `await for`

**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
import 'dart:async';

// =========================================================================
// ส่วนที่ 1: การคำนวณภาษีด้วย Future
// =========================================================================

/// คืนค่าภาษีตามอัตราก้าวหน้า โดยหน่วงเวลา 0.5 วินาที (500 มิลลิวินาที)
Future<double> calculateTax(double income) async {
  await Future.delayed(Duration(milliseconds: 500));

  if (income <= 150000) {
    return 0.0;
  } else if (income <= 300000) {
    return (income - 150000) * 0.05;
  } else if (income <= 500000) {
    // 150,000 แรก ยกเว้นภาษี
    // 150,001 - 300,000 (คิด 5% เป็นเงินสูงสุด 7,500 บาท)
    // ส่วนที่เกิน 300,000 คิด 10%
    return 7500.0 + (income - 300000) * 0.10;
  } else {
    // 150,000 แรก ยกเว้นภาษี
    // 150,001 - 300,000 (คิด 5% เป็นเงินสูงสุด 7,500 บาท)
    // 300,001 - 500,000 (คิด 10% เป็นเงินสูงสุด 20,000 บาท)
    // ส่วนที่เกิน 500,000 คิด 20%
    return 7500.0 + 20000.0 + (income - 500000) * 0.20;
  }
}

// =========================================================================
// ส่วนที่ 2: จำลอง Chat Message ด้วย Stream
// =========================================================================

/// จำลองการส่ง Chat Message ทุก 1 วินาที จำนวน 5 ครั้ง
Stream<String> simulateChatMessage() async* {
  List<String> messages = [
    "สวัสดีครับ ยินดีต้อนรับเข้าสู่แชทบอท 🤖",
    "วันนี้มีบริการอะไรให้ผมช่วยเหลือไหมครับ? 💬",
    "กำลังประมวลผลคำขอของคุณ กรุณารอสักครู่... ⏳",
    "ค้นหาข้อมูลเสร็จเรียบร้อยแล้วครับ! 🎉",
    "ขอบคุณที่ใช้บริการ มีคำถามเพิ่มเติมถามได้เลยนะ ครับ 👋",
  ];

  for (var message in messages) {
    await Future.delayed(Duration(seconds: 1));
    yield message; // ส่งข้อความออกไปทาง Stream
  }
}

// =========================================================================
// ฟังก์ชันหลัก (main)
// =========================================================================

void main() async {
  print("=========================================");
  print("💵 1. ระบบคำนวณภาษีนักศึกษา/วัยทำงาน (Future.wait)");
  print("=========================================");

  // ข้อมูลรายได้จำลองของผู้ใช้ 3 คน
  List<Map<String, dynamic>> users = [
    {"name": "นายสมบูรณ์", "income": 280000.0},
    {"name": "นางสาวสมใจ", "income": 450000.0},
    {"name": "นายสมศักดิ์", "income": 650000.0},
  ];

  print("กำลังคำนวณภาษีสำหรับทุกคนพร้อมกัน...");
  Stopwatch stopwatch = Stopwatch()..start();

  // ดึงข้อมูลรายได้และส่งคำนวณพร้อมกันโดยใช้ Future.wait
  List<double> taxes = await Future.wait(
    users.map((user) => calculateTax(user["income"] as double))
  );

  stopwatch.stop();

  // แสดงผลรายบุคคล
  double totalTax = 0.0;
  for (int i = 0; i < users.length; i++) {
    String name = users[i]["name"];
    double income = users[i]["income"];
    double tax = taxes[i];
    totalTax += tax;

    print("👤 $name | รายได้ต่อปี: ${income.toStringAsFixed(2)} บาท | ภาษีที่ต้องจ่าย: ${tax.toStringAsFixed(2)} บาท");
  }

  print("-----------------------------------------");
  print("📊 สรุปยอดภาษีรวมทั้งหมด: ${totalTax.toStringAsFixed(2)} บาท");
  print("⏱️ ใช้เวลาในการประมวลผลทั้งสิ้น: ${stopwatch.elapsedMilliseconds} มิลลิวินาที (เนื่องจากทำงานพร้อมกัน)");

  print("\n=========================================");
  print("💬 2. ระบบส่งข้อความแชทจำลอง (Stream & await for)");
  print("=========================================");

  await for (String message in simulateChatMessage()) {
    print("[Chat Bot]: $message");
  }

  print("\n✨ สิ้นสุดการทำงานของระบบทั้งหมด ✨");
}


```
<img width="1907" height="970" alt="image" src="https://github.com/user-attachments/assets/c458d012-efcc-43d7-bfe0-3793e0bcb04b" />

---


### คำถามท้ายใบงาน

**ข้อ 1** อธิบายความแตกต่างระหว่าง `final` และ `const` พร้อมยกตัวอย่างกรณีที่ใช้แต่ละแบบ
```text
 const: ต้องรู้ค่าตั้งแต่ตอนเขียนโค้ดและห้ามเปลี่ยนอีกเลย เช่น const pi = 3.14 หรือค่าสีหลักของแอป
 final: รู้ค่าตอนรันโปรแกรมแล้วค่อยล็อกไม่ให้เปลี่ยน เช่น final time = DateTime.now() หรือข้อมูลที่ดึงมาจากเซิร์ฟเวอร์

```
**ข้อ 2** Named Parameters และ Positional Parameters ต่างกันอย่างไร? ควรเลือกใช้แบบไหนเมื่อไหร่?
```text
Positional (ตามตำแหน่ง): ส่งค่าตามลำดับ เช่น login('user', 'pass') เหมาะกับฟังก์ชันที่มีตัวแปรน้อยๆ (1-2 ตัว) เขียนง่ายดี
Named (ระบุชื่อ): ส่งค่าโดยใส่ชื่อกำกับสลับลำดับได้ เช่น login(username: 'user', password: 'pass') เหมาะกับฟังก์ชันที่มีตัวแปรเยอะๆ อ่านง่ายและปลอดภัยกว่า

```
**ข้อ 3** Abstract Class และ Mixin มีจุดประสงค์ต่างกันอย่างไร? ยกตัวอย่างสถานการณ์ที่เหมาะกับแต่ละแบบ
```text
Abstract Class: เป็นโครงร่างหลักเพื่อให้คลาสลูกสืบทอดไปใช้งาน (Is-A) เช่น สร้าง Vehicle เป็นคลาสแม่ให้ Car และ Truck
Mixin: เป็นความสามารถเสริมที่เอาไปแปะให้คลาสไหนใช้ก็ได้ (Can-Do) เช่น สร้างความสามารถ Flyable (บินได้) ไปแปะให้ทั้ง Bird และ Airplane ทั้งที่สองคลาสนี้ไม่ได้เป็นญาติกัน

```
**ข้อ 4** จากการทดลอง 4.1 Sequential ใช้เวลาประมาณกี่ ms และ Parallel ใช้เวลาเท่าไหร่? อธิบายเหตุผลที่ Parallel เร็วกว่า และบอกกรณีที่ต้องใช้ Sequential แทน
```text
Sequential: ใช้เวลาประมาณ 1,500 ms เพราะต้องรอทำงานทีละขั้นตอนจนครบ
Parallel: ใช้เวลาประมาณ 500 ms เพราะใช้ Future.wait สั่งรันทุกงานพร้อมกันทันที
กรณีที่ต้องใช้ Sequential: งานที่ต้องใช้ผลลัพธ์จากงานก่อนหน้า เช่น ต้องดึง Token ล็อกอินให้เสร็จก่อน ถึงจะดึงข้อมูลโปรไฟล์ผู้ใช้ได้

```
**ข้อ 5** Future และ Stream ต่างกันอย่างไร? ยกตัวอย่างสถานการณ์ที่เหมาะกับแต่ละแบบจากการพัฒนา Mobile App จริงๆ
```text
Future: ดึงข้อมูลครั้งเดียวได้คำตอบแล้วจบงานเลย เช่น กดปุ่มบันทึกข้อมูล หรือดึงโปรไฟล์ผู้ใช้มาแสดงผล
Stream: ข้อมูลไหลมาเรื่อยๆ ตลอดเวลาตราบที่ยังไม่ปิดท่อ เช่น ข้อความในห้องแชทเรียลไทม์ หรือพิกัด GPS บนแผนที่ขณะเดินทาง

```
---

## ข้อผิดพลาดที่พบบ่อย

| Error Message | สาเหตุ | วิธีแก้ |
|---|---|---|
| `A value of type 'Null' can't be assigned...` | กำหนด null ให้ตัวแปร non-nullable | เพิ่ม `?` หรือกำหนดค่าเริ่มต้น |
| `The getter '...' isn't defined` | เรียก method ที่ไม่มีใน Type นั้น | ตรวจสอบ Type ของตัวแปร |
| `Non-abstract class '...' missing concrete implementation` | ไม่ได้ implement abstract method | เพิ่ม `@override` และ implement method |
| `Uncaught Error: ...` | Future throw Error แต่ไม่มี try/catch | ห่อด้วย try/catch |
| `setState() or markNeedsBuild()...` (บน Flutter) | เรียก setState หลัง dispose | เช็ค `if (mounted)` ก่อน setState |

---

*ใบงานการทดลองที่ 2-1 | Dart Programming*
*วิชา: การพัฒนาซอฟต์แวร์สำหรับอุปกรณ์เคลื่อนที่*
