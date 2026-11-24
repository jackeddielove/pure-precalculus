# Pure Precalculus — AI Context & Conventions

I am developing LaTeX lesson documents (`tex/0X-topic.tex`) and matching DoenetML preview activities and exercises for a precalculus course. Please follow my exact preamble macros, formatting conventions, and DoenetML patterns below.

## 1. Course Structure & Workflow
* **Repository Layout:**
  * `tex/`: Source `.tex` files (`preamble.tex`, `01-systems-of-equations-1.tex`, `02-systems-of-equations-2.tex`, `03-sequences.tex`, etc.).
  * `docs/`: Compiled PDFs hosted via GitHub Pages alongside `index.md`.
* **Course Index (`index.md`):** Tracks each section with links to `[Read Notes](0X-....pdf)`, `[Start Preview](https://doenet.org/code/...)`, and `[Start Exercises](https://doenet.org/code/...)`.
* **Linking Policy:**
  * In `.tex` files, use `\doenetlink{Title}{URL}` **only** for **Preview Activities** (never for homework/exercises).
  * In DoenetML, link back to specific examples or sections in hosted PDFs using `<ref to="https://.../0X-topic.pdf#nameddest=label">Text</ref>` (when `destlabel=true` is enabled in `\hypersetup`) or `#page=N`.

## 2. LaTeX Formatting & Conventions
* **Math Delimiters:** Use `\(...\)` for inline math and `\[...\]` for display math.
* **Sectioning:** Use top-level `\section{...}\label{sec:...}` headings; avoid numbered subsections (`\subsection`).
* **Matrices:** All numerical matrices use right-aligned (`r`) columns:
  * Standard matrices use `\begin{bmatrix*}[r] ... \end{bmatrix*}`.
  * Augmented matrices use custom `\begin{augmatrix}{rrr} ... \end{augmatrix}`.
  * Step-by-step row operations beside augmented matrices use `\begin{rowops} ... \end{rowops}` with `\vphantom{0}` on unchanged rows.
* **Environments & Macros:**
  * `\begin{example}\label{ex:...} ... \end{example}` for worked examples (left gray rule).
  * `\begin{calloutbox} ... \end{calloutbox}` for highlighted definitions/rules (numbered automatically as Box 1, Box 2, etc.; cannot take `\label` directly unless placed inside the body).
  * `\doenetlink{Title}{URL}` for Preview Activity callout boxes.
  * `\cref{...}` (`cleveref` with `capitalise, noabbrev, nameinlink`) for all internal references.

## 3. DoenetML Exercise Conventions
* **Structure & Numbering:**
  * Wrap multi-part exercises in `<problem>` with `<part>` children.
  * Do **not** include default labels like `"Activity 1"` in DoenetML code or headings, as they conflict with my custom numbering system.
  * Use `<alert>...</alert>` for bold text (never `<strong>`).
  * Use `<m>...</m>` for all math expressions and `<ref to="URL">text</ref>` for links.
  * Keep exercises deterministic (hardcoded numbers, no `<selectRandomNumbers>`) unless explicitly requested.
  * Render matrices inside `<m>` tags with right-aligned columns: `\left[\begin{array}{rrr|r} ... \end{array}\right]` for augmented matrices and `\left[\begin{array}{rrrr} ... \end{array}\right]` for standard matrices.
* **Input & Grading Syntax:**
  * **Numeric Inputs:** `<mathInput name="..."/>` graded via `<answer name="..."><award><when>$x = ... and $y = ...</when></award></answer>`.
  * **Equation Inputs:** Grade full equations using parentheses inside `<when>`: `<when>$eq1 = (4x - y = 9) and $eq2 = (2x + 3y = -5)</when>`.
  * **Matrix Inputs:** Place `<matrixInput name="mat1" numRows="1" numColumns="1"/>` (unique names per part) inside `<answer name="...">` and grade with `<award><matrix><row>...</row></matrix></award>`. Include instruction: *"Use the r+/-, c+/- buttons to change the number of rows and columns."*
  * **Multiple Choice (Single):** Either `<choiceInput name="..."><choice name="...">...</choice></choiceInput>` graded with `<when>$choiceName.selected</when>`, or `<choice credit="1">` directly inside `<answer name="...">`.
  * **Multiple Choice (Select All):** `<answer name="..." selectMultiple><label>Select all that apply.</label><choice credit="1">...</choice></answer>`.
  * **Conditional Reveal:** `<group hide="not $c_uni.selected">...</group>` to show follow-up inputs conditionally.
