🇬🇧 en | [🇮🇷 fa](./README.md)

# Creating classes in LaTeX

In the previous section, we learned how to create a package.  
The important question that arises is: when should we use a package and when a class?

- If your commands can be used with any document class → implement them as a **package (.sty)**.  
- If the commands are specific to a particular type of document and cannot be used with other classes → you should create a **class (.cls)**.

#### Types of classes
1. Independent classes such as `article`, `report`, or `letter`.  
2. Extended classes that are built upon another class.  
   - Example: The `proc` class is built upon `article`.

   #### Examples
- A company can create a local class called `ownlet` for printing its own special letters.  
  This class is built upon `letter` and cannot be used with another class → so its file will be `ownlet.cls`.  

- The `graphics` package provides a set of commands for inserting images and can be used with any class → so its file is `graphics.sty`.

#### Using docstrip and doc for writing classes

If you want to create a **large class in LaTeX**, it is better to use the **doc** tool that comes with LaTeX.

Classes written in this way can be processed in two ways:
1. Running the file with LaTeX → generates documentation.  
2. Processing the file with **docstrip** → generates the `.cls` file.

##### Advantages of using doc
- Automatic generation of the list of definitions and commands.  
- Recording changes as a list.  
- Suitable for maintaining and documenting large sources.

##### Standard sources
- The LaTeX kernel and standard classes are written as **`.dtx`** files.  
- These files include both source code and can be typeset as a document.  
- For example, running LaTeX on `source2e.tex` produces the kernel documentation.

##### Note
Documentation of these files is done using the `ltxdoc.cls` class.  
For more information, you can refer to the files `docstrip.dtx` and `doc.dtx` or the book *The LaTeX Companion*.

#### Policy regarding standard classes

Many reports about standard classes are not actually bugs; rather, they are suggestions for changes in their design.

However, changing **standard classes** like `article`, `report`, or `book` is not the right thing to do, because:

- The current behavior is exactly what was intended in the original design.
- Many users rely on this existing design.
- Changing these classes would cause incompatibility and widespread problems.

Therefore, the LaTeX policy is that major changes should not be made to these classes—even if there are shortcomings.

The recommended solution is that if you need different features or design, you should create a **new class** based on the standard classes and add your desired behavior.

#### Command names in classes

Before the introduction of the L3 programming layer, LaTeX had three types of commands:

1. Author commands  
   Commands that the author uses directly, such as:  
   ```latex
   \section
   \emph
   \times
   ```

2. Class writer commands  
    These commands are for use in class files (.cls) and usually have longer names or a mix of uppercase and lowercase letters, such as:
    ```latex
    \InputIfFileExists
    \PassOptionsToClass
    \LoadClassWithOptions
    ```

3. Internal commands  
    These commands are used in the internal implementation of LaTeX and often include the @ symbol, such as:
    ```latex
    \@tempcnta
    \@ifnextchar
    \@eha
    ```
    The presence of @ indicates that these commands are not suitable for use in a regular document and are only allowed in classes.


General rule:

    Commands that include @ are not part of the official LaTeX language and their behavior may change in future versions.

    Commands with mixed-case names or described in the book LaTeX: A Document Preparation System have official support and will be maintained in the future.


#### Box and color commands in classes

To ensure class compatibility with color and avoid potential issues, it is better to always use box commands in LaTeX and avoid primitive TeX commands:

- Use `\sbox` instead of `\setbox`  
- Use `\mbox` instead of `\hbox`  
- Use `\parbox` or the `minipage` environment instead of `\vbox`

Box commands in LaTeX have advanced options that are as powerful as primitive TeX commands.

##### Possible color issue
In the following case, the font is correctly restored before the closing `}`:

```latex
{\ttfamily <text>}
```
But in the color case:
```latex
{\color{green} <text>}
```
The color is restored after the last `}`. If primitive TeX commands are used, such as:

```latex
\setbox0=\hbox{\color{green} <text>}
```

#### General style of classes

LaTeX provides many commands that help you create classes that are structured, stable, and portable.

##### Loading other classes

To use classes within other classes, LaTeX provides the following commands:

```latex
\LoadClass
\LoadClassWithOptions
```
It is recommended to use these commands and avoid the primitive `\input` command.

Reasons:

Files loaded with `\input` are not listed in `\listfiles`.

Classes loaded with `\LoadClass` are only loaded once, even if requested multiple times.

Using `\input` may cause unexpected results and class options may not be processed correctly.

#### Structure of a class

The general outline of a class file is as follows:

1. **Identification**  
   The file specifies that it is a LaTeX2ε class and provides a brief description of itself.

2. **Preliminary declarations**  
   In this section, some commands are declared and other required classes or files can be loaded. These commands are usually needed for option processing.

