🇬🇧 en | [🇮🇷 fa](./README.md)

# Creating Our Own Packages

So far, we’ve learned how to create and control our own commands and environments. But if their number increases, the **preamble** of our LaTeX file becomes cluttered. Moreover, if we need them in another LaTeX file, we’d have to rewrite everything from scratch. This is where creating a **package** becomes a great idea.

By creating our own package and putting the new commands and environments we’ve defined inside it, we can import it into any LaTeX document whenever we need it, simply using the `\usepackage{<package_name>}` command.

## Naming Convention

Typically, internal LaTeX commands that users are not supposed to use directly are named with an `@` character. In defining packages, there is no restriction on using `@` in command names, but if we use `@` directly in our documents, we’ll get an error. This is to prevent interference with LaTeX’s internal commands and other packages. However, if we really want to use `@` in command names inside our document, we can enable it with `\makeatletter` before usage and disable it with `\makeatother` afterward.

```latex
\makeatletter
\NewDocumentcommand{\@pssa}{}{my name has @}
\makeatother
```

In general, a standard convention for naming commands used **inside a package** (not meant to be used directly in documents) is:
```
\<command_name>@<package_name>
```
But commands intended for use in documents can be named in the regular way.

## Creating a Package

To create a package, the first step is to make a file named `<package_name>.sty`. The `.sty` extension is mandatory, and the file name must match the package name.

For the package to be recognized when included in our document, it must be placed in the same folder as the LaTeX file (although it can also be installed globally for all documents, which varies depending on the OS and TeX distribution).

Now let’s see how we can define the body of our package.

## Package Structure

Here’s a general example of a package structure, followed by explanations of each part:

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{<Package_name>}[1404/05/29 v0.1 a sample latex package]

\RequirePackage{kntu}
\RequirePackage{pssa}
...

% defining initial values for commands or variables if needed

\DeclareOption{option}{definition}

\ExecuteOptions{<option_name>}

\ProcessOptions\relax

% defining our commands, environments, etc.

\endinput
```
This is the general structure for defining a package. Let’s break it down:

- `\NeedsTeXFormat{LaTeX2e}` specifies the minimum required version of TeX or LaTeX to run the package (stick with LaTeX2e for now; later we’ll also talk about LaTeX3).  
- `\ProvidesPackage{<Package_name>}[version]` declares the package. The optional `[version]` argument usually contains the date, version number, and a short description.  
- `\RequirePackage{...}` loads other required packages (equivalent to `\usepackage` in normal documents).  
- `\DeclareOption{option}{definition}` defines what happens if the user provides an option when loading the package.  
- `\ExecuteOptions{option}` sets the default option if the user doesn’t provide one.  
- `\ProcessOptions\relax` checks and applies the options. `\relax` doesn’t do anything special—it just safely ends the option-processing command. After this, we define our commands and environments.  
- `\endinput` must always appear at the end of the package. Anything written after it is ignored.

Using this approach, we can create our own package and use it in our LaTeX documents.

### Example

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{mynote}[2025/08/20 0.1v Simple colorful notes]

% packages used
\RequirePackage{xcolor}

% defining options
\DeclareOption{red}{\def\mynot@notecolor{red}}
\DeclareOption{blue}{\def\mynot@notecolor{blue}}
\DeclareOption{green}{\def\mynot@notecolor{green}}

% if user doesn’t give an option, default is red
\ExecuteOptions{red}

% complete the option creation process
\ProcessOptions\relax

% create a new command to write colored text
\NewDocumentCommand{\mycolorednote}{m}{\textcolor{\mynot@notecolor}{\textbf{#1}}}

\endinput
```

In this example, if the user specifies one of the colors as an optional argument when loading the package, the `\def` command sets a variable with that color value, which is then used in the new command we defined. It’s a simple example but very useful for understanding how commands work inside packages.

So now we’ve learned how to create a personal package in LaTeX. With this simple tool, we can add our personal ideas and needs to LaTeX in a professional way, essentially building our own toolbox. Instead of repeating commands in every project, we just write the package once and reuse it as many times as we want.

---
Author: [Seyed AmirReza Taleghani](https://github.com/AmirRezaTaleghani)

Contact us:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)