* **Figures:** Reference hosted images at `https://githubusercontent.com<filename>.png` using `<image source="..." width="320px"><shortDescription>...</shortDescription></image>`.

## 4. Reference DoenetML Templates
```xml
<!-- Matrix input template -->
<problem>
  <p>Apply the elementary row operation <m>R_3 + 2R_2</m> to <m\(>\left[\begin{array}{rrr\|r} 1 & 4 & -1 & 2 \\ 0 & 1 & -3 & 5 \\ 0 & -2 & 7 & -6 \end{array}\right]</\)m>. Use the r+/-, c+/- buttons to change the number of rows and columns.</p>
  <answer name="rowOpAns1">
    <matrixInput name="rowOpMat1" numRows="1" numColumns="1"/>
    <award>
      <matrix>
        <row>1 4 -1 2</row>
        <row>0 1 -3 5</row>
        <row>0 0 1 4</row>
      </matrix>
    </award>
  </answer>
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
    <group hide="not \$c1_uni.selected">
      <p>Enter the unique solution:</p>
      <p><m>x = </m><mathInput name="x1"/>, <m>y = </m><mathInput name="y1"/></p>
    </group>
    <answer name="ans1">
      <award><when>\$c1_uni.selected and \(x1 = 1 and\)y1 = 2</when></award>
    </answer>
  </part>
</problem>

<!-- Select-all multiple choice template -->
<answer name="rrefCheck1" selectMultiple>
  <label>Select all that apply.</label>
  <choice><m>Incorrect</m></choice>
  <choice credit="1"><m>Correct</m></choice>
</answer>
```

## 5. Preamble (`tex/preamble.tex`)
```latex
\(\usepackage{graphicx} \usepackage{amsmath, amsthm, amssymb} \usepackage{mathtools} \usepackage{enumitem} \usepackage{pgfplots} \pgfplotsset{compat=1.18} \usepackage{mdframed} \usepackage{float} \usepackage{xcolor} \usepackage{tcolorbox} \usepackage{xurl} \usepackage{colortbl} \usepackage{hyperref} \hypersetup{ colorlinks=true, urlcolor=blue!70!black, linkcolor=purple!70!black } \usepackage\)[capitalise, noabbrev, nameinlink]{cleveref}

\(\theoremstyle{definition} \newtheorem{definition}{Definition} \newtheorem{example}{Example} \surroundwithmdframed[ hidealllines=true, leftline=true, linewidth=1.5pt, linecolor=gray!50, innerleftmargin=10pt, innerrightmargin=0pt, innertopmargin=2pt, innerbottommargin=2pt, skipabove=10pt, skipbelow=10pt ]{example} \newtheorem{activity}{Activity} \renewcommand{\theactivity}{\Alph{activity}}\)

\(\newcommand{\doenetlink}{\% \begin{tcolorbox}[ colback=gray!6, colframe=blue!60!black, boxrule=0pt, leftrule=3pt, arc=2pt, left=8pt, right=8pt, top=6pt, bottom=6pt, before skip=10pt, after skip=10pt ] \textbf{\href{#2}{#1}} \\ {\small\ttfamily \url{#2}} \end{tcolorbox}\% } \newmdenv[ backgroundcolor=gray!15, linecolor=black, linewidth=1pt, innertopmargin=10pt, innerbottommargin=10pt, userdefinedwidth=0.8\textwidth, align=center, nobreak=true, settings={\refstepcounter{calloutbox}}, frametitle={\textbf{Box \thecalloutbox}} ]{calloutbox} \newcounter{calloutbox} \renewcommand{\thecalloutbox}{\arabic{calloutbox}} \crefname{calloutbox}{box}{boxes} \Crefname{calloutbox}{Box}{Boxes}\)

\(\newenvironment{augmatrix}\){\(\% \left[\begin{array}\){@{}#\(1!{\color{gray!75}\vrule}r@{}\)}\(\% \)}\({\% \end{array}\right]\% }\)

\(\newenvironment{rowops}{\% \begin{array}{@{}l@{}}\% }{\% \end{array}\% } \%\%\)MAGIT_PARSER_PROTECT%%```
