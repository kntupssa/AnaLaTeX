[🇮🇷 fa](./README.md) | 🇬🇧 en
# Hyperlinks in LaTeX (Part 1)

In PDF documents, one of the most useful and interesting features is hyperlinks—links that take us to another part of the document or even to a website.
In LaTeX, the hyperref package makes this process very easy.

## 1️⃣ Installation and Usage
To use it, simply include the following in the document preamble:
```latex
\usepackage{hyperref}
```
Note: This package should always be the last one you load, as it redefines many LaTeX commands.
## 2️⃣ Automatic References and Links
The hyperref package turns many references into clickable hyperlinks. For example:

```latex
\usepackage{hyperref}
\section{Hyperlinks}\label{hyperlinks}

The reference to this section
now looks like that:\footnote{
Footnotes are also
made into hyperlinks.}

Section~\ref{hyperlinks} is
on page~\pageref{hyperlinks}.

The hyperref bibliographic
entry is~\cite{pack:hyperref}.
```
What happens?

Section references: The \ref{} command now appears as a clickable link in the PDF.
Example: "Section 5.3.1 on page 140".

Page references: The \pageref{} command also becomes a hyperlink.

Footnotes: Footnotes are converted into links as well.

Bibliography references: The \cite{} command becomes an active link.
## 3️⃣ Link Appearance
By default, links in PDF are marked with a red box. This box is visible on-screen only and will not appear when printing.
Since this is not visually appealing, it is recommended to use the colorlinks option:
```latex
\usepackage[colorlinks=true,linkcolor=blue,urlcolor=cyan,citecolor=red]{hyperref}

```
With these settings:

Internal links (like \ref) appear blue.

Web links (\url, \href) appear cyan.

Bibliography references (\cite) appear red.
## 4️⃣ URLs and Custom Link Text
The \href{URL}{text} command displays custom text instead of the full URL:
```latex
\href{https://www.nasa.gov/}{NASA}
% Displays "NASA" linking to the website
\url{https://www.ctan.org/}
% Displays a direct URL
\hyperref[hyperlinks]{Hyperlinks section}
% Link within the document with custom text
```
## 5️⃣ \autoref and \autopageref Commands
Frequent references like “Section 5.3.1 on page 140” led hyperref to introduce two convenient commands:

\autoref{label} → similar to \ref but also displays the type of counter (section, chapter, figure, etc.).

\autopageref{label} → similar to \pageref but adds text like "page".

Example:
```latex
\autoref{hyperlinks} in \autoref{specialities} on \autopageref{hyperlinks}.
% Output: "Subsection 5.3.1 in Chapter 5 on page 140"

```
## 6️⃣ Controlling Link Borders (pdfborder)
The pdfborder key takes three numbers: horizontal corner radius, vertical corner radius, and border thickness:
```latex
\hypersetup{pdfborder = {h v w}}
```
Values:

h → horizontal radius of the border corners

v → vertical radius of the border corners

w → border thickness (0 means no border)
Examples:
```latex
\hypersetup{pdfborder = 0 0 1}   % thin simple border
\hypersetup{pdfborder = 10 10 3} % thick border with rounded corners
\hypersetup{pdfborder = 0 0 0}   % no border
```
## 7️⃣ Link Border Colors
To assign different colors for different types of links (provided the xcolor package is loaded):

urlbordercolor → color of web links (\url, \href)

citebordercolor → color of bibliography references (\cite)

linkbordercolor → color of internal references (\ref, \autoref, etc.)

Example:
```latex
\hypersetup{
  pdfborder       = 0 0 2,
  urlbordercolor  = violet, % web links purple
  citebordercolor = pink,   % bibliography references pink
  linkbordercolor = teal,   % internal references green
}
```
In this section, we got familiar with the hyperref package in LaTeX and learned how to make internal links 📑, URLs 🌐, footnotes 📝, and bibliography references 📚 clickable hyperlinks. We also learned how to customize the text color 🎨 and border color 🖌️ of links to make the PDF more visually appealing and user-friendly.

