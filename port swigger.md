## Lab 1: Unprotected Admin Functionality

* **Goal:** Delete user `carlos`
* **Solution:**
  1. Append `/robots.txt` to the URL to discover the admin panel route.
  2. Access the admin panel URL.
  3. Delete the user `carlos`.

---

## Lab 2: Unprotected Admin Functionality with Unpredictable URL

* **Goal:** Delete user `carlos`
* **Solution:**
  1. Inspect the home page source code using DevTools (`F12`).
  2. Find the JavaScript script containing the hidden admin URL.
  3. Access the hidden admin URL and delete user `carlos`.
---
Lab 3: User Role Controlled by Request Parameter

• Goal: Delete user carlos
• Solution:
  1. Log in to the application using credentials (wiener:peter).
  2. Open DevTools (F12) and go to Application -> Storage -> Cookies.
  3. Locate the `Admin` cookie and change its value from `false` to `true`.
  4. Access the `/admin` panel URL and delete user carlos.
  ---
  Lab 4: User Role Can Be Modified in User Profile

• Goal: Delete user carlos
• Solution:
  1. Log in using credentials (wiener:peter) and go to your account page.
  2. Update your email address and capture the request in Burp Suite.
  3. Send the email update request to Burp Repeater.
  4. Add `"roleid": 2` to the JSON body in the request and send it.
  5. Verify in the response that your `roleid` is changed to 2.
  6. Access the `/admin` panel URL and delete user carlos.
  ---
  Lab 5: User ID Controlled by Request Parameter

• Goal: Obtain and submit the API key for user carlos
• Solution:
  1. Log in using credentials (wiener:peter) and go to your account page.
  2. Notice that the URL contains `id=wiener`.
  3. Change the `id` parameter in the URL from `wiener` to `carlos`.
  4. Copy the API key displayed for user carlos and submit it.
  ---
  Lab 6: User ID Controlled by Request Parameter, with Unpredictable User IDs

• Goal: Find the GUID for carlos, obtain his API key, and submit it.
• Solution:
  1. Go to the blog home page and click on a post written by `carlos`.
  2. Copy carlos's GUID (`userId`) from the URL.
  3. Log in with credentials (`wiener:peter`) and go to `My account`.
  4. Replace your GUID in the `id` parameter with carlos's GUID.
  5. Copy the exposed API key for carlos and submit it via "Submit solution".
  ---
  Lab 7: User ID Controlled by Request Parameter with Data Leakage in Redirect

• Goal: Obtain the API key for user carlos and submit it.
• Solution:
  1. Log in using credentials (wiener:peter) and go to your account page.
  2. Capture the request to `/my-account?id=wiener` and send it to Burp Repeater.
  3. Change the `id` parameter from `wiener` to `carlos` and send the request.
  4. Inspect the response body (even though it returns a `302 Found` redirect) to find carlos's API key.
  5. Copy the exposed API key and submit it via "Submit solution".
  ---
  Lab 8: User ID Controlled by Request Parameter with Password Disclosure

• Goal: Retrieve the administrator's password, log in, and delete user carlos
• Solution:
  1. Log in using credentials (wiener:peter) and go to the user account page.
  2. Change the `id` parameter in the URL from `wiener` to `administrator`.
  3. Inspect the page source code (or the HTTP response) to view the prefilled password in the password input field.
  4. Log out, then log in as `administrator` using the retrieved password.
  5. Access the `/admin` panel and delete user carlos.
  ---
  Lab 9: Insecure Direct Object References (IDOR)

• Goal: Find the password for user carlos in chat logs and log into his account
• Solution:
  1. Open the lab and click on the "Live chat" tab.
  2. Send a message in the chat and click "View transcript" to download the log.
  3. Notice that the transcript file URL uses an incrementing number (e.g., `2.txt`).
  4. Change the filename in the URL to `1.txt` to view the initial chat transcript.
  5. Find carlos's password disclosed inside the chat log.
  6. Return to the login page and log in as `carlos` using the stolen credentials.
  ---
  Lab 10: URL-based Access Control Can Be Circumvented

