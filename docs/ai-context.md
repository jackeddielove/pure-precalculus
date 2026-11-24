I am developing a LaTeX lesson document (`02-systems-of-equations-2.tex`) and matching DoenetML exercises for a precalculus course. Please follow my exact preamble macros, formatting conventions, and DoenetML patterns below.

### 1. LaTeX Formatting & Conventions
* **Math Delimiters:** Use `\(...\)` for inline math and `\[...\]` for display math.
* **Matrices:** All numerical matrices use right-aligned (`r`) columns:
  * Standard matrices use `\begin{bmatrix*}[r] ... \end{bmatrix*}`.
  * Augmented matrices use my custom `\begin{augmatrix}{rrr} ... \end{augmatrix}` environment.
  * Step-by-step row operations beside augmented matrices use `\begin{rowops} ... \end{rowops}` with `\vphantom{0}` on unchanged rows.
* **Environments & Macros:**
  * Use `\begin{example}\label{ex:...} ... \end{example}` for worked examples (styled with a left gray rule).
  * Use `\begin{calloutbox} ... \end{calloutbox}` for highlighted definitions/rules (numbered automatically as Box 1, Box 2, etc.).
  * Use `\doenetlink{Title}{URL}` to insert Doenet activity callout boxes.
  * Use `\cref{...}` (`cleveref` with `capitalise, noabbrev, nameinlink`) for all internal references.

### 2. DoenetML Exercise Conventions
* **Structure & Numbering:**
  * Wrap multi-part exercises in `<problem>` with `<part>` children.
  * Do **not** include default labels like `"Activity 1"` in DoenetML code or headings, as they conflict with my custom numbering system.
  * Use `<m>...</m>` for all math expressions and `<ref to="URL">text</ref>` for links.
  * Render augmented matrices inside Doenet `<m>` tags using `\left[\begin{array}{cc|c} ... \end{array}\right]` (or `{ccc|c}`).
* **Input & Grading Syntax:**
  * **Numeric Inputs:** Use `<mathInput name="..."/>` graded via `<answer name="..."><award><when>$x = ... and $y = ...</when></award></answer>`.
  * **Equation Inputs:** Grade full equations using parentheses inside `<when>`: `<when>$eq1 = (4x - y = 9) and $eq2 = (2x + 3y = -5)</when>`.
  * **Matrix Inputs:** Place `<matrixInput name="mat1" numRows="1" numColumns="1"/>` (using unique names per part) inside `<answer name="...">` and grade with `<award><matrix><row>...</row></matrix></award>` (space-separated entries). Include the instruction: *"Use the r+/-, c+/- buttons to change the number of rows and columns."*
  * **Multiple Choice (Single):** Either `<choiceInput name="..."><choice name="...">...</choice></choiceInput>` graded with `<when>$choiceName.selected</when>`, or `<choice credit="1">` directly inside `<answer name="...">`.
  * **Multiple Choice (Select All):** Use `<answer selectMultiple><label>...</label><choice credit="1">...</choice></answer>`.
  * **Conditional Reveal:** Use `<group hide="not $c_uni.selected">...</group>` to display solution inputs only when a unique-solution choice is selected.
* **Figures:** Reference hosted images at `https://githubusercontent.com<filename>.png` using `<image source="..." width="320px"><shortDescription>...</shortDescription></image>`.
* **Pedagogical Alignment:** Match the notation (`R_1`, `R_2`, `R_3`, `R_i + cR_j`), structure, and progression of the examples in `02-systems-of-equations-2.tex`.

