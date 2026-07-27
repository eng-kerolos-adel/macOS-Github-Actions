<div align="center">

# 🍎 macOS Build from Windows — Free with GitHub Actions

**بتشتغل على Windows وعايز تبني تطبيق macOS؟ مش محتاج ماك بوك!**
Build macOS apps from any OS — completely free using GitHub Actions CI/CD.

[![Flutter](https://img.shields.io/badge/Flutter-3.22.0-blue?logo=flutter)](https://flutter.dev)
[![Tauri](https://img.shields.io/badge/Tauri-2.x-orange?logo=tauri)](https://tauri.app)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![KeroCodes](https://img.shields.io/badge/by-KeroCodes-purple)](https://youtube.com/@KeroCodes)

<br/>

[🎬 شوف الفيديو](https://youtube.com/shorts/lebBPnhlH-E) · [⬇️ تحميل الـ Workflow](#️-تحميل-الـ-workflow) · [📖 الشرح بالعربي](#-الشرح-بالعربي) · [📖 English Docs](#-english-docs)

</div>

---

## 📋 المحتويات

- [⚡ Quick Start](#-quick-start)
- [📖 الشرح بالعربي](#-الشرح-بالعربي)
- [📖 English Docs](#-english-docs)
- [🗂️ هيكل المشروع](#️-هيكل-المشروع)
- [⚙️ إعداد الـ Workflow](#️-إعداد-الـ-workflow)
- [📥 تحميل الـ Build](#-تحميل-الـ-build)
- [❓ الأسئلة الشايعة](#-الأسئلة-الشايعة)
- [🤝 المساهمة](#-المساهمة)

---

## ⚡ Quick Start

```bash
# 1. انسخ الـ Repository
git clone https://github.com/eng-kerolos-adel/macOS-Github-Actions.git

# 2. انسخ مجلد .github جوه مشروعك
cp -r macos-github-actions/.github /path/to/your-project/

# 3. افتح الفايل وغيّر الـ Flutter version
# .github/workflows/main.yml  →  flutter-version: 'YOUR_VERSION'

# 4. Push وشوف السحر!
git add . && git commit -m "Add macOS CI/CD" && git push
```

> 🎉 روح **GitHub → Actions** وشوف تطبيقك بيتبني على Mac مجاناً!

---

## 📖 الشرح بالعربي

### المشكلة

أبل قافلة بيئتها — عشان تعمل Build لأي تطبيق macOS، محتاج **Xcode** اللي مش بينزل غير على أجهزة أبل. يعني نظرياً محتاج تشتري ماك بوك بـ **100,000 جنيه** مجرد عشان تعمل Export!

### الحل

**GitHub بتوفر سيرفرات Mac حقيقية مجاناً** كجزء من منصة CI/CD بتاعتها.

إنت بتكتب كودك عادي على Windows، وبـ **5 سطور** في فايل صغير — أول ما تعمل `push`، سيرفرات GitHub هتقوم بالشغل كله وترجعلك ملف `.dmg` جاهز.

### الـ Key Line السحرية

```yaml
runs-on: macos-latest  # ← سيرفر Mac مجاني كامل بـ Xcode!
```

---

## 📖 English Docs

### The Problem

Apple locks down its entire build environment. To build any macOS app, you need **Xcode** — which only runs on Apple hardware. That means theoretically buying a MacBook ($2,500+) just to export your project.

### The Solution

**GitHub provides real Mac servers for free** as part of their CI/CD platform. You write code on Windows, add 5 lines to a config file, push to GitHub — and their servers build your macOS app automatically.

### How It Works

```
Your Windows Machine          GitHub Cloud
─────────────────             ─────────────────────
Write Flutter/Tauri code  →   Spin up macOS Server
git push to GitHub        →   Install Xcode + Flutter
                          →   Run: flutter build macos
                          →   Upload .app / .dmg artifact
                              ↓
                          You download the finished app ✅
```

---

## 🗂️ هيكل المشروع

```
.github/
└── workflows/
    └── main.yml          ← الـ Workflow الكامل (Flutter + Tauri + Expo)

README.md                 ← إنت هنا 👋
LICENSE                   ← MIT License
```

---

## ⚙️ إعداد الـ Workflow

### للـ Flutter Projects ✅

الـ Workflow شغّال فوراً. بس افعل ده:

**1. غيّر الـ Flutter version في `main.yml`:**

```yaml
- name: Setup Flutter
  uses: subosito/flutter-action@v2
  with:
    flutter-version: '3.22.0'   # ← غيّرها لـ version بتاعتك
```

مش عارف الـ version بتاعتك؟

```bash
flutter --version
```

**2. تأكد إن macOS Desktop مفعّل في مشروعك:**

```bash
flutter config --enable-macos-desktop
flutter create --platforms=macos .
```

---

### للـ Tauri Projects 🦀

في `main.yml`، في الـ job اللي اسمه `build-macos-tauri`، غيّر:

```yaml
# قبل
if: false

# بعد
if: true
```

وعطّل الـ Flutter job بحط `if: false` عنده.

---

### للـ React Native / Expo Projects ⚛️

نفس الكلام، في الـ job اللي اسمه `build-macos-expo`:

```yaml
# قبل
if: false

# بعد
if: true
```

---

### Triggers — امتى يشتغل الـ Workflow؟

```yaml
on:
  push:
    branches: [main, develop]     # عند كل push
  pull_request:
    branches: [main]              # عند فتح Pull Request
  workflow_dispatch:              # أو تشغيله يدوي من GitHub
```

---

## 📥 تحميل الـ Build

بعد ما الـ Workflow يخلص:

1. روح **GitHub → Actions**
2. اضغط على آخر **Workflow Run**
3. تحت **Artifacts** في أسفل الصفحة، هتلاقي:

| الملف | الاستخدام |
|-------|-----------|
| `macOS-App-[sha].zip` | يحتوي على `.app` و `.dmg` |
| `.app` | التطبيق جاهز للتشغيل على أي Mac |
| `.dmg` | ملف تثبيت جاهز للتوزيع |

> الـ Artifacts بتتحذف تلقائياً بعد **14 يوم**. غيّر `retention-days` في الـ Workflow لو محتاج أكتر.

---

## ⬇️ تحميل الـ Workflow

**نسخ مباشر — جاهز للاستخدام:**

```bash
curl -o .github/workflows/main.yml \
  https://raw.githubusercontent.com/KeroCodes/macos-github-actions/main/.github/workflows/main.yml
```

أو من هنا:

**[⬇️ تحميل main.yml](https://github.com/KeroCodes/macos-github-actions/raw/main/.github/workflows/main.yml)**

---

## ❓ الأسئلة الشايعة

<details>
<summary><b>مجاني تماماً؟</b></summary>

آه! GitHub بيديك **2,000 دقيقة مجانية شهرياً** على macOS runners.
بما إن البيلد بياخد 5–15 دقيقة، عندك **130–400 build مجاني** في الشهر.

</details>

<details>
<summary><b>إيه الفرق بين .app و .dmg؟</b></summary>

- **`.app`** — التطبيق نفسه. اسحبه لأي Mac وهيشتغل.
- **`.dmg`** — ملف تثبيت للتوزيع (زي `.exe installer` على Windows).

</details>

<details>
<summary><b>ممكن أعمل iOS Build بنفس الطريقة؟</b></summary>

آه، بس iOS بيحتاج **Apple Developer Account** بـ $99/سنة للـ Code Signing.
macOS أسهل لأنك ممكن تتخطى الـ Signing للتجربة.

</details>

<details>
<summary><b>الـ Workflow فشل، إيه اللي أعمله؟</b></summary>

1. افتح الـ failed run في GitHub Actions
2. اضغط على الـ step اللي فشل
3. اقرأ الـ logs — هتقولك السبب بالظبط

أشيع أسباب الفشل:
- غلط في الـ Flutter version
- `macos` platform مش مفعّل في المشروع
- package بتاعك مش بيدعم macOS

</details>

<details>
<summary><b>Repository private يشتغل؟</b></summary>

آه! الـ 2,000 دقيقة المجانية بتشمل الـ Private و Public repositories.

</details>

---

## 🤝 المساهمة

لو عندك اقتراح أو لقيت bug:

1. افتح **Issue** بتوصف المشكلة
2. أو ابعت **Pull Request** بالحل مباشرة

كل مساهمة بتتراجع خلال 48 ساعة. ✅

---

<div align="center">

**صنعه بحب من مصر 🇪🇬**

[![YouTube](https://img.shields.io/badge/YouTube-KeroCodes-red?logo=youtube)](https://youtube.com/@KeroCodes)
[![Instagram](https://img.shields.io/badge/Instagram-KeroCodes-purple?logo=instagram)](https://instagram.com/Kero_Codes)
[![Facebook](https://img.shields.io/badge/Facebook-KeroCodes-blue?logo=facebook)](https://facebook.com/Kero.Codes)

*لو الريبو ده وفّرلك فلوس الماك بوك، ابعت ⭐ — بتساعدنا نوصل لمبرمجين تانيين!*

</div>