• Goal: Access the admin panel and delete user carlos
• Solution:
  1. Try to access `/admin` and observe that the front-end system blocks access.
  2. Send the request to Burp Repeater and change the request URL path to `/`.
  3. Add the header `X-Original-URL: /admin` to the request and send it to access the admin panel.
  4. Change the header to `X-Original-URL: /admin/delete` and add `?username=carlos` to the request URL line.
  5. Send the request to successfully delete user carlos.
  ---
  Lab 11: Method-Based Access Control Can Be Circumvented

• Goal: Exploit flawed HTTP method access controls using non-admin credentials to promote user wiener to an administrator.
• Solution:
  1. Log in using admin credentials (`administrator:admin`) and access the admin panel.
  2. Promote user `carlos` and capture the `POST /admin-roles` HTTP request in Burp Suite, then send it to Repeater.
  3. Log out and log in using non-admin credentials (`wiener:peter`).
  4. Copy the session cookie of `wiener` and replace the admin session cookie in Burp Repeater with it.
  5. Try sending the request; notice the application responds with "Unauthorized".
  6. Right-click the request in Repeater and select "Change request method" to convert it from `POST` to `GET` (or manually change it and send parameters via URL string e.g. `?username=wiener&action=upgrade`).
  7. Send the request to bypass access controls and promote `wiener` to an administrator.
  ---
  Lab 12: Multi-step Process with No Access Control on One Step

• Goal: Exploit a flawed multi-step process to promote user wiener to an administrator.
• Solution:
  1. Log in using admin credentials (`administrator:admin`) and access the admin panel.
  2. Initiate promoting user `carlos` and complete the confirmation step (step 2).
  3. Capture the confirmation HTTP request (`POST /admin-roles` with `action=upgrade` and `confirmed=true`) in Burp Suite and send it to Repeater.
  4. Log out and log in using non-admin credentials (`wiener:peter`).
  5. Copy the session cookie of `wiener` and replace the admin session cookie in Burp Repeater with it.
  6. Change the parameter `username=carlos` to `username=wiener`.
  7. Send the request to bypass access controls on the confirmation step and promote `wiener` to an administrator.
  ---
  Lab 13: Referer-Based Access Control

• Goal: Exploit flawed access control based on the Referer header to promote user wiener to an administrator.
• Solution:
  1. Log in using admin credentials (`administrator:admin`) and access the admin panel[span_0](start_span)[span_0](end_span).
  2. Promote user `carlos` and capture the HTTP request in Burp Suite, then send it to Repeater[span_1](start_span)[span_1](end_span).
  3. Log out and log in using non-admin credentials (`wiener:peter`)[span_2](start_span)[span_2](end_span).
  4. Notice that directly accessing `/admin-roles?username=carlos&action=upgrade` without a proper `Referer` header treats the request as unauthorized[span_3](start_span)[span_3](end_span).
  5. In Burp Repeater, copy the session cookie of `wiener` into the captured admin request, change the username to `wiener`, and ensure the `Referer` header contains `/admin`[span_4](start_span)[span_4](end_span).
  6. Send the request to bypass access control and promote `wiener` to an administrator[span_5](start_span)[span_5](end_span).
  ---
  # توثيق لابات PortSwigger - Access Control Vulnerabilities

---

## Lab 1: Unprotected Admin Functionality
• **الهدف:** الوصول لصفحة الأدمن ومسح المستخدم `carlos`.
• **الحل:**
  1. اذهب للرابط `/robots.txt` في المتصفح.
  2. ابحث عن المسار المخفي الخاص بالإدارة (مثل `/administrator-panel`).
  3. التقط المسار وادخل عليه في المتصفح للوصول لصفحة الأدمن، ثم قم بحذف المستخدم `carlos`.

---

## Lab 2: Unprotected Admin Functionality with Unpredictable URL
• **الهدف:** مسح المستخدم `carlos`.
• **الحل:**
  1. افحص كود المصدر للصفحة الرئيسية باستخدام Developer Tools (`F12`).
  2. ابحث عن ملفات JavaScript التي تحتوي على رابط الأدمن المخفي/العشوائي.
  3. ادخل على الرابط المخفي واقم بحذف المستخدم `carlos`.

---

