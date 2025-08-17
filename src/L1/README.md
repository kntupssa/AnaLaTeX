🇮🇷 fa | [🇬🇧 en](./README.en.md)

# ساختن دستورهای جدید در LaTeX

سکوی نرم‌افزاری «LaTeX» به کاربر خودش اجازه می‌ده تا از نهایت شخصی‌سازی بهره‌مند بشه و بتونه کار خودش رو به روشی خودکار و دلخواه پیش ببره. یکی از توانایی‌های خاص و جالب LaTeX در این زمینه، طراحی و ساختن دستورات شخصی هست.

چرا ما نیاز داریم که از چنین ویژگی بهره‌مند بشیم زمانی که داریم اسناد خودمون رو با LaTeX می‌نویسیم؟ شاید این سوال برای خیلی‌ها پیش بیاد ولی پاسخ‌ش می‌تونه کوتاه و سرراست باشه: زمانی که دستورات پیش‌فرض LaTeX برای کار شما مناسب نباشند یا بخوایید کاری متفاوت‌تر از کاربری عادی انجام بدید.

در سکوی LaTeX به دستوراتی که تعریف میشن Macro گفته می‌شه (چه کاربر اون‌ها رو تعریف کرده باشه چه سازندگان LaTeX). قبل از اینکه LaTeX به صورت پیش‌فرض این موارد رو پشتیبانی کنه، این کدها به صورت یک بسته عرضه می‌شدن، با نام xparse، که در سال ۲۰۲۰ در هسته‌ی اصلی LaTeX جایگذاری شد.

## چطور دستور تعریف کنیم؟

برای این کار شما می‌تونید از ۴ دستور زیر استفاده کنید:
```latex
\NewDocumentCommand{\<name>}{<argspec>}{<definition>} % First
\RenewDocumentCommand{\<name>}{<argspec>}{<definition>} % Second
\ProvideDocumentCommand{\<name>}{<argspec>}{<definition>} % Third
\DeclareDocumentCommand{\<name>}{<argspec>}{<definition>} % Fourth
```
حالا می‌پردازیم به جزییات تک به تک این دستورات:
| کاربرد | اگر دستور قبلاً نبود | اگر دستور قبلاً بود | دستور |
| -   | -   | -    | - |
| فقط تعریف دستور جدید | تعریف میکنه | خطا می‌ده | اول |
| تغییر رفتار دستور موجود | خطا می‌ده | بازتعریف می‌کنه | دوم |
| تعریف امن، بدون دستکاری قبلی | تعریف می‌کنه | تغییری نمی‌ده | سوم |
| (باز)تعریف دستور | تعریف می‌کنه | بازتعریف می‌کنه | چهارم |

### تعریف ورودی‌های دستور

در LaTeX دستورها می‌تونن ورودی بگیرن، اما چجوری باید تعریف‌شون کرد؟ اینجا می‌رسیم به بخش دوم، یعنی argspec که به خلاصه‌شده‌ی Argument Specification هست.

تعریف argspec به صورت رشته‌ای از حروف‌هاست، که هر کدوم از این حروف برای LaTeX دارای یک معنای خاص هست که ما به ۴تا از اون‌ها در آموزش می‌پردازیم. در حالت کلی ورودی‌ها یا اجباری هستند یا اختیاری:

- اجباری
    1. ورودی m : معمولی و اجباری. یعنی چیزی که حتماً باید داخل آکولاد { } نوشته بشه.

- اختیاری
    1. ورودی o : اختیاری داخل []. اگر نوشته نشه، مقدار ویژه‌ای به نام  -NoValue-  برمی‌گردوند.
    2. ورودی O{default} : مثل o، ولی اگر چیزی نوشته نشه، مقدار پیش‌فرضی که تعیین کردیم جایگزین می‌شه؛ default.
    3. ورودی s : ستاره‌ی اختیاری. اگر ستاره باشه، مقدار True می‌ده و اگر نباشه False.

### چند مثال

حالا برای اینکه برامون جا بیوفته چجوری یک دستور بسازیم، چندتا مثال رو با هم بررسی می‌کنیم:

*مثال اول*: برای شروع کار بیایید یک دستور ساده بسازیم، بدون ورودی فقط برامون یک چیزی برگردونه.

```latex
\NewDocumentCommand{\kntupssa}{}{ %
    K. N. Toosi University \emph{Physics Students Scientific Association}
} %
```

*مثال دوم*: می‌خواییم یک دستور بسازیم که برامون «کت» رو در نمادگذاری دیراک بسازه.

```latex
\NewDocumentCommad{\ket}{m}{ %
    \left|#1\right.\rangle
} %

% Check the result
$\ket{v}$
```
توجه داشته باشید که شماره‌ی هر متغییر بر اساس ترتیب تعریف در argspec تعیین می‌شه و برای استفاده ازش باید یک هشتگ در ابتدا قرار بدید.

*مثال سوم*: دستوری می‌سازیم که ضرب داخلی در نمادگذاری دیراک رو تعریف می‌کنه و اگر مقداری برای یکی از بردارها قرار ندیم، بجاش علامت سوال قرار می‌گیره.

```latex
\NewDocumentCommand{\innerproduct}{O{?} O{?}}{ %
    \langle#1|#2\rangle
} %

% Check the result
$\innerproduct[\psi]$
$\innerproduct[\psi][\phi]$
$\innerproduct$
```

*مثال چهارم*: ضرب داخلی رو به در دو نمادگذاری دیراک و گیبس نشان دهیم.

```latex
\NewDocumentCommand{\innerproduct}{s O{?} O{?}}{ %
    \IfBooleanTF {#1} { % if it's stared
        \vec{#2}.\vec{#3} % Gibbs Notation
    } { % if it isn't stared
        \langle#2|#3\rangle % Dirac Notation
    }
} %

% Check the result
$\innerproduct[v][w]$
$\innerproduct*[v][w]$
$\innerproduct*$
$\innerproduct$
```
دستور کنترلی `IfBooleanTF` در واقع نشان دهنده‌ی یک شرطه «اگر متغییر مورد نظر صحیح بود بلوک اول اجرا بشه در غیر این صورت بلوک دوم».

خیلی ممنون از همراهی شما!
منتظر ایده‌ها و سوالات شما هستیم، خیلی خوشحال می‌شیم اگر شما هم علاقه دارید تا چیزهایی از دنیای LaTeX به بقیه یاد بدید به ما بپوندید و همکاری کنید.

🌱 خب تا اینجا ممکنه سوالات دیگری هم براتون ایجاد شده باشه مثل چجوری یک محیط در LaTeX بسازیم یا چجوری جریان تولید سندمون رو چجوری در دست بگیریم و بتونیم همچون یک زبان برنامه‌نویسی بر روی سکوی LaTeX کد بنویسیم؟ در آموزش‌های بعدی بهشون خواهیم پرداخت!

---
نویسندگان: [امیررضا طالقانی](https://github.com/AmirRezaTaleghani)، [احسان آرمیده](https://github.com/EhsanAramide)

راه‌های ارتباطی با انجمن علمی:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)