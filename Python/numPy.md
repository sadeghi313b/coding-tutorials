# 📦 توابع NumPy - دسته‌بندی شده

  

## 1. ساخت آرایه‌ها (Array Creation)
 
| تابع                            |             توضیح              | مثال                   |
| ------------------------------- | :----------------------------: | ---------------------- |
| `np.array()`                    |  ایجاد آرایه از لیست یا تاپل   | `np.array([1, 2, 3])`  |
| `np.zeros(shape)`               |     آرایه‌ای با مقادیر صفر     | `np.zeros((2, 3))`     |
| `np.ones(shape)`                |     آرایه‌ای با مقادیر یک      | `np.ones((3, 3))`      |
| `np.full(shape, fill_value)`    |    آرایه‌ای با مقدار دلخواه    | `np.full((2, 2), 7)`   |
| `np.eye(N)`                     |          ماتریس همانی          | `np.eye(3)`            |
| `np.arange(start, stop, step)`  |    تولید آرایه با گام مشخص     | `np.arange(0, 10, 2)`  |
| `np.linspace(start, stop, num)` | تولید آرایه با تعداد نقاط مشخص | `np.linspace(0, 1, 5)` |
 
---

  

## 2. تغییر شکل و دستکاری آرایه (Array Manipulation)

  

| تابع | توضیح | مثال |

|------|:------:|-------|

| `np.reshape(a, newshape)` | تغییر شکل آرایه | `np.reshape(a, (2, 3))` |

| `np.ravel(a)` | تبدیل آرایه به یک‌بعدی | `np.ravel(a)` |

| `np.transpose(a)` | ترانهاده کردن آرایه | `np.transpose(a)` |

| `np.concatenate((a, b), axis=0)` | اتصال آرایه‌ها | `np.concatenate((a, b))` |

| `np.stack((a, b), axis=0)` | پشته‌سازی آرایه‌ها | `np.stack((a, b))` |

| `np.split(a, indices)` | تقسیم آرایه | `np.split(a, 3)` |

| `np.tile(a, reps)` | تکرار آرایه | `np.tile(a, (2, 3))` |

  

---

  

## 3. عملیات ریاضی پایه (Basic Mathematical Operations)

  

| تابع | توضیح | مثال |

|------|:------:|-------|

| `np.add(a, b)` | جمع عنصر به عنصر | `np.add([1, 2], [3, 4])` |

| `np.subtract(a, b)` | تفریق عنصر به عنصر | `np.subtract([5, 6], [2, 3])` |

| `np.multiply(a, b)` | ضرب عنصر به عنصر | `np.multiply([2, 3], [4, 5])` |

| `np.divide(a, b)` | تقسیم عنصر به عنصر | `np.divide([10, 20], [2, 4])` |

| `np.power(a, b)` | توان عنصر به عنصر | `np.power([2, 3], 2)` |

| `np.mod(a, b)` | باقیمانده تقسیم | `np.mod([5, 7], [2, 3])` |

  

---

  

## 4. توابع ریاضی پیشرفته (Advanced Math Functions)

  

| تابع | توضیح | مثال |

|------|:------:|-------|

| `np.exp(x)` | تابع نمایی (e^x) | `np.exp([1, 2])` |

| `np.log(x)` | لگاریتم طبیعی (پایه e) | `np.log([1, np.e])` |

| `np.log10(x)` | لگاریتم پایه ۱۰ | `np.log10([1, 10])` |

| `np.sqrt(x)` | جذر | `np.sqrt([4, 9])` |

| `np.sin(x)` | سینوس | `np.sin(np.pi / 2)` |

| `np.cos(x)` | کسینوس | `np.cos(0)` |

| `np.tan(x)` | تانژانت | `np.tan(np.pi / 4)` |

| `np.arcsin(x)` | معکوس سینوس | `np.arcsin(1)` |

| `np.arccos(x)` | معکوس کسینوس | `np.arccos(1)` |

| `np.arctan(x)` | معکوس تانژانت | `np.arctan(1)` |

  

---

  

## 5. آمار و احتمال (Statistics and Probability)

  

| تابع | توضیح | مثال |

|------|:------:|-------|

| `np.mean(a)` | میانگین | `np.mean([1, 2, 3])` |

| `np.median(a)` | میانه | `np.median([1, 2, 3])` |

| `np.std(a)` | انحراف معیار | `np.std([1, 2, 3])` |

| `np.var(a)` | واریانس | `np.var([1, 2, 3])` |

| `np.min(a)` | کمینه | `np.min([1, 2, 3])` |

| `np.max(a)` | بیشینه | `np.max([1, 2, 3])` |

| `np.percentile(a, q)` | صدک | `np.percentile([1, 2, 3], 50)` |

  

---

  

## 6. توابع تصادفی (Random Functions)

  

| تابع | توضیح | مثال |

|------|:------:|-------|

| `np.random.rand(d0, d1, ...)` | توزیع یکنواخت بین ۰ و ۱ | `np.random.rand(2, 3)` |

| `np.random.randn(d0, d1, ...)` | توزیع نرمال استاندارد | `np.random.randn(2, 3)` |

| `np.random.randint(low, high, size)` | اعداد صحیح تصادفی | `np.random.randint(0, 10, (2, 3))` |

| `np.random.choice(a, size)` | انتخاب تصادفی از آرایه | `np.random.choice([1, 2, 3], 2)` |

| `np.random.seed(seed)` | تنظیم بذر تصادفی | `np.random.seed(42)` |

  

---

  

## 7. عملیات منطقی و مقایسه‌ای (Logical and Comparison)

  

| تابع | توضیح | مثال |

|------|:------:|-------|

| `np.equal(a, b)` | بررسی تساوی عنصر به عنصر | `np.equal([1, 2], [1, 3])` |

| `np.not_equal(a, b)` | بررسی نابرابری عنصر به عنصر | `np.not_equal([1, 2], [1, 3])` |

| `np.greater(a, b)` | بررسی بزرگ‌تر بودن | `np.greater([2, 3], [1, 4])` |

| `np.less(a, b)` | بررسی کوچک‌تر بودن | `np.less([2, 3], [3, 2])` |

  

---

  

## 8. ضرب برداری و نقطه‌ای (Dot & Cross Product)

  

| تابع | توضیح | مثال |

|------|:------:|-------|

| `np.dot(a, b)` | ضرب نقطه‌ای دو آرایه | `np.dot([1, 2], [3, 4])` ⟶ `11` |

| `np.vdot(a, b)` | ضرب نقطه‌ای بردارهای تخت‌شده (همراه با مزدوج مختلط) | `np.vdot([1+2j], [3+4j])` |

| `np.inner(a, b)` | ضرب داخلی (inner product) | `np.inner([1, 2], [3, 4])` ⟶ `11` |

| `np.outer(a, b)` | ضرب خارجی (outer product) | `np.outer([1, 2], [3, 4])` ⟶ `[[3, 4], [6, 8]]` |

| `np.cross(a, b)` | ضرب برداری (بردار متعامد در فضای 3بعدی) | `np.cross([1, 0, 0], [0, 1, 0])` ⟶ `[0, 0, 1]` |

| `np.matmul(a, b)` | ضرب ماتریسی (عملگر @) | `np.matmul([[1,2]], [[3],[4]])` ⟶ `[[11]]` |

| `a @ b` | عملگر میانبر برای ضرب ماتریسی | `a @ b` همان `np.matmul(a, b)` است |