## Lab 3: User Role Controlled by Request Parameter
• **الهدف:** مسح المستخدم `carlos`.
• **الحل:**
  1. قم بتسجيل الدخول بالحساب المتاح (`wiener:peter`).
  2. افتح أدوات التطوير DevTools (`F12`) واذهب إلى `Application -> Storage -> Cookies`.
  3. ابحث عن الكوكي `Admin` وغير قيمتها من `false` إلى `true`.
  4. ادخل على الرابط `/admin` وقم بحذف المستخدم `carlos`.

---

## Lab 4: User Role Can Be Modified in User Profile
• **الهدف:** مسح المستخدم `carlos`.
• **الحل:**
  1. سجل الدخول بالحساب (`wiener:peter`) واذهب لصفحة الحساب الشخصي.
  2. قم بتحديث البريد الإلكتروني والتقط الطلب عبر Burp Suite.
  3. أرسل طلب التحديث إلى Burp Repeater.
  4. أضف الخاصية `"roleid": 2` داخل جسم طلب JSON ثم أرسله.
  5. تأكد من تغير الـ `roleid` إلى 2 في الاستجابة، ثم ادخل على `/admin` واحذف `carlos`.

---

## Lab 5: User ID Controlled by Request Parameter
• **الهدف:** الحصول على الـ API key الخاص بالمستخدم `carlos` وتسليمه.
• **الحل:**
  1. سجل الدخول بالحساب (`wiener:peter`) واذهب لصفحة الحساب.
  2. لاحظ وجود المعامل `id=wiener` في رابط الصفحة (URL).
  3. غيّر قيمة المعامل `id` من `wiener` إلى `carlos`.
  4. انسخ الـ API key الخاص بـ `carlos` واضغط على "Submit solution" لتسليمه.

---

## Lab 6: User ID Controlled by Request Parameter, with Unpredictable User IDs
• **الهدف:** العثور على GUID الخاص بـ `carlos` واستخراج الـ API key وتسليمه.
• **الحل:**
  1. اذهب للصفحة الرئيسية وافتح مقالاً مكتوباً بواسطة `carlos`.
  2. انسخ الـ GUID الخاص بكارلوس من رابط الصفحة (`userId`).
  3. سجل الدخول بـ (`wiener:peter`) واذهب لصفحة `My account`.
  4. استبدل الـ GUID الخاص بك في معامل الـ `id` بالـ GUID الخاص بـ `carlos`.
  5. انسخ الـ API key المكشوف لـ `carlos` وقدمه عبر "Submit solution".

---

## Lab 7: User ID Controlled by Request Parameter with Data Leakage in Redirect
• **الهدف:** استخراج الـ API key للمستخدم `carlos` من طلب إعادة التوجيه.
• **الحل:**
  1. سجل الدخول بـ (`wiener:peter`) واذهب لصفحة الحساب.
  2. التقط الطلب `/my-account?id=wiener` وأرسله إلى Burp Repeater.
  3. غيّر المعامل `id` من `wiener` إلى `carlos` وأرسل الطلب.
  4. افحص محتوى الاستجابة (Response Body) بالرغم من إرجاع كود `302 Found` ستجد الـ API key الخاص بـ `carlos`.
  5. انسخ الـ API key وقدمه في اللاب.

---

## Lab 8: User ID Controlled by Request Parameter with Password Disclosure
• **الهدف:** استخراج كلمة سر الأدمن وتسجيل الدخول بحسابه لمسح `carlos`.
• **الحل:**
  1. سجل الدخول بـ (`wiener:peter`) واذهب لصفحة الحساب.
  2. غيّر المعامل `id` في الرابط من `wiener` إلى `administrator`.
  3. افحص كود الصفحة لاستخراج كلمة السر المكتوبة داخل حقل كلمة السر المخفي.
  4. قم بتسجيل الخروج، ثم سجل الدخول بحساب `administrator` بكلمة السر المستخرجة.
  5. ادخل على لوحة الإدارة `/admin` واحذف `carlos`.

---

