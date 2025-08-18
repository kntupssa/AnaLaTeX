[🇮🇷 fa](./README.md) | 🇬🇧 en

# Program Flow and Inputs Control in LaTeX (Part 1)

After learning how we can define a command or environment, a new question may arise: how can we make these commands and environments more flexible so that we can write dynamic code? In this tutorial, we address this question.

Before starting, let’s analyze more precisely what we mean by making the code more flexible and dynamic.

Consider the following code:

```latex
\NewDocumentCommand{\kntupssa}{m}{
    \DoSth{m}
}
```
But what if we want to pass a list of characters to the command input, so that the user doesn’t have to provide multiple variables separately, or even in some cases, our code should be able to handle an unspecified number of characters? This is exactly when **controllers** come to our aid.

## Program Flow Controllers

These types of controllers in LaTeX cover logical conditions, which are few in number and somewhat limited; of course, at this stage they are simple, but in later tutorials we will see how complex and controllable they can become.

#### Having or Not Having a Value

In this type of controllers, we can only check whether an input has a value or not. They are mostly used for checking optional arguments.

Generally, they include the following commands:
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
The first type checks whether the given input `arg` has no value →  
if so, the first block is executed; otherwise, the second block.  
The second type does the same but with the opposite condition: it checks if the input has a value.

*Example*: We design a command such that if the optional argument has no value, it performs one action; otherwise, another.
```latex
\NewDocumentCommand{\kntupssa}{o}{
    \IfNoValueTF{#1}{
        {\textit KNTU PSSA}
    } {
        {#1 KNTU PSSA}
    }
}
```
Note that the "true" block executes when no optional input is given to the command! For example:
```latex
\kntupssa
```

#### Input Being Blank or Non-Blank

These commands check whether an input is blank or not. Let’s first see the commands and then an example:
```latex
\IfBlankTF{<arg>}{<if true>}{<if false>}
\IfBlankT{<arg>}{<if true>}
\IfBlankF{<arg>}{<if false>}
```
An optional input can be passed but still be blank. The following example demonstrates this clearly:
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

#### Boolean Values "True" and "False"

This type of controllers checks inputs that can take boolean values. An input like `s` (the star) is one such example.

The values `True` and `False`, which we usually encounter in programming, are represented in LaTeX as follows:
```latex
\BooleanTrue % = True
\BooleanFalse % = False
```
In the [first tutorial](../L1/README.md), we used this condition to introduce starred commands, but we didn’t fully explain it. Now, let’s take a closer look:

```latex
\IfBooleanTF{<arg>}{<if true>}{<if false>}
\IfBooleanT{<arg>}{<if true>}
\IfBooleanF{<arg>}{<if false>}
```
The input for these commands is similar to the previous ones, but their working logic differs. They can even be applied to mandatory arguments:

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

🌱 Up to this point, we have learned how to take control of our document’s flow in LaTeX and modify it. In Part 2, we will discuss how to transform and control inputs before using them. Stay with us!

---

Authors: [Abolfazl Mohammadi](https://github.com/abolfazlmohammadi12), [Ehsan Aramide](https://github.com/EhsanAramide)

Contact us:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)