### 3. Reference DoenetML Templates
```xml
<!-- Matrix input template -->
<problem>
  <p>Write the augmented matrix corresponding to each system of linear equations (using the variable order <m>x, y</m> or <m>x, y, z</m>). Use the r+/-, c+/- buttons to change the number of rows and columns.</p>
  <part>
    <p><m>2x - 5y = 8</m><br/><m>-3x + y = 4</m></p>
    <answer name="ans1">
      <matrixInput name="mat1" numRows="1" numColumns="1"/>
      <award>
        <matrix>
          <row>2 -5 8</row>
          <row>-3 1 4</row>
        </matrix>
      </award>
    </answer>
  </part>
</problem>

<!-- Equation input template -->
<problem>
  <p>Write the system of linear equations corresponding to each augmented matrix.</p>
  <part>
    <p><m>\left[\begin{array}{cc|c} 4 & -1 & 9 \\ 2 & 3 & -5 \end{array}\right]</m></p>
    <p>Equation 1: <mathInput name="eq1_1"/></p>
    <p>Equation 2: <mathInput name="eq1_2"/></p>
    <answer name="sysAns1">
      <award>
        <when>$eq1_1 = (4x - y = 9) and $eq1_2 = (2x + 3y = -5)</when>
      </award>
    </answer>
  </part>
</problem>

<!-- Conditional choiceInput + unique solution template -->
<problem>
  <part>
    <p><m>3x + 2y = 7</m>, <m>5x - y = 3</m></p>
    <choiceInput name="choice1">
      <choice name="c1_inc">Inconsistent</choice>
      <choice name="c1_inf">Consistent with infinitely many solutions</choice>
      <choice name="c1_uni">Consistent with a unique solution</choice>
    </choiceInput>
    <group hide="not $c1_uni.selected">
      <p>Enter the unique solution:</p>
      <p><m>x = </m><mathInput name="x1"/>, <m>y = </m><mathInput name="y1"/></p>
    </group>
    <answer name="ans1">
      <award><when>$c1_uni.selected and $x1 = 1 and $y1 = 2</when></award>
    </answer>
  </part>
</problem>

<!-- Select-all multiple choice template -->
<answer selectMultiple>
  <label>Select all that apply.</label>
  <choice><m>Incorrect</m></choice>
  <choice credit="1"><m>Correct</m></choice>
</answer>
```

### 4. My Preamble (`preamble.tex`)
```latex
\usepackage{graphicx}
\usepackage{amsmath, amsthm, amssymb}
\usepackage{mathtools}
\usepackage{enumitem}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
\usepackage{mdframed}
\usepackage{float}
\usepackage{xcolor}
\usepackage{tcolorbox}
\usepackage{xurl}
\usepackage{colortbl}
\usepackage{hyperref}
\hypersetup{
    colorlinks=true,
    urlcolor=blue!70!black,
    linkcolor=purple!70!black
}
\usepackage[capitalise, noabbrev, nameinlink]{cleveref}

\theoremstyle{definition}
\newtheorem{definition}{Definition}
\newtheorem{example}{Example}
\surroundwithmdframed[
    hidealllines=true,
    leftline=true,
    linewidth=1.5pt,
    linecolor=gray!50,
    innerleftmargin=10pt,
    innerrightmargin=0pt,
    innertopmargin=2pt,
    innerbottommargin=2pt,
    skipabove=10pt,
    skipbelow=10pt
]{example}
\newtheorem{activity}{Activity}
\renewcommand{\theactivity}{\Alph{activity}}

\newcommand{\doenetlink}{%
    \begin{tcolorbox}[
        colback=gray!6,
        colframe=blue!60!black,
        boxrule=0pt,
        leftrule=3pt,
        arc=2pt,
        left=8pt, right=8pt, top=6pt, bottom=6pt,
        before skip=10pt, after skip=10pt
    ]
    \textbf{\href{#2}{#1}} \\
    {\small\ttfamily \url{#2}}
    \end{tcolorbox}%
}
\newmdenv[
    backgroundcolor=gray!15,
    linecolor=black,
    linewidth=1pt,
    innertopmargin=10pt,
    innerbottommargin=10pt,
    userdefinedwidth=0.8\textwidth,
    align=center,
    nobreak=true,
    settings={\refstepcounter{calloutbox}},
    frametitle={\textbf{Box \thecalloutbox}}
]{calloutbox}
\newcounter{calloutbox}
\renewcommand{\thecalloutbox}{\arabic{calloutbox}}
\crefname{calloutbox}{box}{boxes}
\Crefname{calloutbox}{Box}{Boxes}

\newenvironment{augmatrix}{%
  \left[\begin{array}{@{}#1!{\color{gray!75}\vrule}r@{}}%
}{%
  \end{array}\right]%
}

\newenvironment{rowops}{%
  \begin{array}{@{}l@{}}%
}{%
  \end{array}%
}
```