## Lab 9: Insecure Direct Object References (IDOR)
• **الهدف:** الوصول للـ Chat Logs واستخراج كلمة سر `carlos` وتسجيل الدخول بحسابه.
• **الحل:**
  1. افتح اللاب واضغط على تبويب "Live chat".
  2. أرسل رسالة ثم اضغط على "View transcript" لتنزيل السجل.
  3. لاحظ أن رابط التنزيل يستخدم أرقاماً متسلسلة (مثل `2.txt`).
  4. غيّر اسم الملف في الرابط إلى `1.txt` للوصول للسجل الأول.
  5. ابحث عن كلمة سر `carlos` الموجودة داخل النص، وسجل الدخول بحسابه.

---

## Lab 10: URL-based Access Control Can Be Circumvented
• **الهدف:** التجاوز باستخدام الهيدر `X-Original-URL` للوصول للأدمن ومسح `carlos`.
• **الحل:**
  1. حاول الوصول إلى `/admin` ولاحظ حظر الطلب من الواجهة الأمامية.
  2. أرسل الطلب إلى Burp Repeater وغيّر المسار الأساسي في الطلب إلى `/`.
  3. أضف الهيدر `X-Original-URL: /admin` للوصول لصفحة الإدارة.
  4. غيّر الهيدر إلى `X-Original-URL: /admin/delete` وأضف `?username=carlos` في السطر الأول للطلب.
  5. أرسل الطلب لإتمام حذف المستخدم `carlos`.

---

## Lab 11: Method-Based Access Control Can Be Circumvented
• **الهدف:** التجاوز عبر تغيير نوع الـ HTTP Method لترقية الحساب `wiener`.
• **الحل:**
  1. سجل الدخول بحساب الأدمن (`administrator:admin`) وادخل للوحة الإدارة.
  2. قم بفتح طلب ترقية المستخدم `carlos` وأرسل طلب الـ `POST` إلى Burp Repeater.
  3. سجل الدخول بالحساب العادي (`wiener:peter`) وانسخ قيمة الـ Session Cookie الخاصة به.
  4. استبدل كوكيز الأدمن بكوكيز المستخدم العادي داخل Burp Repeater.
  5. اضغط كليك يمين على الطلب واختر "Change request method" لتحويله من `POST` إلى `GET`.
  6. أرسل الطلب لتجاوز صلاحيات الوصول وترقية `wiener` إلى أدمن.

---

## Lab 12: Multi-step Process with No Access Control on One Step
• **الهدف:** استغلال ثغرة في خطوات عملية الترقية لترقية الحساب `wiener`.
• **الحل:**
  1. سجل الدخول بحساب الأدمن (`administrator:admin`) وافتح لوحة الإدارة.
  2. ابدأ عملية ترقية `carlos` وأكمل خطوة التأكيد الثانية (Confirmation step).
  3. التقط طلب التأكيد (`POST /admin-roles` مع المعاملات `action=upgrade` و `confirmed=true`) وأرسله للـ Repeater.
  4. سجل الدخول بالمستخدم العادي (`wiener:peter`) وانسخ الـ Session Cookie الخاصة به.
  5. استبدل كوكيز الأدمن بكوكيز `wiener` وغير اسم المستخدم من `carlos` إلى `wiener`.
  6. أرسل الطلب لتجاوز التحقق في خطوة التأكيد وترقية `wiener` إلى أدمن.

---

## Lab 13: Referer-Based Access Control
• **الهدف:** التجاوز باستخدام هيدر الـ `Referer` لترقية الحساب `wiener`.
• **الحل:**
  1. سجل الدخول بحساب الأدمن (`administrator:admin`) وافتح لوحة الإدارة.
  2. قم بترقية `carlos` والتقط الطلب وأرسله إلى Burp Repeater.
  3. سجل الدخول بالمستخدم العادي (`wiener:peter`).
  4. لاحظ أن طلب الترقية المباشر بدون هيدر `Referer` مناسب يتم رفضه.
  5. في Burp Repeater، ضع الـ Session Cookie الخاصة بالمستخدم `wiener` وغير اسم المستخدم إلى `wiener`.
  6. تأكد من وجود الهيدر `Referer: .../admin` داخل الطلب ثم أرسله لترقية `wiener` إلى أدمن.
  