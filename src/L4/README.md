🇮🇷 fa | [🇬🇧 en](./README.en.md)

# کنترل جریان برنامه و ورودی‌ها در LaTeX (بخش ۲)

در ادامه‌ی [قسمت سوم آموزش](../L3/README.md)، در این آموزش می‌خواییم یادبگیریم که چطور می‌تونیم با کنترل‌کننده‌های ورودی کار کنیم و قبل از اینکه اصلاً از ورودی استفاده کنیم، اون رو تغییر بدیم.

چیزی که در آموزش قبل باهاش آشنا شدیم مقدمه‌ای بود به برنامه‌نویسی در LaTeX، امّا داستان ما و LaTeX می‌تونه پیچیده‌تر و پویاتر بشه! قبل از آغاز این آموزش، بهتون پیشنهاد می‌کنیم تا به ابتدای قسمت سوم مراجعه کنید تا هم مثال براتون یادآوری بشه و هم هدف این دو قسمت.

## پردازشگرهای ورودی (Argument Processors)

پردازشگرهای ورودی یکسری دستور هستن که **بعد** از گرفته شدن ورودی و **قبل** از داده شدن اون‌ها به کد، روی ورودی‌ها عملیات‌هایی انجام می‌دن که موجب تغییر در ورودی‌ها و پویاسازی کد می‌شه.

برای اینکه توضیحات بهتر قابل درک‌تر بشن بیایید به این کد نگاهی بندازیم:
```latex
\NewDocumentCommand{\<cmd>}{<arg-spec>}{<code>}
```
این، شماتیک تعریف یک دستور هست؛ تا آخر این بخش گوشه چشمی بهش داشته باشید. در واقع فرایند کاری پردازشگرها از عمق LaTeX به این صورت دیده می‌شه:

**مرحله‌ی اول**: LaTeX ورودی‌های داده شده به دستور `cmd` طبق تعریف `arg-spec` رو جمع‌آوری می‌کنه.

**مرحله‌ی دوم**: این مقادیر داده شده، در صورت تعریف پردازشگر ورودی، مستقیم به بخش `code` وارد نمی‌شن، بلکه به پردازشگر داده می‌شن تا اون‌ها رو تغییر بده و اون‌ها رو پردازش کنه.

**مرحله‌ی سوم**: حالا مقادیری که پردازش شدن و تغییر پیدا کردن به بخش `code` ورود پیدا می‌کنن.

در رابطه با پردازشگرها، چند نکته وجود داره:

**نکته‌ی اول**: پردازشگرها برای استفاده باید به صورت زیر در کد قرار بگیرن:
```latex
\NewDocumentCommand{\<cmd>}{o m >{\<processor-name>} m}
```
و باید در نظر داشت که همیشه روی ورودی بعد از خودشون کار انجام می‌دن، مثلاً در این نمونه روی ورودی اجباری آخر کار انجام می‌شه.

**نکته‌ی دوم**: ورودی‌ها از راست به چپ به اولویت اجرا می‌شن:
```latex
% First, `processor-1` will be applied on `m`, then, `processor-2` will be applied on the output of the `processor-1`.
>{\<processor-2>} >{\<processor-1>} m
```

**نکته‌ی سوم**: پردازشگرها روی مقادیر ورودی داده شده از کاربر و مقادیر پیش‌فرض عمل می‌کنند، ولی روی مقدار خاص `-NoValue-` عمل نمی‌کنند.

## بررسی پردازشگرهای ورودی پرکاربرد

حالا که با کلیت پردازشگرها آشنا شدیم، بهتره که کار با چند پردازشگر پرکاربرد رو یادبگیریم و در قالب چند مثال ترفندهای کار باهاشون رو پیدا کنیم.