### 5. Current Document (`02-systems-of-equations-2.tex`)
```latex
\documentclass{article}

\input{preamble}

\title{Systems of Equations 2}
\author{}
\date{}

\begin{document}

\maketitle


\section{Introduction to Matrices}\label{sec:matrices}

In Systems of Equations 1, we learned to solve systems of linear equations using substitution and elimination. In this section, we formalize the elimination method using matrices. Recall that an \(n \times m\) system of equations is a system of \(n\) equations in \(m\) variables:
\begin{align*}
a_{11}x_1 + \cdots + a_{1m}x_m &= b_1 \\
&\vdots \\
a_{n1}x_1 + \cdots + a_{nm}x_m &= b_n.
\end{align*}
What distinguishes one system of equations from another is the coefficients \(a_{ij}\) and the constants \(b_k\). We can represent the coefficients and constants in rectangular arrays called \textbf{matrices} (singular, \textbf{matrix}). The coefficients are represented in an \(n \times m\) matrix and the constants are represented in an \(n \times 1\) matrix:
\[\begin{bmatrix}
a_{11} & \cdots & a_{1m} \\
\vdots & \ddots & \vdots \\
a_{n1} & \cdots & a_{nm}
\end{bmatrix} \quad \text{and} \quad \begin{bmatrix}
b_1 \\
\vdots \\
b_n
\end{bmatrix}.\]
Thus, for example, the system of equations
\begin{align*}
-x + 4y + 2z &= -3 \\
2x + 3y - z &= 5 \\
3x - y + z &= 2
\end{align*}
can be represented by the matrices
\[\begin{bmatrix*}[r]
-1 & 4 & 2 \\
2 & 3 & -1 \\
3 & -1 & 1
\end{bmatrix*} \quad \text{and} \quad \begin{bmatrix*}[r]
-3 \\
5 \\
2
\end{bmatrix*}.\]
Putting the coefficients and constants together, we can represent the system of equations as an \textbf{augmented matrix}:
\[\begin{augmatrix}{rrr}
-1 & 4 & 2 & -3 \\
2 & 3 & -1 & 5 \\
3 & -1 & 1 & 2
\end{augmatrix}.\]


\doenetlink{Systems of Equations 2 Preview Activity}{https://doenet.org/code/760268}

\section{Using Matrices to Solve Systems of Equations}\label{sec:using-matrices}

Now that we have encoded systems of equations as augmented matrices, we see how the elimination method of solving systems of equations is encoded as operations on those matrices.

\begin{example}\label{ex:elimination-matrix}
Consider the \(3\times 3\) system of equations
\begin{align*}
-x + 4y + 2z &= -3 \\
2x + 3y - z &= 11 \\
3x - y + z &= 6
\end{align*}
which is represented by the augmented matrix
\[
\begin{augmatrix}{rrr}
-1 & 4 & 2 & -3 \\
2 & 3 & -1 & 11 \\
3 & -1 & 1 & 6
\end{augmatrix}.
\]
Using the elimination method from Systems of Equations 1, we might first multiply the first equation by \(-1\) so that the leading coefficient of \(x\) is \(1\):
\begin{align*}
x - 4y - 2z &= 3 \\
2x + 3y - z &= 11 \\
3x - y + z &= 6.
\end{align*}
In matrix form, this corresponds to multiplying every entry in the first row by \(-1\):
\[
\begin{augmatrix}{rrr}
1 & -4 & -2 & 3 \\
2 & 3 & -1 & 11 \\
3 & -1 & 1 & 6
\end{augmatrix}.
\]
Next, we eliminate \(x\) from the second and third equations by subtracting \(2\) times the first equation from the second, and subtracting \(3\) times the first equation from the third:
\begin{align*}
x - 4y - 2z &= 3 \\
11y + 3z &= 5 \\
11y + 7z &= -3.
\end{align*}
Performing the corresponding operations on the rows of our augmented matrix yields
\[
\begin{augmatrix}{rrr}
1 & -4 & -2 & 3 \\
0 & 11 & 3 & 5 \\
0 & 11 & 7 & -3
\end{augmatrix}.
\]
To eliminate \(y\) from the third equation, we subtract the second equation from the third:
\begin{align*}
x - 4y - 2z &= 3 \\
11y + 3z &= 5 \\
4z &= -8.
\end{align*}
In the augmented matrix, subtracting the second row from the third row produces
\[
\begin{augmatrix}{rrr}
1 & -4 & -2 & 3 \\
0 & 11 & 3 & 5 \\
0 & 0 & 4 & -8
\end{augmatrix}.
\]
From the last row, we have \(4z = -8\), so \(z = -2\). Substituting \(z = -2\) back into the second equation gives
\[
11y + 3(-2) = 5 \implies 11y = 11 \implies y = 1,
\]
and substituting both values into the first equation yields
\[
x - 4(1) - 2(-2) = 3 \implies x = 3.
\]
Thus, the system has the unique solution \((3, 1, -2)\).
\end{example}

Notice that each step in the elimination method above corresponds to one of three basic manipulations on the rows of the augmented matrix, none of which changes the solution set of the underlying system.

\begin{calloutbox}
\textbf{Elementary Row Operations} Given an augmented matrix, the following three operations produce an augmented matrix representing an equivalent system of linear equations. To record each step, we write the operation that produces a new row directly beside that row:
\begin{enumerate}
    \item \textbf{Row Swap:} Interchange two rows (\(R_i\) written beside the new \(R_j\), and \(R_j\) written beside the new \(R_i\), ).
    \item \textbf{Row Scaling:} Multiply all entries in a row by a nonzero constant \(c \neq 0\) (written \(cR_i\) beside the new row \(i\)).
    \item \textbf{Row Replacement:} Add a multiple of one row to another row (written \(R_i + cR_j\) beside the new row \(i\)).
\end{enumerate}
\end{calloutbox}



Using this notation, we can write the entire elimination process from \cref{ex:elimination-matrix} compactly as a sequence of row-equivalent augmented matrices. We do this in the following example.

\begin{example}\label{ex:row-operations}
Use row operations to solve the following system of equations:
\begin{align*}
-x + 4y + 2z &= -3 \\
2x + 3y - z &= 11 \\
3x - y + z &= 6.
\end{align*}

\begin{align*}
\begin{augmatrix}{rrr}
-1 & 4 & 2 & -3 \\
2 & 3 & -1 & 11 \\
3 & -1 & 1 & 6
\end{augmatrix}
&\sim
\begin{augmatrix}{rrr}
1 & -4 & -2 & 3 \\
2 & 3 & -1 & 11 \\
3 & -1 & 1 & 6
\end{augmatrix}
\begin{rowops}
-R_1 \\
\vphantom{0} \\
\vphantom{0}
\end{rowops} \\[8pt]
&\sim
\begin{augmatrix}{rrr}
1 & -4 & -2 & 3 \\
0 & 11 & 3 & 5 \\
0 & 11 & 7 & -3
\end{augmatrix}
\begin{rowops}
\vphantom{0} \\
R_2 - 2R_1 \\
R_3 - 3R_1
\end{rowops} \\[8pt]
&\sim
\begin{augmatrix}{rrr}
1 & -4 & -2 & 3 \\
0 & 11 & 3 & 5 \\
0 & 0 & 4 & -8
\end{augmatrix}
\begin{rowops}
\vphantom{0} \\
\vphantom{0} \\
R_3 - R_2
\end{rowops}
\end{align*}
The bottom row in the last augmented matrix tells us that \(4z=-8\) and thus \(z=-2\). Substituting \(z=-2\) into the second row, \(11y+3z=5\), we have \(11y-6=5\) and thus \(y=1\). Substituting \(y=1\) and \(z=-2\) into the first row, \(x-4y-2z=3\) we have \(x-4+4=3\) and thus \(x=3\). We have arived at the unique solution to the system:  \((3, 1, -2)\).

\end{example}


\end{document}