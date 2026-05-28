# نظام الكاشير - إيفو ماركت

نظام كاشير متكامل لسوبرماركت مبني بـ Node.js + React.

## المتطلبات

- Node.js 18 أو أحدث
- قاعدة بيانات PostgreSQL

## طريقة التشغيل

### 1. إعداد قاعدة البيانات

أنشئ قاعدة بيانات PostgreSQL وشغّل هذا SQL لإنشاء الجداول:

```sql
CREATE TABLE IF NOT EXISTS categories (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS products (
  id SERIAL PRIMARY KEY,
  barcode TEXT,
  name TEXT NOT NULL,
  price NUMERIC(10,2) NOT NULL,
  stock_quantity INTEGER NOT NULL DEFAULT 0,
  unit TEXT NOT NULL DEFAULT 'قطعة',
  category_id INTEGER REFERENCES categories(id)
);

CREATE TABLE IF NOT EXISTS sales (
  id SERIAL PRIMARY KEY,
  total NUMERIC(10,2) NOT NULL,
  discount NUMERIC(10,2),
  amount_paid NUMERIC(10,2),
  change_due NUMERIC(10,2),
  payment_method TEXT NOT NULL DEFAULT 'cash',
  items_count INTEGER NOT NULL DEFAULT 0,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE TABLE IF NOT EXISTS sale_items (
  id SERIAL PRIMARY KEY,
  sale_id INTEGER NOT NULL REFERENCES sales(id),
  product_id INTEGER NOT NULL REFERENCES products(id),
  quantity NUMERIC(10,3) NOT NULL,
  unit_price NUMERIC(10,2) NOT NULL,
  subtotal NUMERIC(10,2) NOT NULL
);
```

### 2. إعداد متغيرات البيئة

انسخ ملف `.env.example` إلى `.env` وعدّل القيم:

```bash
cp .env.example .env
```

ثم عدّل `DATABASE_URL` بمعلومات قاعدة بياناتك:
```
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE_NAME
PORT=3000
NODE_ENV=production
```

### 3. تشغيل التطبيق

```bash
node index.mjs
```

افتح المتصفح على: `http://localhost:3000`

## الصفحات

- `/` — واجهة الكاشير (نقطة البيع)
- `/dashboard` — لوحة القيادة
- `/products` — إدارة المنتجات
- `/categories` — إدارة الأقسام
- `/sales` — سجل المبيعات

## الاستضافة على Railway / Render / VPS

1. ارفع الملفات على السيرفر
2. اضبط متغير البيئة `DATABASE_URL`
3. شغّل `node index.mjs`