3. **Options**  
   The class declares and processes its options.

4. **More declarations**  
   In this section, most of the class work is done: declaring variables, new commands and fonts, and loading other files.

   #### Class identification

The first thing a class file does is identify itself.

##### Structure of class identification:

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{<class-name>}[<date> <other information>]
```

Example:
```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{article}[2022-06-01 Standard LaTeX class]
```
The `<date>` must be in the format `yyyy-mm-dd` or `yyyy/mm/dd`.

The year must be exactly four digits, and both the month and day must be two digits.

If the date is not entered correctly, it may be misinterpreted and using the class may cause problems.

Important notes:

This date is checked when using `\documentclass`:
```latex
\documentclass{article}[2022-06-01]
```

The class description is displayed when it is used and is recorded in the log file.

The phrase Standard LaTeX should only be used for classes included in the standard LaTeX distribution.

#### Using classes within other classes

A LaTeX class can load another class to use its features and structure.

##### Main commands

```latex
\LoadClass[<options>]{<class-name>}[<date>]
```

Example:
```latex
\RequirePackage{ifthen}[2022-06-01]
```

This command works like \documentclass and allows classes to be built upon an existing class.

The class author can change only parts of the base class without starting from scratch.

Loading a class with the same options:
```latex
\LoadClassWithOptions{<class-name>}[<date>]
```

Example:
```latex
 \LoadClass[twocolumn]{article}
 ```
 This method allows the class to apply all options used in the current class to the base class.

 #### A minimal class file

A minimal LaTeX class should include the main definitions for document appearance and page numbering.

##### Main items in a minimal class

- Define the main text size with `\normalsize`
- Set the text width and height: `\textwidth` and `\textheight`
- Specify page numbering

##### Example of a minimal class file

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{minimal}[2022-06-01 Standard LaTeX minimal class]

% Define text size
\renewcommand{\normalsize}{\fontsize{10pt}{12pt}\selectfont}

% Set text dimensions
\setlength{\textwidth}{6.5in}
\setlength{\textheight}{8in}

% Page numbering
\pagenumbering{arabic}
```

Note: This minimal class does not support footnotes, margins, or floats, and does not provide any two-letter font commands like `\rm`. Most classes offer more content and features than this minimum.

#### Example 1: A local letter class

A company may have its own letter class to format letters in the company style. Here is a simple implementation of such a class.

#### Class introduction

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{neplet}[2022-06-01 NonExistent Press letter class]
```
Passing options and loading the base class

```latex
\DeclareOption*{\PassOptionsToClass{\CurrentOption}{letter}}
\ProcessOptions\relax
\LoadClass[a4paper]{letter}
```
Defining the page style for the letterhead

```latex
\renewcommand{\ps@firstpage}{%
  \renewcommand{\@oddhead}{<letterhead goes here>}%
  \renewcommand{\@oddfoot}{<letterfoot goes here>}%
}
```
This simple example shows how a local letter class can be created. The actual class may need more structure and features.

#### Example 2: A newsletter class

A simple newsletter can be created with LaTeX using a variant of the article class. The class starts by introducing itself as `smplnews.cls`:

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{smplnews}[2022-06-01 The Simple News newsletter class]
\newcommand{\headlinecolor}{\normalcolor}
```

Processing options and loading the base class:

```latex
\DeclareOption{onecolumn}{\OptionNotUsed}
\DeclareOption{green}{\renewcommand{\headlinecolor}{\color{green}}}
\DeclareOption*{\PassOptionsToClass{\CurrentOption}{article}}
\ProcessOptions\relax
\LoadClass[twocolumn]{article}
```

Custom `\maketitle` definition:

```latex
\renewcommand{\maketitle}{%
\twocolumn[% 
\fontsize{72}{80}\fontfamily{phv}\fontseries{b}%
\fontshape{sl}\selectfont\headlinecolor
\@title
]% 
}
```

Redefining \section and section numbering:

```latex
\renewcommand{\section}{%
  \@startsection
  {section}{1}{0pt}{-1.5ex plus-1ex minus-.2ex}%
  {1ex plus .2ex}{\large\sffamily\slshape\headlinecolor}%
}
\setcounter{secnumdepth}{0}
```

Setting font size and page dimensions:

```latex
\renewcommand{\normalsize}{\fontsize{9}{10}\selectfont}
\setlength{\textwidth}{17.5cm}
\setlength{\textheight}{25cm}
```

This skeleton is a starting point for creating a newsletter class. The actual class may provide more features such as issue numbers, article authors, and page styles.

#### Commands for class authors

This section briefly explains commonly used commands for class authors.

For further reading about the LaTeX system, you can refer to standard sources such as *LaTeX: A Document Preparation System* or *The LaTeX Companion*.

