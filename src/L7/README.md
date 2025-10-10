🇮🇷 fa | [🇬🇧 en](./README.en.md)

# هایپرلینک‌ها در LaTeX (بخش ۱)

---

در اسناد PDF چیزی که بسیار کاربردی و جذابه، هایپرلینک‌ها هستن؛ لینک‌هایی که ما را به بخش دیگری از سند یا حتی به یک وب‌سایت هدایت می‌کنن.  
در LaTeX بسته‌ای به نام `hyperref` وجود داره که این کار رو خیلی راحت می‌کنه.

---

## 1️⃣ نصب و استفاده

برای استفاده، کافیه در بخش پیش‌گفتار (preamble) بنویسیم:

```latex
\usepackage{hyperref}
```
نکته: این بسته باید همیشه آخرین بسته‌ای باشد که بارگذاری می‌کنید، چون بسیاری از دستورات LaTeX را بازنویسی می‌کند.

## 2️⃣ ارجاع‌ها و لینک‌های خودکار
بسته‌ی hyperref باعث می‌شود بسیاری از ارجاع‌ها به صورت هایپرلینک دربیایند.
به عنوان مثال:

```latex
\usepackage{hyperref}
\section{هایپرلینک‌ها}\label{hyperlinks}

The reference to this section
now looks like that:\footnote{
Footnotes are also
made into hyperlinks.}

Section~\ref{hyperlinks} is
on page~\pageref{hyperlinks}.

The hyperref bibliographic
entry is~\cite{pack:hyperref}.
```
چه اتفاقی می‌افتد؟

ارجاع به بخش‌ها: دستور \ref{} حالا به صورت لینک در PDF ظاهر می‌شود.
مثلا: «بخش 5.3.1 در صفحه 140».

ارجاع به صفحات: دستور \pageref{} نیز لینک‌دار می‌شود.

پاورقی‌ها (Footnotes): حتی پاورقی‌ها هم تبدیل به لینک می‌شوند.

ارجاع‌های کتاب‌نامه‌ای: دستور \cite{} به صورت لینک فعال می‌شود.

## 3️⃣ ظاهر لینک‌ها
به‌طور پیش‌فرض، لینک‌ها در فایل PDF با یک کادر قرمز مشخص می‌شوند. این کادر فقط در صفحه‌نمایش (on-screen) دیده می‌شود و هنگام چاپ ظاهر نمی‌شود.
اما چون معمولاً ظاهر زیبایی ندارد، توصیه می‌شود از گزینه‌ی colorlinks استفاده کنید:
```latex
\usepackage[colorlinks=true,linkcolor=blue,urlcolor=cyan,citecolor=red]{hyperref}
```
با این تنظیمات:

لینک‌های داخلی (مثل \ref) آبی می‌شوند.

لینک‌های وب (\url, \href) فیروزه‌ای نمایش داده می‌شوند.

ارجاع‌های کتاب‌نامه (\cite) قرمز خواهند بود.
## 4️⃣ تایپ URL و لینک‌های دلخواه
دستور \href{URL}{text} برای نمایش یک متن دلخواه به جای آدرس:
```latex
\href{https://www.nasa.gov/}{NASA}
%NASA است که به سایت مورد نظر ارجاع داده می‌شودخروجی لینکی با متن 
\url{https://www.ctan.org/}
% نمایش یک URL مستقیم
\hyperref[hyperlinks]{Hyperlinks section}
%لینک داخل سند با متن متفاوت
```
## 5️⃣ دستورات \autoref و \autopageref
استفاده زیاد از ارجاعات مثل «بخش ۵.۳.۱ در صفحه ۱۴۰» باعث شده که hyperref دو دستور جدید معرفی کند:

\autoref{label} → مشابه \ref است اما نوع شمارنده (بخش، فصل، شکل و غیره) را هم نمایش می‌دهد.

\autopageref{label} → مشابه \pageref است اما متن اضافه مثل "page" یا "صفحه" تولید می‌کند.

مثال:
```latex
\autoref{hyperlinks} در \autoref{specialities} روی \autopageref{hyperlinks}.
% خروجی: «زیربخش ۵.۳.۱ در فصل ۵ روی صفحه ۱۴۰».
```
## 6️⃣ کنترل حاشیه لینک‌ها (pdfborder)
کلید pdfborder سه عدد می‌گیرد: شعاع گوشه افقی، شعاع گوشه عمودی، ضخامت حاشیه
```latex
\hypersetup{pdfborder = {h v w}}
```
سه مقدار می‌گیره:

h → شعاع افقی گوشه کادر (برای گرد کردن گوشه‌ها)

v → شعاع عمودی گوشه کادر

w → ضخامت کادر (اگر 0 باشه، کادری نمایش داده نمی‌شه)
مثال‌ها:
```latex
\hypersetup{pdfborder = 0 0 1}   % کادر باریک ساده
\hypersetup{pdfborder = 10 10 3} % کادر ضخیم با گوشه‌های گرد
\hypersetup{pdfborder = 0 0 0}   % بدون کادر
```
## 7️⃣ رنگ حاشیه لینک‌ها
زمانی که بخواهید برای هر نوع لینک رنگ متفاوتی تعیین کنید، می‌توانید از سه کلید زیر استفاده کنید؛ البته به شرطی که بسته‌ی xcolor قبلاً بارگذاری شده باشد.

urlbordercolor → رنگ حاشیه لینک‌های اینترنتی (\url, \href)

citebordercolor → رنگ حاشیه رفرنس‌های کتابخانه‌ای (\cite)

linkbordercolor → رنگ حاشیه ارجاعات داخلی (\ref, \autoref, ...)
مثال:
```latex
\hypersetup{
  pdfborder       = 0 0 2,
  urlbordercolor  = violet, % لینک‌های وب بنفش
  citebordercolor = pink,   % رفرنس‌های کتاب صورتی
  linkbordercolor = teal,   % ارجاعات داخلی سبز
}

```
در این بخش با بسته‌ی hyperref در LaTeX آشنا شدیم و یاد گرفتیم چگونه لینک‌های داخلی 📑، URLها 🌐، پاورقی‌ها 📝 و ارجاعات کتاب‌نامه‌ای 📚 را به صورت هایپرلینک فعال کنیم. همچنین توانستیم رنگ متن 🎨 و حاشیه لینک‌ها 🖌️ را تنظیم کرده و ظاهر PDF را زیباتر و قابل‌استفاده‌تر کنیم.

در بخش بعدی  قصد داریم عمیق‌تر شویم و دستورات پیشرفته و امکانات بیشتری از این بسته را بررسی کنیم.
---
نویسنده: [ابوالفضل محمدی](https://github.com/abolfazlmohammadi12)

راه‌های ارتباطی با انجمن علمی:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)