# وثيقة معماريّة GetX في تطبيق Belle

## مقدمة

يتبع هذا التطبيق نموذج GetX المعتاد في Flutter، حيث يتم تقسيم التطبيق إلى:

- Controllers: إدارة المنطق والـ state
- Views: عناصر الواجهة
- Models: تمثيل البيانات
- Bindings: تسجيل الكائنات المدارة
- Routes: توجيه الانتقال بين الصفحات
- Services: التعامل مع الخادم

## 1) مخطط العمارة المنطقية

هذا المخطط يوضح كيف تقسم البنية التطبيق إلى طبقات واضحة وتعمل معاً داخل GetX.

```mermaid
flowchart TB
    subgraph UI[Views]
        V1[Auth Screens]
        V2[Home Screens]
        V3[Booking Screens]
        V4[Profile Screens]
        V5[Admin Screens]
    end

    subgraph STATE[Controllers]
        C1[AuthController]
        C2[HomeController]
        C3[BookingController]
        C4[FavoriteController]
        C5[AdminController]
    end

    subgraph DATA[Models]
        M1[UserModel]
        M2[ServiceModel]
        M3[CategoryModel]
        M4[AppointmentModel]
    end

    subgraph INFRA[Infrastructure]
        B[Bindings]
        R[Routes / AppPages]
        S[ApiService]
        A[Backend API]
    end

    V1 --> C1
    V2 --> C2
    V3 --> C3
    V4 --> C4
    V5 --> C5

    C1 --> M1
    C2 --> M2
    C3 --> M2
    C4 --> M2
    C5 --> M3

    B --> C1
    B --> C2
    B --> C3
    B --> C4
    B --> C5

    R --> V1
    R --> V2
    R --> V3
    R --> V4
    R --> V5

    C1 --> S
    C2 --> S
    C3 --> S
    C5 --> S
    S --> A
```

## 2) مخطط تدفق الحجز في التطبيق

هذا المخطط يوضح مسار المستخدم عند حجز خدمة من قائمة الخدمات حتى تأكيد الحجز.

```mermaid
flowchart LR
    A[HomeScreen] --> B[اختيار خدمة]
    B --> C[ServiceDetailsScreen]
    C --> D[BookingController]
    D --> E[SelectDateScreen]
    E --> F[SelectTimeScreen]
    F --> G[BookingSummaryScreen]
    G --> H[تأكيد الحجز]
    H --> I[BookingConfirmedScreen]
    D --> J[ApiService]
    J --> K[Backend API]
    K --> L[تحديث حالة الحجز]
    L --> I
```

## 3) مخطط التسلسل للحصول على قائمة الخدمات

هذا المخطط يوضح كيف يتم جلب قائمة الخدمات من الخادم وتحديث واجهة المستخدم عبر GetX.

```mermaid
sequenceDiagram
    participant User
    participant HomeScreen
    participant HomeController
    participant ApiService
    participant Backend

    User->>HomeScreen: فتح الشاشة الرئيسية
    HomeScreen->>HomeController: onInit()
    HomeController->>ApiService: get('/services')
    ApiService->>Backend: طلب قائمة الخدمات
    Backend-->>ApiService: JSON من الخدمات
    ApiService-->>HomeController: البيانات المستلمة
    HomeController->>HomeController: تحديث observable list
    HomeController-->>HomeScreen: Rebuild UI
    HomeScreen-->>User: عرض الخدمات والعروض
```

## 4) مخطط الصفوف لأهم النماذج والـ Controllers

```mermaid
classDiagram
    class GetxController
    class AuthController extends GetxController {
        +currentUser
        +isLoading
        +createAccount()
        +verifySignupCode()
        +login()
    }

    class BookingController extends GetxController {
        +selectedService
        +selectedDate
        +selectedTime
        +submitBooking()
    }

    class FavoriteController extends GetxController {
        +favoriteList
        +toggleFavorite()
    }

    class AdminController extends GetxController {
        +services
        +bookings
        +addService()
        +updateService()
    }

    class UserModel {
        +id
        +name
        +email
        +phone
        +fromJson()
    }

    class ServiceModel {
        +id
        +serviceName
        +categoryName
        +price
        +rating
        +fromJson()
    }

    class AppRoutes {
        +loginScreen
        +mainScreen
        +serviceDetails
        +bookingSummary
        +adminDashboard
    }

    AuthController --> UserModel
    BookingController --> ServiceModel
    FavoriteController --> ServiceModel
    AdminController --> ServiceModel
    AppRoutes --> "getPages"
```

## 5) ملاحظات عن التنفيذ في المشروع

- تم استخدام `GetMaterialApp` في `main.dart` لتحديد إعدادات التطبيق العالمية.
- تم تعريف المسارات في `app_pages.dart` باستخدام `GetPage`.
- تم تسجيل الاعتماديات الأساسية في `bindings/initial_bindings.dart`.
- تستخدم الشاشات `Get.put()` و `Get.lazyPut()` وفق الحاجة.
- يتم استخدام `Rx` لإعادة بناء الواجهة عند تغيير الحالة.

## الخلاصة

تضمن هذه البنية تطبيقاً قابلًا للتطوير ومرنًا في إدارة الحالة، مع فواصل واضحة بين الشاشات، المنطق، والبيانات. هذا يسهّل إضافة ميزات جديدة مثل الحجز، الإدارة، المفضلة، أو المراسلات داخل التطبيق دون الحاجة لتغيير هيكل التطبيق بشكل كبير.
