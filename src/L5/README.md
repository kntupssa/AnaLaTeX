🇮🇷 fa | [🇬🇧 en](./README.en.md)

# ساخت بسته های شخصی خودمون

تاالان یاد گرفتیم چطوری دستور ها و محیط های شخصی خودمون رو بسازیم و کنترل کنیم اما اگر تعدادشون زیاد باشه بخش preambe یا مقدمه فایل LaTeX مون رو خیلی شلوغ میکنه از طرفی اگه توی یک فایل LaTeX دیگه بهشون نیاز داشته باشیم بازم باید از اول بشینیم و بنویسیمشون؛ اینجاهاس که ساخت بسته میتونه ایده خوبی باشه.

با ساختن بسته خودمون و قرار دادن دستورات و محیط های جدیدی که ساختیم درونش، میتونیم هروقت که بخوایم توی سند LaTeX مون به کمک دستور `{<package_name>}usepackage\` واردش کنیم و ازش استفاده کنیم.

## قاعده درست نامگذاری

معمولا دستورات داخلی LaTeX که کاربر ها قرار نیست توی اسنادشون از اونها استفاده کنن با حرف @ نامگذاری میشن. توی تعریف بسته ها محدودیتی برای استفاده از @ در نامگذاری دستورات نداریم ولی اگر در اسنادمون بخوایم از @ استفاده کنیم با ارور رو به رو میشیم. این برای اینه که دستور های داخلی LaTeX و بسته ها رو دچار اختلال نکنیم ولی اگه بازم اصرار داشته باشیم از @ برای نامگذاری دستورات در سند استفاده کنیم، میتونیم از دستورات `makeatletter\` قبل از استفاده و `makeatother\` بعد از استفاده از @ بهره ببریم.

```latex
\makeatletter
\NewDocumentcommand{\@pssa}{}{my name has @}
\makeatother
```

درحالت کلی یک قاعده استاندارد برای نامگذرای دستورات مورد استفاده درون بسته که در سند قرار نیست استفاده بشن `(<command_name>)@(<package_name>)\`  هست ولی دستوراتی که قراره توی سند استفاده بشن میتونن به همون فرم معمولی نامگذاری بشن.

## ساخت بسته

برای ساختن یک پکیج، اولین قدم اینه که یک فایل `package_name>.sty>` بسازیم که sty. بودن پسوند الزامیه و اسم فایل باید همون اسم بسته باشه.

برای اینکه بسته در فایل سندمون موقع وارد کردن شناسایی بشه باید اون رو در همون پوشه ای که فایل سند هست قرار بدیم (البته میشه در مکانی هم قرارش داد که برای تمام اسناد قابل شناسایی باشه ولی در سیستم عامل ها و توزیع های مختلف TeX متفاوت هست).

 حالا بریم ببینیم داخل این فایل چطوری میتونیم بسته مون رو تعریف کنیم.

## بدنه بسته

 در اینجا اول یک مثال از ساختار کلی بسته میزنیم و بعدش بخش های مختلف رو یکی یکی توضیح میدیم. به مثال پایین نگاه کنید

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{<Package_name>}[1404/05/29 v0.1 a sample latex package]

\RequirePackage{kntu}
\RequirePackage{pssa}
...

defining initial value for commands or variables if it needed

\DeclareOption{option}{definition}

\ExecuteOptions{<option_name>}

\ProcessOptions\relax

defining our commands and envs and whatever we want

\endinput
```
این ساختار کلی برای تعریف بسته مون هست اما ببینیم هرکدوم ازین دستورات چه معنی ای میده.

دستور `NeedsTeXFormat{LaTeX2e}\` مشخص میکنه که حداقل چه نسخه ای از TeX یا LaTeX برای اجرای بسته کافیه (روی همون LaTeX2e بزارید البته بعدا در مورد LaTeX3 هم صحبت میکنیم).

دستور `ProvidesPackage{<Package_name>}[version]\` بسته را معرفی میکنه. توی ورودی اختیاری version معمولا طبق همون شکلی که در مثال نوشته شده اول تاریخ بعدش نسخه و در نهایت یک توضیح کوتاه راجع به بسته مینویسیم.

دستور `{...}RequirePackage\` بسته های مورد استفاده مون رو فرا میخونه و دقیقا معادل usepackage در اسناد معمولی هست.

دستور `DeclareOption{option}{definition}\` تعریف میکنه وقتی کاربر موقع وارد کردن بسته به ورودی اختیاری option مقدار داد چه کدی اجرا بشه.

دستور `ExecuteOptions{option}\` گزینه پیش فرض وقتی که کاربر به option مقدار نداده رو تعیین میکنه.

دستور `ProcessOptions\` ورودی های اختیاری رو چک و اجرا میکنه و `relax\` هم درواقع کار خاصی نمیکنه فقط برای بستن امن دستور پردازش ورودی هاست. بعد از اون دستور ها و محیط های خودمون رو مینویسیم.

دستور `endinput\` حتما در پایان کد باید وجود داشته باشه و بعد از اون چیزی خونده نمیشه.

به این ترتیب میتونیم بسته خودمون رو بسازیم و ازش توی اسنادمون بهره ببریم. 

### مثال

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{mynote}[2025/08/20 0.1v Simple colorful notes]

% packages that we used
\RequirePackage{xcolor}

% defining options
\DeclareOption{red}{\def\mynot@notecolor{red}}
\DeclareOption{blue}{\def\mynot@notecolor{blue}}
\DeclareOption{green}{\def\mynot@notecolor{green}}

% if user didn't give the option, red will be executed
\ExecuteOptions{red}

% completing the option creation process
\ProcessOptions\relax

% creating a new command to write colored text
\NewDocumentCommand{\mycolorednote}{m}{\textcolor{\mynot@notecolor}{\textbf{#1}}}

\endinput
```

توی این مثال اگر کاربر موقع فراخونی بسته یکی از رنگ هارو به عنوان ورودی اختیاری بده، به کمک دستور `def\` یک متغیر با مقدار اون رنگ ایجاد میشه و توی دستور جدیدی که ساختیم استفاده میشه. مثال ساده ای هست ولی برای فهمیدن نحوه کار دستور ها توی بسته کاربردیه.

پس ما الان یاد گرفتیم چطور می‌شه یک بسته شخصی در LaTeX ساخت؛ حالا با همین ابزار ساده، می‌تونیم ایده‌ها و نیازهای شخصی‌مون رو به شکل حرفه‌ای به LaTeX اضافه کنیم و یک جعبه‌ابزار اختصاصی برای خودمون بسازیم. به این ترتیب، به جای تکرار دستورات در هر پروژه، یک‌بار بسته می‌نویسیم و بعد بارها ازش استفاده می‌کنیم.


نویسنده: [سید امیررضا طالقانی](https://github.com/AmirRezaTaleghani)

راه‌های ارتباطی با انجمن علمی:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)

