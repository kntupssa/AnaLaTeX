🇮🇷 fa | [🇬🇧 en](./README.en.md)

# کنترل جریان برنامه و ورودی‌ها در LaTeX (بخش ۱)

بعد از اینکه یادگرفتیم چجوری می‌تونیم یک دستور یا محیط تعریف کنیم، ممکنه پرسشی صورت بگیره برامون: ما چطور می‌تونیم این دستورات و محیط‌ها رو منعطف‌تر کنیم تا بتونیم کدی پویا بنویسیم؟ در این آموزش به پاسخ این پرسش می‌پردازیم.

بذارید قبل از آغاز، دقیق‌تر واکاوی کنیم ببینیم منظور از منعطف‌تر و پویاتر شدن یعنی چی؟

کد زیر رو در نظر بگیرید:

```latex
\NewDocumentCommand{\kntupssa}{m}{
    \DoSth{m}
}
```
امّا قصد داریم به ورودی دستور یک لیست از کاراکترها رو بدیم که نیاز نباشه کاربر کد ما چندین متغییر رو به ورودی دستور تحویل بده، یا حتی در شرایطی کد ما باید جوری انعطاف داشته باشه که تعداد نامشخصی از کاراکتر هم گرفت کار کنه! در این شرایط دقیقاً لحظه‌ای هست که کنترل‌کننده‌ها به ما کمک فراوانی می‌کنن.

## کنترل‌کننده‌های جریان برنامه

این گونه از کنترل‌کننده‌ها در LaTeX شرط‌های منطقی رو پوشش می‌دن که تعدادشون کم و دارای محدودیت هستن؛ البته در سطح فعلی کم هستن در آموزش‌های بعدی می‌بینیم که این دستورات چقدر پیچیده و قابل کنترل خواهند شد.

#### داشتن یا نداشتن مقدار

در این گونه از کنترل‌کننده‌ها ما فقط می‌تونیم بررسی کنیم که: یک ورودی مقداری دارد یا نه؟
بیشترین استفاده‌شون در بررسی مقدار ورودی‌های اختیاری هست.

این گونه به صورت کلی دارای دستورات زیر هست:
```latex
% Type 1
\IfNoValueTF{<arg>}{<if true>}{<if false>}
\IfNoValueT{<arg>}{<if true>}
\IfNoValueF{<arg>}{<if false>}

% The reverse condition, Type 2
\IfValueTF{<arg>}{<if true>}{<if false>}
\IfValueT{<arg>}{<if true>}
\IfValueF{<arg>}{<if false>}
```
گونه‌ی اول بررسی می‌کنه که: اگر ورودی داده شده، arg، مقداری ندارد -> 
پس بلوک اول اجرا می‌شه، در غیر این‌صورت بلوک دوم اجرا می‌شه. گونه‌ی دوم هم همین کار رو انجام می‌ده اما با شرط برعکس: بررسی می‌کنه که ورودی داده شده مقداری داشته باشه.

*مثال*: دستوری طراحی می‌کنیم که اگر ورودی اختیاری مقداری نداشت یک کاری انجام بده در غیر این صورت کاری دیگه.
```latex
\NewDocumentCommand{\kntupssa}{o}{
    \IfNoValueTF{#1}{
        {\textit KNTU PSSA}
    } {
        {#1 KNTU PSSA}
    }
}
```
به این مورد دقت داشته باشید که زمانی بلوک «درست» این کد اجرا می‌شه که هیچ ورودی اختیاریی به دستور داده نشه! مثلاً:
```latex
\kntupssa
```
#### خالی بودن یا پر بودن ورودی
دستورات این گونه به بررسی خالی بودن یا پر بودن یک ورودی می‌پردازن، اول دستورات‌ش رو ببینیم و بعد یک مثال بزنیم:
```latex
\IfBlankTF{<arg>}{<if true>}{<if false>}
\IfBlankT{<arg>}{<if true>}
\IfBlankF{<arg>}{<if false>}
```
یک ورودی اختیاری می‌تونه داده بشه ولی خالی داده بشه، مثال زیر رو اجرا کنید و خودش کاملاً توضیح‌دهنده هست:
```latex
\NewDocumentCommand{\kntupssa}{o}{
    \IfNoValueTF{#1}{
        {\textit KNTU PSSA} % Actually, `o` has `-NoValue-` value here.
    } {
        \IfBlankTF{#1}{
            {The 'o' is blank}
        } {
            {#1 No blank 'o', KNTU PSSA}
        }
    }
}

% Check the result
\kntupssa[] % Blank
\kntupssa[ ] % Blank
\kntupssa % -NoValue-
\kntupssa[\texttt] % Has value
```

#### مقادیر منطقی «درست» و «نادرست»

این گونه از دستورات کنترل‌کننده، ورودی‌هایی رو بررسی می‌کنن که می‌تونن مقادیر درست و نادرست رو داشته باشن، ورودی‌ای مثل s که ستاره هست از این دست ورودی‌ها هست.

مقادیر درست و نادرست که در برنامه‌نویسی به صورت `True` و `False` باهاشون روبه‌رو می‌شیم، در LaTeX به صورت زیر هستن:
```latex
\BooleanTrue % = True
\BooleanFalse % = False
```
در [آموزش اول](../L1/README.md) ما از این شرط برای معرفی دستورات ستاره‌دار استفاده کردیم امّا دقیق مسئله رو باز نکردیم، اینجا کمی دقیق‌تر بهش نگاه می‌ندازیم:

```latex
\IfBooleanTF{<arg>}{<if true>}{<if false>}
\IfBooleanT{<arg>}{<if true>}
\IfBooleanF{<arg>}{<if false>}
```
ورودی این دستورات هم مثل دستورات بالاست، تنها تفاوتی که دارن در روش کارشون هست. این گونه می‌تونه برای ورودی‌های اجباری هم استفاده بشه:
```latex
\NewDocumentCommand{\kntupssa}{s m}{
    \IfBooleanTF{#1}{
        {Has Star #2}
    } {
        \IfBooleanT{#2}{
            {But the second argument is True}
        }
    }
}

% Check the result
\kntupssa*{Second Argument}
\kntupssa{\BooleanTrue} % Second block of `IfBooleanTF` will be executed
```

🌱 تا به این جا یادگرفتیم که چجوری می‌تونیم جریان تولید اسنادمون رو در LaTeX در دست خودمون بگیریم و تغییرش بدیم،‌ در بخش دوم به این موضوع می‌پردازیم که: چجوری قبل از اینکه ورودی‌ها رو استفاده کنیم، تغییرشون بدیم و کنترل‌شون کنیم، با ما همراه باشید!

---
نویسنده: [ابوالفضل محمدی](https://github.com/abolfazlmohammadi12)، [احسان آرمیده](https://github.com/EhsanAramide)

راه‌های ارتباطی با انجمن علمی:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)