In the next section , we will dive deeper and explore advanced commands and additional features of this package.


---
Authors: [Abolfazl Mohammadi](https://github.com/abolfazlmohammadi12)

Contact us:

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:kntu.physics.association@gmail.com)
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/physicskntu)
[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjxzdmcgaGVpZ2h0PSI3MiIgdmlld0JveD0iMCAwIDcyIDcyIiB3aWR0aD0iNzIiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGcgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj48cGF0aCBkPSJNOCw3MiBMNjQsNzIgQzY4LjQxODI3OCw3MiA3Miw2OC40MTgyNzggNzIsNjQgTDcyLDggQzcyLDMuNTgxNzIyIDY4LjQxODI3OCwtOC4xMTYyNDUwMWUtMTYgNjQsMCBMOCwwIEMzLjU4MTcyMiw4LjExNjI0NTAxZS0xNiAtNS40MTA4MzAwMWUtMTYsMy41ODE3MjIgMCw4IEwwLDY0IEM1LjQxMDgzMDAxZS0xNiw2OC40MTgyNzggMy41ODE3MjIsNzIgOCw3MiBaIiBmaWxsPSIjMDA3RUJCIi8+PHBhdGggZD0iTTYyLDYyIEw1MS4zMTU2MjUsNjIgTDUxLjMxNTYyNSw0My44MDIxMTQ5IEM1MS4zMTU2MjUsMzguODEyNzU0MiA0OS40MTk3OTE3LDM2LjAyNDUzMjMgNDUuNDcwNzAzMSwzNi4wMjQ1MzIzIEM0MS4xNzQ2MDk0LDM2LjAyNDUzMjMgMzguOTMwMDc4MSwzOC45MjYxMTAzIDM4LjkzMDA3ODEsNDMuODAyMTE0OSBMMzguOTMwMDc4MSw2MiBMMjguNjMzMzMzMyw2MiBMMjguNjMzMzMzMywyNy4zMzMzMzMzIEwzOC45MzAwNzgxLDI3LjMzMzMzMzMgTDM4LjkzMDA3ODEsMzIuMDAyOTI4MyBDMzguOTMwMDc4MSwzMi4wMDI5MjgzIDQyLjAyNjA0MTcsMjYuMjc0MjE1MSA0OS4zODI1NTIxLDI2LjI3NDIxNTEgQzU2LjczNTY3NzEsMjYuMjc0MjE1MSA2MiwzMC43NjQ0NzA1IDYyLDQwLjA1MTIxMiBMNjIsNjIgWiBNMTYuMzQ5MzQ5LDIyLjc5NDAxMzMgQzEyLjg0MjA1NzMsMjIuNzk0MDEzMyAxMCwxOS45Mjk2NTY3IDEwLDE2LjM5NzAwNjcgQzEwLDEyLjg2NDM1NjYgMTIuODQyMDU3MywxMCAxNi4zNDkzNDksMTAgQzE5Ljg1NjY0MDYsMTAgMjIuNjk3MDA1MiwxMi44NjQzNTY2IDIyLjY5NzAwNTIsMTYuMzk3MDA2NyBDMjIuNjk3MDA1MiwxOS45Mjk2NTY3IDE5Ljg1NjY0MDYsMjIuNzk0MDEzMyAxNi4zNDkzNDksMjIuNzk0MDEzMyBaIE0xMS4wMzI1NTIxLDYyIEwyMS43Njk0MDEsNjIgTDIxLjc2OTQwMSwyNy4zMzMzMzMzIEwxMS4wMzI1NTIxLDI3LjMzMzMzMzMgTDExLjAzMjU1MjEsNjIgWiIgZmlsbD0iI0ZGRiIvPjwvZz48L3N2Zz4=&logoColor=white)](https://www.linkedin.com/company/physics-kntu)
[![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white)](https://www.instagram.com/kntuphysics)