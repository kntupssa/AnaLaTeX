[🇮🇷 fa](./README.md) | 🇬🇧 en

# Creating New Commands in LaTeX

The LaTeX software platform allows its users to take full advantage of customization and automate their workflow in a personal way. One of the unique and interesting features of LaTeX in this regard is designing and creating custom commands.

Why do we need such a feature when writing our documents with LaTeX? This question may arise for many, but the answer can be short and straightforward: when the default LaTeX commands do not fit your needs, or you want to do something beyond the usual usage.

In LaTeX, commands that are defined are called **Macros** (whether defined by the user or by the LaTeX developers). Before LaTeX natively supported these features, the code was provided as a package called **xparse**, which in 2020 was integrated into the LaTeX core.

## How to Define a Command?

To do this, you can use the following four commands:
```latex
\NewDocumentCommand{\<name>}{<argspec>}{<definition>} % First
\RenewDocumentCommand{\<name>}{<argspec>}{<definition>} % Second
\ProvideDocumentCommand{\<name>}{<argspec>}{<definition>} % Third
\DeclareDocumentCommand{\<name>}{<argspec>}{<definition>} % Fourth
```
Now let’s go into the details of each command:

| Usage | If the command did not exist | If the command already existed | Command |
| -   | -   | -    | - |
| Only define a new command | Defines it | Raises an error | First |
| Change the behavior of an existing command | Raises an error | Redefines it | Second |
| Safe definition, without overwriting existing | Defines it | Does nothing | Third |
| (Re)define a command | Defines it | Redefines it | Fourth |

### Defining Command Arguments

In LaTeX, commands can take inputs, but how do we define them? This is where the second part comes in, namely **argspec**, which is short for *Argument Specification*.

The definition of argspec is a string of characters, each of which has a special meaning in LaTeX. We’ll cover four of them here. In general, arguments are either mandatory or optional:

- Mandatory
    1. Argument `m`: Normal and required. That means it must be written inside curly braces `{ }`.

- Optional
    1. Argument `o`: Optional inside `[ ]`. If omitted, a special value called `-NoValue-` is returned.
    2. Argument `O{default}`: Similar to `o`, but if omitted, it uses the given default value instead.
    3. Argument `s`: Optional star. If present, it returns `True`; if not, `False`.

### Some Examples

Now, to better understand how to create a command, let’s review some examples:

*Example 1*: To start, let’s make a simple command with no input, which just returns a text.

```latex
\NewDocumentCommand{\kntupssa}{}{ %
    K. N. Toosi University \emph{Physics Students Scientific Association}
} %
```

*Example 2*: Let’s create a command that generates a "ket" in Dirac notation.

```latex
\NewDocumentCommand{\ket}{m}{ %
    \left|#1\right.\rangle
} %

% Check the result
$\ket{v}$
```
Note that the numbering of each variable is based on the order in the argspec, and to use it you must prefix it with a hash `#`.

*Example 3*: We create a command that defines the inner product in Dirac notation. If one of the vectors is not provided, a question mark will be shown instead.

```latex
\NewDocumentCommand{\innerproduct}{O{?} O{?}}{ %
    \langle#1|#2\rangle
} %

% Check the result
$\innerproduct[\psi]$
$\innerproduct[\psi][\phi]$
$\innerproduct$
```

*Example 4*: Let’s show the inner product in both Dirac and Gibbs notation.

```latex
\NewDocumentCommand{\innerproduct}{s O{?} O{?}}{ %
    \IfBooleanTF {#1} { % if it's starred
        \vec{#2}.\vec{#3} % Gibbs Notation
    } { % if it isn't starred
        \langle#2|#3\rangle % Dirac Notation
    }
} %

% Check the result
$\innerproduct[v][w]$
$\innerproduct*[v][w]$
$\innerproduct*$
$\innerproduct$
```
The control command `IfBooleanTF` actually represents a conditional: "If the given variable is true, execute the first block; otherwise, execute the second."

Thank you very much for staying with us!  
We look forward to your ideas and questions. We’d be delighted if you’d like to share knowledge about LaTeX with others and join us in contributing.

🌱 So far, you may also be wondering things like: *How do we create a new environment in LaTeX?* or *How do we take control of our document workflow and code on the LaTeX platform like a programming language?*  
We’ll cover these in the next tutorials!

---
Authors: [AmirReza Taleghani](https://github.com/AmirRezaTaleghani), [Ehsan Aramide](https://github.com/EhsanAramide)

Contact the Scientific Association:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)
