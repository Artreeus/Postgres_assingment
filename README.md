# PostgreSQL Basics - Questions and Answers

## 1. What is PostgreSQL?

PostgreSQL হল একটি উন্নত, ওপেন-সোর্স রিলেশনাল ডাটাবেস সিস্টেম। 

## 2. What is the purpose of a database schema in PostgreSQL?

PostgreSQL-এ একটি স্কিমা হল আপনার ডাটাবেস অবজেক্টগুলি সংগঠিত করার একটি পদ্ধতি। 

## 3. Explain the **Primary Key** and **Foreign Key** concepts in PostgreSQL.

**প্রাইমারি কি** হল টেবিলের একটি কলাম বা কলামের সমষ্টি যা প্রতিটি রেকর্ডকে অনন্যভাবে চিহ্নিত করে। 

**ফরেন কি** হল এমন একটি কলাম যা অন্য টেবিলের প্রাইমারি কি'র সাথে সম্পর্ক স্থাপন করে। 

## 4. What is the difference between the `VARCHAR` and `CHAR` data types?

`VARCHAR` হল পরিবর্তনশীল দৈর্ঘ্যের টেক্সট স্টোরেজ। 
অন্যদিকে, `CHAR` নির্দিষ্ট দৈর্ঘ্যের টেক্সট স্টোরেজ। 

## 5. Explain the purpose of the `WHERE` clause in a `SELECT` statement.

`WHERE` ক্লজ হল SQL-এর ফিল্টারিং টুল। এটি ব্যবহার করে আমরা শুধুমাত্র সেই সারিগুলি পেতে পারি যা আমাদের নির্দিষ্ট শর্ত পূরণ করে। 

## 6. What are the `LIMIT` and `OFFSET` clauses used for?

`LIMIT` এবং `OFFSET` ক্লজগুলি ডাটা পেজিনেশনের জন্য ব্যবহৃত হয়, যা বড় ডাটাসেটকে ছোট, ব্যবস্থাপনাযোগ্য অংশে ভাগ করতে সাহায্য করে।

`LIMIT` নির্দিষ্ট করে কতগুলি সারি ফেরত আসবে। আমি এটাকে শপিং লিস্টে "শুধু ১০টি আপেল নিন" লেখার মতো ভাবি। উদাহরণস্বরূপ, `LIMIT 10` শুধুমাত্র প্রথম ১০টি ফলাফল দেখাবে।

`OFFSET` নির্দিষ্ট করে কতগুলি সারি এড়িয়ে যাবে। আমি এটাকে "প্রথম ২০টি এড়িয়ে, তারপর আমাকে পরের ১০টি দেখাও" বলার মতো ভাবি। উদাহরণস্বরূপ, `OFFSET 20` প্রথম ২০টি সারি এড়িয়ে যাবে।


## 7. How can you modify data using `UPDATE` statements?

`UPDATE` স্টেটমেন্ট আমাদেরকে ইতিমধ্যে ডাটাবেসে থাকা তথ্য পরিবর্তন করতে সাহায্য করে। আমার জন্য, এটি একটি বই-এর মধ্যে কিছু তথ্য সংশোধন করার মতো - আপনি পুরো বইটি পুনরায় লিখছেন না, কেবল নির্দিষ্ট অংশগুলি পরিবর্তন করছেন।

একটি বেসিক `UPDATE` স্টেটমেন্ট দেখতে এমন:
```sql
UPDATE customers
SET email = 'new_email@example.com', last_updated = CURRENT_DATE
WHERE customer_id = 123;
```


## 8. What is the significance of the `JOIN` operation, and how does it work in PostgreSQL?

`JOIN` অপারেশন রিলেশনাল ডাটাবেসের একটি মূল বৈশিষ্ট্য। এটি বিভিন্ন টেবিলের তথ্য একত্রিত করতে ব্যবহৃত হয়। 
উদাহরণস্বরূপ, আমার যদি `customers` এবং `orders` দুটি টেবিল থাকে, তাহলে আমি জানতে চাই কোন গ্রাহক কী অর্ডার করেছেন, এভাবে:

```sql
SELECT customers.name, orders.product, orders.order_date
FROM customers
JOIN orders ON customers.id = orders.customer_id;
```


## 9. Explain the `GROUP BY` clause and its role in aggregation operations.

`GROUP BY` ক্লজ হল একটি শক্তিশালী টুল যা সমান মানসম্পন্ন ডাটাকে একত্রিত করে এবং তাদের উপর অ্যাগ্রিগেট ফাংশন প্রয়োগ করতে দেয়।

উদাহরণস্বরূপ:
```sql
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department;
```

## 10. How can you calculate aggregate functions like `COUNT()`, `SUM()`, and `AVG()` in PostgreSQL?

PostgreSQL-এ অ্যাগ্রিগেট ফাংশন হল এমন বিশেষ ফাংশন যা একাধিক রেকর্ড থেকে একটি একক মান গণনা করে। 

প্রধান অ্যাগ্রিগেট ফাংশনগুলি:

**`COUNT()`**: এটি কতগুলি সারি রয়েছে তা গণনা করে।
```sql
SELECT COUNT(*) FROM orders; -- সব অর্ডারের সংখ্যা
SELECT COUNT(customer_id) FROM orders; -- NULL ছাড়া অর্ডারের সংখ্যা
```

**`SUM()`**: এটি নির্দিষ্ট কলামের মানগুলির যোগফল গণনা করে।
```sql
SELECT SUM(order_amount) FROM orders; -- সব অর্ডারের মোট মূল্য
SELECT SUM(quantity) FROM order_items WHERE product_id = 123; -- নির্দিষ্ট পণ্যের মোট বিক্রি
```

**`AVG()`**: এটি নির্দিষ্ট কলামের মানগুলির গড় গণনা করে।
```sql
SELECT AVG(price) FROM products; -- সব পণ্যের গড় মূল্য
SELECT AVG(rating) FROM reviews WHERE product_id = 456; -- নির্দিষ্ট পণ্যের গড় রেটিং
```

এছাড়াও **`MIN()`** এবং **`MAX()`** ফাংশন রয়েছে, যা সর্বনিম্ন এবং সর্বোচ্চ মান খুঁজে বের করে:
```sql
SELECT MIN(price), MAX(price) FROM products; -- সবচেয়ে সস্তা এবং সবচেয়ে দামি পণ্য
```
