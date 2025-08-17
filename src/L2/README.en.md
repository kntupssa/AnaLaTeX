[🇮🇷 fa](./README.md) | 🇬🇧 en

# Creating New Environments in LaTeX

As we learned in the [previous tutorial](../L1/README.md), custom commands in LaTeX empower us to personalize the document creation process and move beyond the standard LaTeX usage. In this tutorial, we are going to learn how to create our own custom environments.

When you first install LaTeX, you usually encounter something like this:

```latex
\documentclass{article}

% This is called `document environment`
\begin{document} 
\end{document}
```
In fact, what we have been writing all our document text inside so far is called the *document* environment. But how can customizing such things help us? First, let’s learn how to create a custom environment, and then explore practical examples of how they can be useful.

## How to Create a Custom Environment?

Just like in the previous tutorial, we use similar commands [with slight differences] to define an environment. The usage is the same, only the names are different:

```latex
\NewDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % First
\RenewDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % Second
\ProvideDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % Third
\DeclareDocumentEnvironment{<name>}{<argspec>}{<begin-def>}{end-def} % Fourth
```
The differences between environment-defining commands and command-defining ones are:

- Two separate parts: one for the environment start and one for the end
- No backslash (`\`) before the environment name
- In the Argument Specification, you can capture the text inside the environment as input using the letter `b`

In other aspects, they behave similarly.

Now let’s explore some examples to better understand the power LaTeX gives us.

## Examples

*Example 1*: Define an environment that surrounds the text inside with `kntupssa`.

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

*Example 2*: Define an environment for mathematical formulas that adds a custom text at the end.

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
This example teaches two points:

1. The plus sign (`+`) before arguments tells LaTeX that the input can span multiple lines. (This is an optimization feature.)
2. When capturing all text inside an environment, it’s better to put that argument last in the argspec. Also, when receiving the text, it’s cleaner if one of the parts (begin or end) is left empty, so the code stays neat and consistent.

*Example 3*: Define an environment that centers text on the page, sets the font style to italic by default, but allows customization.

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

*Example 4*: Define an environment that can optionally have a star form. This example contains a useful trick.

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
When using the `s` type argument for environments, note that the star appears right after the braces. Alternatively, we can define two separate environments:

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
In this method, we simply define two environments - one with a star, one without. This is a standard coding practice that keeps code cleaner and more consistent with LaTeX’s convention for starred commands.

🌱 Over the last two tutorials, we learned how to create custom commands and environments in LaTeX. But there’s still more: controlling the flow of our document generation. Stay tuned for the next tutorials! Your feedback helps us improve, please share your thoughts with us.

---
Author: [Ehsan Aramide](https://github.com/EhsanAramide)

Contact us:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)