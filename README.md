  
# بیا به دنیای SOLID وارد بشیم! 🎩✨

قوانینی که هر برنامه‌نویسی رو از یه کدنویس معمولی به یه جادوگر کد تمیز تبدیل می‌کنه. SOLID پنج اصل طلایی کدنویسی شی‌گراست که باعث میشه کدهات تمیزتر، قابل نگهداری‌تر و پرقدرت‌تر بشن. 
آماده‌ای؟ بزن بریم!

## 1. اصل تک‌مسئولیتی 🦸‍♂️

هر کلاس باید فقط یک وظیفه داشته باشه.
یعنی نباید همه‌چیز رو در یک کلاس بگنجونی. 
هر کلاس مثل یه قهرمانه که یه ماموریت خاص داره.

**مثال بد:**

یک کلاس که هم داده از سرور می‌گیره و هم اطلاعات کاربر رو مدیریت می‌کنه.

```dart
class UserManager {
  void fetchUserData() {
    // گرفتن داده از سرور
  }
  
  void saveUserData() {
    // ذخیره اطلاعات کاربر
  }
}
```

حالا وقتشه این کلاس رو تمیز کنیم. بیایم مسئولیت‌ها رو از هم جدا کنیم:

```dart
class UserDataFetcher {
  void fetchUserData() {
    // گرفتن داده از سرور
  }
}

class UserDataSaver {
  void saveUserData() {
    // ذخیره اطلاعات کاربر
  }
}
```

حالا هر کلاس فقط یک کار انجام میده و اگر بخوایم کدی رو تغییر بدیم، بدون خراب کردن بقیه، راحت‌تر می‌تونیم دستکاری کنیم. 🔧

## 2. اصل باز/بسته 💼

کدهات باید برای توسعه باز و برای تغییر بسته باشن. 
یعنی چی؟ یعنی به جای اینکه کدهای قدیمی رو برای اضافه کردن قابلیت‌های جدید تغییر بدی، قابلیت‌های جدید رو از طریق ارث‌بری یا پیاده‌سازی اضافه کن. کد قدیمی نباید دست‌کاری بشه.

**مثال بد:**

```dart
class NotificationService {
  void sendNotification(String message, String type) {
    if (type == 'email') {
      // ارسال ایمیل
    } else if (type == 'sms') {
      // ارسال پیامک
    }
  }
}
```

هر بار که می‌خوای روش جدیدی برای ارسال نوتیفیکیشن اضافه کنی (مثلاً نوتیفیکیشن پوش)، باید کدت رو تغییر بدی. 😩 بیا درستش کنیم:

```dart
abstract class NotificationSender {
  void send(String message);
}

class EmailNotificationSender implements NotificationSender {
  @override
  void send(String message) {
    // ارسال ایمیل
  }
}

class SMSNotificationSender implements NotificationSender {
  @override
  void send(String message) {
    // ارسال پیامک
  }
}
```

حالا اگر بخوایم نوع جدیدی از نوتیفیکیشن‌ها (مثلاً پوش نوتیفیکیشن) اضافه کنیم، فقط یه کلاس جدید می‌سازیم بدون اینکه کدهای قبلی رو تغییر بدیم. 🎉

## 3. اصل جایگزینی لیسکوف 🧠

این اصل میگه که اگر یه کلاس فرزند از کلاس والد ارث‌بری کرده، باید بتونه جای والدش رو بگیره، بدون اینکه چیزی تو برنامه خراب بشه.

**مثال بد:**

```dart
class Bird {
  void fly() {
    print("پرواز می‌کنه");
  }
}

class Penguin extends Bird {
  @override
  void fly() {
    throw Exception("پنگوئن‌ها نمی‌تونن پرواز کنن");
  }
}
```

اگر کد بالا رو جایی استفاده کنیم که فقط انتظار پرنده‌هایی داریم که پرواز می‌کنن، برنامه به مشکل می‌خوره. 
بهتره از ارث‌بری اشتباه خودداری کنیم:

```dart
abstract class Bird {}

class FlyingBird extends Bird {
  void fly() {
    print("پرواز می‌کنه");
  }
}

class Penguin extends Bird {
  // پنگوئن نیازی به تابع پرواز نداره
}
```

حالا اگر پنگوئن اضافه بشه، سیستم از هم نمی‌پاشه و تو بی‌دغدغه کدت رو ادامه میدی. 🐧💨

## 4. اصل جداسازی رابط‌ها 📐

این اصل میگه که نباید یه کلاس رو مجبور کنی تا متدهایی رو پیاده‌سازی کنه که اصلاً بهش نیازی نداره.
بیا از ساختارهای کوچک‌تر و اختصاصی‌تر استفاده کنیم.

**مثال بد:**

```dart
abstract class Worker {
  void work();
  void eat();
}

class Robot implements Worker {
  @override
  void work() {
    print("کار می‌کنه");
  }

  @override
  void eat() {
    // خب ربات‌ها که نمی‌خورن! 🤖
    throw Exception("ربات‌ها نمی‌تونن غذا بخورن");
  }
}
```

بهتره رابط‌ها رو به دسته‌های کوچیک‌تر تقسیم کنیم:

```dart
abstract class Worker {
  void work();
}

abstract class Eater {
  void eat();
}

class Robot implements Worker {
  @override
  void work() {
    print("کار می‌کنه");
  }
}

class Human implements Worker, Eater {
  @override
  void work() {
    print("کار می‌کنه");
  }

  @override
  void eat() {
    print("غذا می‌خوره");
  }
}
```

اینطوری کلاس‌ها فقط چیزهایی که لازم دارن رو پیاده‌سازی می‌کنن، نه بیشتر. 🔧

## 5. اصل وارونگی وابستگی 🏗️

این اصل میگه که کلاس‌ها نباید به جزئیات وابسته باشن؛ بلکه باید به رابط‌ها یا ابسترکشن‌ها وابسته باشن. 
این یعنی به جای اینکه به کلاس‌های خاصی تکیه کنی، به رابط‌هایی تکیه کن که به راحتی میشه اون‌ها رو جایگزین کرد.

**مثال بد:**

```dart
class Database {
  void save(String data) {
    // ذخیره داده در دیتابیس
  }
}

class DataManager {
  final Database db = Database();
  
  void saveData(String data) {
    db.save(data);
  }
}
```

مشکل اینجاست که DataManager کاملاً به کلاس Database وابسته‌ هست. 
اگر بخوایم دیتابیس رو تغییر بدیم، باید کل کد رو تغییر بدیم. 
بهتره از وابستگی به جزئیات دوری کنیم:

```dart
abstract class DataStorage {
  void save(String data);
}

class Database implements DataStorage {
  @override
  void save(String data) {
    // ذخیره داده در دیتابیس
  }
}

class DataManager {
  final DataStorage storage;
  
  DataManager(this.storage);
  
  void saveData(String data) {
    storage.save(data);
  }
}
```

حالا می‌تونیم از هر نوع ذخیره‌سازی دیگه‌ای (مثلاً فایل) استفاده کنیم، بدون اینکه کدهای دیگر تغییر کنن. 🔄

---

خیلی ساده بود، نه؟
الان SOLID رو یاد گرفتی. پس با قدرت برو جلو و کدهاتو متحول کن! 🚀
