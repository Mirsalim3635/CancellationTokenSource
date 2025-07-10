
# CancellationTokenSource nima?

**CancellationTokenSource** – bu C# tilida `Task` yoki asynchronous (asinxron) operatsiyalarni bekor qilish uchun ishlatiladigan klass.  
Masalan, fayl yuklash yoki ma’lumot o‘qish jarayonida ushbu klass yordamida ishni majburan to‘xtatish mumkin.

---

## 🛠 U qanday ishlaydi?

CancellationTokenSource ishlatishda **ikkita asosiy qism mavjud**:

| Qism | Vazifasi |
| --- | --- |
| `CancellationTokenSource` | Signal yuboradi |
| `CancellationToken` | Signalni qabul qiladi |

---

## ⚙️ Ishlash jarayoni

- Avval **CancellationTokenSource yaratiladi**.
- Undan **CancellationToken olinadi**.
- Vazifa boshlanganda unga shu **token beriladi**.
- Vazifa davomida token **doimiy tekshirilib turiladi**.
- Agar `Cancel()` chaqirilsa:
   - Token **CancellationTokenSource dan signal oladi**.
   - Vazifa **bekor qilinadi**.

---

## ❓ Nega kerak?

- **Keraksiz vazifalar bajarilmasligi uchun.**  
   Bu holatda CPU va RAM resurslari bo‘shatiladi.
- **API so‘rovlari** va **uzoq davom etadigan amallarda** juda muhim.
- **UI bloklanmaydi**, foydalanuvchi interfeysi muzlamaydi.

---

## 📌 Qayerlarda qo‘llaniladi?

Misol:

### 🚕 Taksi buyurtma qilishda

> Yandex Go orqali taksi chaqirsak (`Task` ishga tushadi).  
> Agar uzoq kutib haydovchi qabul qilmasa, **“Bekor qilish” tugmasini bosamiz** (`Cancel` signal beradi).  
> Natijada buyurtma **bekor qilinadi** (`Task stop bo‘ladi`).

---

## 📝 kod jarayonida ishlash ketma ketligi

1.**yaratish**


```csharp
CancellationTokenSource cts = new CancellationTokenSource();
```
2.**Token olish**
```csharp
CancellationToken token = cts.Token;
```
3.**Taskni boshlash**
```csharp
Task.Run(() =>
{
    while (true)
    {
        if (token.IsCancellationRequested)
        {
            Console.WriteLine("Bekor qilindi.");
            break;
        }
        // vazifa davom etadi
    }
}, token);
```
4.**Bekor qilish**
```csharp
cts.Cancel();

```
---
