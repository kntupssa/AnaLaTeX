🇮🇷 fa | [🇬🇧 en](./README.en.md)

# ساختن محیط‌های جدید در  LaTeX

همون‌طور که در [آموزش پیشین](../L1/README.md) یادگرفتیم، دستورات شخصی‌سازی شده در LaTeX ما رو توانا می‌کنن تا بتونیم فرایند ساخت اسناد رو برای خودمون شخصی‌سازی کنیم و از استفاده‌ی معیار بقیه از LaTeX فاصله بگیریم. این دفعه در این آموزش می‌خواییم یادبگیریم چجوری محیط شخصی‌سازی شده‌ی خودمون رو بسازیم.

همگی وقتی برای اولین‌بار LaTeX نصب می‌کنید با چنین چیزی امکان داره رو‌به‌رو بشید:

```latex
\documentclass{article}

% This is called `document environment`
\begin{document} 
\end{document}
```
در واقع اون چیزی تموم این مدت داخل‌ش متن اسنادمون رو می‌نوشتیم، نام‌ش محیط document بود. حالا شخصی‌سازی چنین مواردی چجوری می‌تونه به ما کمک‌کننده باشه؟ اول از همه بیایید یادبگیریم چطور یک محیط شخصی برای خودمون بسازیم و بعد با مثال‌های گوناگون کاربرد اون‌ها رو در عمل ببینیم.

## چطور محیط شخصی‌سازی شده بسازیم؟

درست مثل آموزش پیشین، ما از همون دستورات [با کمی تفاوت] استفاده می‌کنیم برای ساختن یک محیط؛ کاربرد دستورات تفاوتی نداره فقط نام‌شون متفاوته:

```latex
\NewDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % First
\RenewDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % Second
\ProvideDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % Third
\DeclareDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % Fourth
```
تفاوت دستورهای سازنده‌ی محیط با سازنده‌ی دستور موارد زیر هست:

- دو بخش متفاوت برای دستور آغاز و دستور پایان محیط
- پیش از نام محیط کاراکتر \ وجود ندارد
- در بخش Argument Specification شما می‌تونید متن قرار گرفته درون محیط رو به عنوان ورودی بگیرید؛ از حرف b باید استفاده کنید.

در موارد دیگه مثل هم دیگه رفتار می‌کنن.

حالا برای درک توانایی که LaTeX بهمون داده به طرح چند مثال می‌پردازیم.

## چند مثال

*مثال اول*: محیطی تعریف کنید که متن داخل محیط رو بین `kntupssa` قرار بده.

```latex
\NewDocumentEnvironment{kntupssa}{}
    { % begin code
        $KNTUPSSA \rightarrow$
    }
    { % end code
        $\leftarrow KNTUPSSA$
    }

% Check the result
\begin{kntupssa}
We just created a Tips and Tricks tutorial about LaTeX, AnaLaTeX.
\end{kntupssa}
```

*مثال دوم*: یک محیط تعریف می‌کنیم ویژه‌ی فرمول‌های ریاضیاتی و در پایان محیط یک متن دلخواه قرار می‌دیم.

```latex
\NewDocumentEnvironment{MathEnv}{+m +b}
    { % begin code
        \[
        #2
        \rightarrow #1 \leftarrow
        \]
    }
    { % end code
    }

% Check the result
\begin{MathEnv}{KNTUPSSA}
    i\hbar \frac{\partial \Psi}{\partial t} = \hat{H} \Psi
\end{MathEnv}
```
این مثال دو نکته‌ی آموزنده داره:

نکته‌ی اول: نشان مثبت پشت ورودی‌ها برای این هست که به LaTeX بفهمونیم متن ورودی می‌تونه یک متن بلند بالا باشه. (در واقع به منظور بهینه‌سازی این ویژگی قرار گرفته شده.)

نکته‌ی دوم: وقتی تمام متن داخل محیط رو به عنوان ورودی می‌گیریم، بهتره که اول از همه در argspec اون رو آخر از همه قرار بدید و در جایگاه دوم، وقتی متن رو می‌گیرید بهتره که یکی از بخش‌های (فرقی نداره یا بخش آغازین یا بخش پایانی) خالی باشه، چون باعث می‌شه کد نوشته شده منسجم و تمیز باشه.

*مثال سوم*: محیطی بسازید که در وسط صفحه متنی دلخواه رو قرار بده و به صورت پیش‌فرض وضعیت قلم رو ایتالیک کنه ولی قابلیت تغییر وضعیت قلم رو داشته باشه.

```latex
\NewDocumentEnvironment{reshape}{O{\itshape} +b}
    { % begin code
        \center{#1{#2}}
    }
    { % end code
    }

% Check the result
\begin{reshape}[\textbf]
K. N. Toosi University Physics Students Scientific Association
\end{reshape}
```

*مثال چهارم*: در این مثال می‌خواییم یک محیط بسازیم با که قابلیت ستاره‌دار شدن رو داشته باشه ولی نکته‌ای آموزنده در اون قرار داره.

```latex
\NewDocumentEnvironment{ItBfReshape}{s +b}
    { % begin code
        \IfBooleanTF {#1} {
            \textit{#2}
        } {
            \textbf{#2}
        }
    }
    { % end code
    }

\begin{ItBfReshape}
    Without star
\end{ItBfReshape}


\begin{ItBfReshape}*
    With star
\end{ItBfReshape}
```
برای استفاده از نوع ورودی s در ساختن محیط‌هامون باید به این مورد توجه داشته باشیم که ستاره در جلوی آکولاد قرار می‌گیره ولی می‌تونیم به روش دیگری هم محیط‌هامون رو ستاره‌دار کنیم:

```latex
\NewDocumentEnvironment{ItBfReshape}{+b}{...}{...}
\NewDocumentEnvironment{ItBfReshape*}{+b}{...}{...}

\begin{ItBfReshape}
    Without star
\end{ItBfReshape}

\begin{ItBfReshape*}
    With star
\end{ItBfReshape*}
```

در این روش تنها کاری که باید انجام بدیم تعریف دو محیط متفاوت هست که یکی ستاره‌داره و دیگری نداره، این استاندارد در کدنویسی باعث روان‌تر شدن کد و یکسان‌سازی بیشتر با دستورات ستاره‌دار می‌شه.

🌱 ما طی دو آموزش گذشته یادگرفتیم چجوری دستورها و محیط‌های شخصی‌سازی شده در LaTeX بسازیم، اما یک چیز می‌تونه به عنوان چاشنی به این داستان اضافه بشه؛ هدایت کردن جریان تولید سندمون در LaTeX. منتظر آموزش‌های بعدی ما باشید، بازخوردهای شما می‌تونه برای ما مفید و کاربردی باشه خیلی خوشحال می‌شیم اگر موردی بود حتماً بهمون بگید.

---
نویسنده: [احسان آرمیده](https://github.com/EhsanAramide)

راه‌های ارتباطی با انجمن علمی:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)