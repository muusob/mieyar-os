
# Dockerfile for Meyar Brief Generator
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM node:20-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3000

COPY package*.json ./
RUN npm ci --only=production

COPY --from=builder /app/dist ./dist

EXPOSE 3000

CMD ["node", "dist/server.cjs"]# 📐 منصة مِعْيار - صانع موجز الهوية البصرية الذكي (Meyar Brief Generator)

[![Node.js Version](https://img.shields.io/badge/Node.js-18%2B-brightgreen.svg)](https://nodejs.org/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Framework](https://img.shields.io/badge/Framework-React%2019%20%2B%20Vite%20%2B%20Express-black.svg)](https://react.dev)

**مِعْيار** هي منصة تفاعلية متكاملة مدعومة بالذكاء الاصطناعي (Google Gemini AI) صُممت لمساعدة المصممين وأصحاب الأعمال على إنشاء صياغة علمية ودقيقة لـ **موجز الهوية البصرية (Brand Brief)** بنقرات بسيطة، مع دعم كامل للدمج السلس داخل **مدونات بلوجر (Blogger)** دون تعارضات أو مشاكل أمنية.

---

## ✨ المميزات الرئيسية

1. **صانع الموجز الإبداعي الذكي (AI Brand Brief):**
   - تحليل قطاع العمل ورسالة العلامة والجمهور المستهدف.
   - اقتراح الألوان المناسبة واستخراج الدلالات النفسية (Color Psychology).
   - توضيح خيارات شعار اللفظة (Logotype) وسيكولوجية الأشكال.

2. **دعم الدمج المباشر في بلوجر (Blogger Embed):**
   - ملف `blogger-embed.html` جاهز للنسخ المباشر في أداة `HTML/JavaScript` ببلوجر.
   - ملف `بلوجر.XML` كقالب متكامل يتضمن كافة وسوم العناصر الهيكلية والـ `b:skin`.
   - تعديل تلقائي للارتفاع (Auto Resizing) لمنع ظهور أشرطة التمرير المزدوجة داخل المدونة.
   - إعدادات أمان تسمح بالدمج عبر الـ `iFrame` وتمنع حظر `X-Frame-Options`.

3. **نظام استبيانات وتقييمات تفاعلي (Evaluation & Surveys):**
   - استبيان سريع للزوار لتقييم تجربة الاستخدام وإبداء الملاحظات.
   - لوحة تحكم محمية بكلمة مرور للمصمم/المشرف لمراجعة الإحصائيات وتصدير التقارير بتنسيق `CSV`.

4. **تصدير وطباعة احترافية:**
   - طباعة وتصدير البريف بصيغة PDF أو مشاركته عبر واتساب بنقرة واحدة.

---

## 🚀 التشغيل المحلي (Local Setup)

### المتطلبات الأساسية
- **Node.js**: الإصدار 18 أو أحدث.
- **npm** أو **bun**.
- مفتاح API الخاص بـ **Google Gemini API**.

### الخطوات:

1. **استنساخ المستودع (Clone Repository):**
   ```bash
   git clone https://github.com/username/meyar-brief-generator.git
   cd meyar-brief-generator
   ```

2. **تثبيت الحزم والاعتمادات (Install Dependencies):**
   ```bash
   npm install
   ```

3. **إعداد متغيرات البيئة (Environment Variables):**
   قم بإنشاء ملف `.env` بناءً على `.env.example`:
   ```env
   GEMINI_API_KEY="أدخل_مفتاح_جيمناي_هنا"
   PORT=3000
   ```

4. **تشغيل خادم التطوير (Development Server):**
   ```bash
   npm run dev
   ```
   سيتم تشغيل المنصة على: `http://localhost:3000`

---

## 🏗️ بناء المشروع والإنتاج (Production Build)

لبناء وتثبيت النسخة النهائية المجمعة للإنتاج:

```bash
# بناء التطبيق للخادم والواجهة
npm run build

# تشغيل الخادم الرئيسي المجمع
npm start
```

---

## 🐳 التشغيل بواسطة Docker (اختياري)

تم تزويد المستودع بملف `Dockerfile` لتسهيل النشر على Cloud Run أو أي خادم سحابي:

```bash
# بناء حاوية دكر
docker build -t meyar-app .

# تشغيل الحاوية
docker run -d -p 3000:3000 --env-file .env meyar-app
```

---

## 📝 كيفية دمج المنصة في منصة بلوجر (Blogger)

### الطريقة الأولى: استخدام أداة HTML/JavaScript (الأسلوب الأسهل)
1. افتح لوحة تحكم **Blogger** -> اختر **التنسيق (Layout)**.
2. أضف أداة جديدة من نوع **HTML/JavaScript**.
3. افتح ملف `blogger-embed.html` الموجود بهذا المستودع، وانسخ محتواه بالكامل وانشره في الأداة.

### الطريقة الثانية: إضافة كود المظهر الكامل (Theme XML)
1. افتح لوحة تحكم **Blogger** -> **المظهر (Theme)** -> **تعديل HTML**.
2. يمكنك استخدام الشفرة الموجودة في ملف `بلوجر.XML`.

---

## 📁 هيكل المشروع (Project Structure)

```
meyar-brief-generator/
├── server.ts              # خادم Express الرئيسي ومعالجة طلبات Gemini API ورؤوس الأمان
├── blogger-embed.html     # كود الاندماج المخصص لمدونات بلوجر مع التجاوب التلقائي
├── بلوجر.XML             # قالب بلوجر الهيكلي الجاهز
├── src/
│   ├── App.tsx            # المكون الرئيسي للواجهة التفاعلية والاستبيان
│   ├── main.tsx           # مدخل تطبيق React
│   ├── types.ts           # تعريف الأنواع والبيانات
│   └── index.css          # أنماط Tailwind CSS
├── public/                # الملفات الثابتة والشعارات
├── .env.example           # نموذج متغيرات البيئة
├── Dockerfile             # ملف إعداد الحاويات للنشر السحابي
└── package.json           # الحزم والسكربتات
```

---

## 👨‍🎨 الحقوق والإشراف

- **إشراف وتطوير:** المصمم مصعب
- **التواصل:** 0510500718
- **وسائل التواصل الاجتماعي:** `@MUUSOB` / منصة إكس: `@MUUS3B`
- **الترخيص:** MIT License

---
*تم تطوير المنصة بأعلى معايير التجاوب وسرعة الأداء وأمان البيانات.*
