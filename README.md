# Use Case Diagram - Belle Beauty Salon

فيما يلي كود Mermaid يطابق الأسلوب العام للمخطط في الصورة، مع تمثيل المستخدمين على الجانبين ووظائف النظام داخل الصندوق الرئيسي.

```mermaid
flowchart LR
    subgraph System[Beauty Salon System]
        direction TB

        U1["register"]
        U2["Manage\nprofile"]
        U3["Browse\nservices"]
        U4["Book\nAppointment"]
        U5["Browse\nstore"]
        U6["Manage\nBookings"]
        U7["login"]
        U8["Reset\nPassword"]
        U9["View\nSchedule"]
        U10["Update\nBooking\nStatus"]
        U11["Manage\nprofile"]
        U12["Manage\nWorker"]
        U13["Manage\nServices"]
        U14["Manage\nstore"]
        U15["view Profiles\nand Reports"]
        U16["Checkout"]
        U17["Add to\ncart"]
        U18["AI image\nanalysis"]
        U19["Check 2 Hour\npolicy"]
    end

    Customer((customer))
    Worker((worker))
    Admin((admin))

    Customer --> U1
    Customer --> U2
    Customer --> U3
    Customer --> U4
    Customer --> U5
    Customer --> U6
    Customer --> U7
    Customer --> U8
    Customer --> U9
    Customer --> U10
    Customer --> U16
    Customer --> U17
    Customer --> U18

    U3 -. extends .-> U18
    U4 -. includes .-> U19
    U17 -. includes .-> U16
    U7 -. extends .-> U8
    U9 -. extends .-> U10

    Worker --> U9
    Worker --> U10
    Worker --> U11

    Admin --> U12
    Admin --> U13
    Admin --> U14
    Admin --> U15

    U12 -. extends .-> U13
    U13 -. extends .-> U14
    U15 -. extends .-> U12

    classDef actor fill:#fff,stroke:#000,stroke-width:2px,color:#000;
    classDef usecase fill:#fff,stroke:#000,stroke-width:1.5px,color:#000;

    class Customer,Worker,Admin actor;
    class U1,U2,U3,U4,U5,U6,U7,U8,U9,U10,U11,U12,U13,U14,U15,U16,U17,U18,U19 usecase;
```

## ملاحظات
- هذا المخطط يوضح حالات الاستخدام الأساسية للمشروع.
- تم حساب العلاقات بشكل عام لتقريب أسلوب الصورة، مع وجود `extends` و `includes` لتوضيح العلاقة بين الحالات.
- إذا أردت، أستطيع تعديل هذا الكود ليصبح أقرب إلى الصورة بشكل أكثر دقة من ناحية الترتيب والحجم أو تحويله إلى نسخة عربية/إنجليزية كاملة.
