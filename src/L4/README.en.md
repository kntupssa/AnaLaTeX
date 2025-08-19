[🇮🇷 fa](./README.md) | 🇬🇧 en

# Program Flow and Inputs Control in LaTeX (Part 2)

Following [Part 3 of the tutorial](../L3/README.md), in this lesson we’ll learn how to work with input processors and modify inputs **before** they are actually used.

What we covered in the previous tutorial was an introduction to programming in LaTeX, but the story with LaTeX can get much more complex and dynamic! Before starting this tutorial, we suggest you revisit the beginning of Part 3 to recall the examples and the goal of these two parts.

## Argument Processors

Argument processors are commands that perform operations on inputs **after** they are collected and **before** they are passed to the code, which allows modifying inputs and making code more dynamic.

To make things clearer, let’s look at this code:
```latex
\NewDocumentCommand{\<cmd>}{<arg-spec>}{<code>}
```
This is the schematic definition of a command; keep it in mind throughout this section. The working process of processors, from the deep structure of LaTeX, looks like this:

**Step 1**: LaTeX collects the inputs provided to the command `cmd` based on the defined `arg-spec`.

**Step 2**: These collected inputs, if a processor is defined, are not directly passed to the `code` part, but instead are sent to the processor to modify and handle them.

**Step 3**: The processed and modified inputs are then passed to the `code` part.

Some important notes about processors:

**Note 1**: To use processors, they must be written in the code as follows:
```latex
\NewDocumentCommand{\<cmd>}{o m >{\<processor-name>} m}
```
They always operate on the argument immediately following them, for example, in this case, on the last mandatory argument.

**Note 2**: Arguments are processed from right to left:
```latex
% First, `processor-1` will be applied on `m`, then, `processor-2` will be applied on the output of `processor-1`.
>{\<processor-2>} >{\<processor-1>} m
```

**Note 3**: Processors work on user-provided arguments and default values, but they do not operate on the special value `-NoValue-`.

## Commonly Used Argument Processors

Now that we understand processors in general, let’s learn how to work with some commonly used ones and explore a few tricks with examples.

### Processor `SplitArgument`
```latex
\SplitArgument{<number>}{<token(s)>}
```
It takes an input and searches for `<number>` occurrences of `<token(s)>`, splitting the input at those points. This process continues until it returns a list consisting of `number + 1` elements. For example, try this code:
```latex
\NewDocumentCommand{\splitter}{ >{\SplitArgument{2}{,}} m }{
    {\textit #1}
}

% Check the result
\splitter{Ehsan, Abolfazl}
```
Two points are hidden in this command:  
1) You’ll get a `-NoValue-` because only one comma exists, but you told the processor to split by two commas.  
2) Only the first element is italicized. This happens because after splitting, what LaTeX places in front of the command looks like this:
```latex
{\textit {Ehsan}{Abolfazl}}
```
Since the command takes only one argument, providing two arguments is ineffective.

### Processor `SplitList`
```latex
\SplitList{<token(s)>}
```
It takes an input and splits it wherever it encounters `<token(s)>`.

For example:
```latex
\NewDocumentCommand{\ListGen}{ O{,} >{\SplitList{#1}} m }{
    \DoSomething #2
}
```
The first point here is that processors can use previous arguments as their inputs.  

The second point can be seen in the following code: the number of splits does not matter for this processor; it splits as many times as needed:
```latex
% What you pass
{Ehsan, Abolfazl, Amirreza}

% What you get if the delimiter is `,`
{Ehsan}{Abolfazl}{Amirreza}
```

### Processor `ProcessList`
```latex
\ProcessList{<list>}{<function>}
```
After the `SplitList` processor splits the input into a list of elements, this command iterates over the elements one by one, sending each as input to `<function>` to perform an operation.

By now, you should understand how, when we provide optional arguments to the `documentclass` command, it processes them. Simply put, we can imagine it like this:
```latex
\NewDocumentCommand{\SomeCommand}{ >{\SplitList{,}} o m }{
    \ProcessList{#1}{\ApplyOptions}
    \ApplyVar{#2}
}

% This can be used as below:
\SomeCommand[opt1, opt2, opt3]{parameter}
```

🌱 Now that we’ve learned how to create custom commands and environments, and even do some basic programming in LaTeX in a simple way, a question arises: if I want to use all my creations in other projects, should I copy them manually from project to project? Our answer in the next tutorial will be: no! Stay tuned.

---

Authors: [Abolfazl Mohammadi](https://github.com/abolfazlmohammadi12), [Ehsan Aramide](https://github.com/EhsanAramide)

Contact us:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)