### پردازشگر ‍‍`SplitArgument`
```latex
\SplitArgument{<number>}{<token(s)>}
```
یک ورودی رو می‌گیره و می‌گرده دنبال `<number>`تا کاراکتر که داخل `<token(s)>` تعریف شده و ورودی رو از همون بخش قطع می‌کنه، این فرایند رو ادامه می‌ده تا جایی که یک لیست متشکل از `number + 1`تا عضو برمی‌گردونه. برای نمونه کد زیر رو اجرا کنید:
```latex
\NewDocumentCommand{\splitter}{ >{\SplitArgument{2}{,}} m }{
    {\textit #1}
}

% Check the result
\splitter{Ehsan, Abolfazl}
```
دو نکته در این دستور پنهانه:
۱) شما یک مقدار `-NoValue-` می‌گیرید، چون فقط یک کاما وجود داره ولی شما به پردازشگر گفتید تا دو کاما برش بده و 
۲) می‌بینید که فقط عضو اول ایتالیک شده، این موضوع به این خاطر رخ می‌ده که بعد از برش داده شدن چیزی که LaTeX جلوی دستور قرار میده این هست:
```latex
{\textit {Ehsan}{Abolfazl}}
```
و چون این دستور یک ورودی می‌گیره دادن دو ورودی به اون بی‌فایده‌ست.

### پردازشگر `SplitList`
```latex
\SplitList{<token(s)>}
```
ورودی رو تحویل می‌گیره و در هر جایی که به `<token(s)>` برخورد کرد، برشی روی ورودی انجام می‌ده.

به این مثال توجه کنید:

```latex
\NewDocumentCommand{\ListGen}{ O{,} >{\SplitList{#1}} m }{
    \DoSomething #2
}
```
نکته‌ای که اول از همه در این کد می‌شه یاد داد این هست که: پردازشگر‌ها می‌تونن از ورودی‌های قبل از خودشون به عنوان ورودی استفاده کنن.

نکته‌ی دوم رو می‌تونید در کد زیر مشاهده کنید؛ تعداد برش‌ها برای این پردازشگر مهم نیست و به هر تعدادی برش می‌ده:
```latex
% What you pass
{Ehsan, Abolfazl, Amirreza}

% What you get if the delimiter is `,`
{Ehsan}{Abolfazl}{Amirreza}
```

### پردازشگر `ProcessList`
```latex
\ProcessList{<list>}{<function>}
```
بعد از اینکه پردازشگر `SplitList` ورودی رو برش داد و به لیستی از اعضا تبدیل‌ش کرد، این دستور روی این اعضا پیمایش می‌کنه و تک به تک اون‌ها رو به عنوان ورودی به `<function>` می‌فرسته تا عملی روی اون‌ها صورت بگیره.

دیگه باید براتون جا افتاده باشه که چجوری وقتی به دستور `documentclass` مقدارهای اختیاری می‌دیم، اون‌ها پردازش می‌کنه خیلی ساده فکر کنیم می‌تونیم بگیم به صورت زیر:
```latex
\NewDocumentCommand{\SomeCommand}{ >{\SplitList{,}} o m }{
    \ProcessList{#1}{\ApplyOptions}
    \ApplyVar{#2}
}

% This can be used as below:
\SomeCommand[opt1, opt2, opt3]{parameter}
```

🌱 حالا که یادگرفتیم چجوری دستور و محیط شخصی بسازیم و یادگرفتیم چجوری داخل LaTeX به سبکی ساده و نچندان پیجیده، کمی برنامه‌نویسی کنیم، یک پرسش میاد در ذهن‌مون: اگر بخوام تمام ساخته‌هام رو در پروژه‌های دیگه‌ای هم استفاده کنم، باید همینطور از این پروژه به اون پروژه انتقال‌شون بدم؟ پاسخ ما در آموزش بعدی بهش خیر هست، همراه ما باشید!

---
نویسنده: [احسان آرمیده](https://github.com/EhsanAramide)، [ابوالفضل محمدی](https://github.com/abolfazlmohammadi12)

راه‌های ارتباطی با انجمن علمی:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)