##### Class identification

To identify a class file in LaTeX, use the following commands:

 `\NeedsTeXFormat{⟨format-name⟩}[⟨release-date⟩]`  
  Specifies that this file should be processed with a particular format.  
   Standard format: `LaTeX2e`  
   Date (optional): should be written as `yyyy-mm-dd` or `yyyy/mm/dd`.  
   If the system date is older, a warning is issued.

**Example:**
  ```latex
  \NeedsTeXFormat{LaTeX2e}[2022-06-01]
  ```

`\ProvidesClass{⟨class-name⟩}[⟨release-info⟩]`
Declares that the current file is a class.

release-info includes the release date and, if needed, a short description or version number.

This information is used for version checking when loading the class.

Descriptions should not be long.

Example:
```latex
\ProvidesClass{article}[2022-06-01 v1.0 Standard LaTeX class]
```

`\ProvidesFile{⟨file-name⟩}[⟨release-info⟩]`
Similar to the previous command but for declaring auxiliary files (other than the main class file).

Example:
```latex
\ProvidesFile{T1enc.def}[2022-06-01 v1.0 Standard LaTeX file]
```
 Note: The phrase Standard LaTeX should only be used in official files of the standard LaTeX distribution.

#### Loading classes

To create a new class, you can use existing classes. This is done with the following commands:

 `\LoadClass[⟨options⟩]{⟨class-name⟩}[⟨release-info⟩]`  
  Loads another class in the class file.  
   Similar to using `\documentclass`.  
   Used only once in each class file.

`\LoadClassWithOptions{⟨class-name⟩}[⟨release-info⟩]`  
  Same as the previous command but also passes all options from the current file to the base class.

**Examples:**
```latex
\LoadClass{article}[2022-06-01]
\LoadClassWithOptions{article}[2022-06-01]
```
These commands allow new classes to be built based on standard classes and only desired parts to be changed.

#### Creating and using key–value options in classes

Key–value options in classes have two parts: **creation** and **usage**. These options can be used while loading the class or in the preamble.

##### Creating options
`\DeclareKeys[⟨family⟩]{⟨declarations⟩}`  
Creates a set of options based on the ⟨declarations⟩ list.  
Each option has one or more properties.

##### Main properties
`.code` — Executes custom code  
`.if` — Sets a switch `\if…`  
`.ifnot` — Sets a reverse switch  
`.pass-to-packages` — Specifies that the class option is globally passed to packages  
`.store` — Stores the value in a macro  
`.usage` — Specifies where the option can be used:  
  `load` (only when loading the class)  
  `preamble` (in the preamble)  
  `general` (no restriction)

##### Example
```latex
\DeclareKeys[myclass]{
  draft.if = @myclass@draft,
  draft.usage = preamble,
  name.store = \@myclass@name,
  name.usage = load
}
```

draft → is given in the preamble and sets the switch `\if@myclass@draft`.

name → is only given when loading the class and its value is stored in `\@myclass@name`.

#### Unknown options

`\DeclareUnknownKeyHandler[⟨family⟩]{⟨code⟩}`
Specifies the behavior when encountering an unknown key.

Key name = `#1`

Value = `#2`

Full option = `\CurrentOption`

##### Example:
 Forward unknown option to the `article` class

```latex
\DeclareUnknownKeyHandler{
  \PassOptionsToClass{\CurrentOption}{article}
}
```
#### Processing options

`\ProcessKeyOptions[⟨family⟩]`
Checks and processes all options given to the current class.
No need for `\ProcessOptions`.

#### Manual setting

`\SetKeys[⟨family⟩]{⟨keyvals⟩}`
Explicitly sets a set of options for the class.
If ⟨family⟩ is not specified, the value of `\@currname` is used.

### Summary

Up to this point, we have become familiar with the basic concepts of creating classes in LaTeX. We learned that:

- What is the difference between a class and a package, and when to use each.
- The general structure of a class includes identification, preliminary declarations, options, and the class body.
- How classes can load other classes and transfer options.
- The method of identifying a class with the commands `\NeedsTeXFormat` and `\ProvidesClass`, and the importance of date and description.
- How to define basic commands like `\normalsize`, page dimensions, and numbering in a class.
- With practical examples like a local letter class and a newsletter class, we saw how to redefine commands and set custom styles.
- Creating and managing key–value options for classes with `\DeclareKeys` and processing them with `\ProcessKeyOptions`.
- How to handle unknown options with `\DeclareUnknownKeyHandler` and the possibility of passing them to other classes.

With this knowledge, we can now create custom classes for different types of documents and have full control over the appearance and behavior of our LaTeX document.

Author:[Mohammad Taha Roshanbin](https://github.com/M-tahaRoshanbin)

Contact us:


[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)

