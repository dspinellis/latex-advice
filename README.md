# Advice for writing LaTeX documents

- [Introduction](#introduction)
- [Put the document under version control](#put-the-document-under-version-control)
- [Write readable and maintainable LaTeX source code](#write-readable-and-maintainable-latex-source-code)
- [Automate the document's build](#automate-the-document-build)
- [Use Continuous Integration](#use-continuous-integration)
- [Automate the management of bibliographic references](#automate-the-management-of-bibliographic-references)
- [Use style files](#use-style-files)
- [Use third-party LaTeX packages](#use-third-party-latex-packages)
- [Avoid explicit formatting](#avoid-explicit-formatting)
- [Mathematics](#mathematics)
- [Use symbolic references](#use-symbolic-references)
- [Tables](#tables)
- [Figures](#figures)
- [Floats](#floats)
- [Typography](#typography)
- [LaTeX formatting](#latex-formatting)
- [Enforce all of the above](#enforce-all-of-the-above)
- [See also](#see-also)
- [License](#license)


## Introduction
If you're writing a scientific book, a paper, or a thesis in
computer science, engineering, mathematics, physics or a related
field, it pays to write it using [LaTeX](https://www.latex-project.org/about/),
especially if your work contains
formulas, symbols, and heavy cross referencing.
Here is advice for doing so, collected over decades of writing
[hundreds of papers and books](https://www.spinellis.gr/pubs/index.html),
mostly using LaTeX.

Note that this list is not intended to provide advice on
English writing style, scientific writing, or TeX programming.
There are many other excellent guides for all these topics.

Contributions via *GitHub* pull requests are always welcomed.

## Put the document under version control
* Create a repository for your document and add to it all the
  document's source code.
  This can live on *GitHub* if you plan to work with others or want
  to keep a copy on another server, or it can be a local Git repository
  you create with `git init`.
* Do not include in the repository generated files, such as
  the formatted PDF document,
  compiled bibliographies,
  or PDF charts generated from R or Python scripts.
  Instead, add *Makefile* rules to automatically create these files
  at build time.
  Generated files needlessly increase the size of the repo,
  making it time-consuming to clone and fetch updates.
  They also pollute difference listings with immaterial changes.
* Commit your changes to the repository in logical chunks using
  [meaningful commit messages](https://chris.beams.io/posts/git-commit/).
* When you remove text, delete it completely, rather than commenting
  it out.
  The version control system will keep track of it and your document
  will remain clean.
* Tag the revisions you submit using annotated tag objects (`git tag -a`).
  This allows you to go back to specific versions when going over review comments,
  or when starting a version of a new publication venue.
  Push the tags to the online repository you're using so that all can see them
  (`git push --tags`).

## Write readable and maintainable LaTeX source code
* Treat the LaTeX source code with love and care as you would treat
  software code.
  You aim is to keep the source code readable and maintainable.
* Avoid long lines, splitting text somewhere between column 60 and 70.
  (You can configure your editor to do this for you).
* Start each phrase on a separate line, and further split a phrase
  according to its logical structure.
  Here is an example.
```
Obtaining metrics from large code bodies is difficult
for technical and operational reasons~\cite{Moc09,GS13}.
On the technical side,
code dependencies make it difficult to establish
the full context needed in order to parse and semantically analyse the code.
This is especially true for C code,
where the compilation depends on
system header files,
compiler-defined macros,
search paths, and
compile-time flags passed through the build process~\cite{Spi03r,LKA11,GG12}.
The operational reasons are associated with the required throughput,
though due to the relatively small number of releases we examined,
this was not a major issue in this study.
```
* Splitting lines at the level of phrases and a sentence's logical structure
  offers you the following advantages.
  * It gives clean line differences of changes made between versions;
    a line change corresponds at most to a single phrase.
    (You can obtain difference listings on *GitHub*, or via `git diff`.)
    Clean differences make it easier to see what has changed and to
    merge changes of others when working as a group.
  * It makes it easy to rearrange the order of sentences or the elements
    in a list, simply by moving lines around.
  * It gives you with a second (a structural) view of your document,
    which nicely complements the flowed typeset view that LaTeX generates.
    This provides you with additional opportunities to spot and fix
    syntax, grammar, and style mistakes.
    For example, this allows you to spot inconsistent or repetitive
    structure in a paragraph's sentences.
* Use comments (sequences starting with `%`) to indicate the key idea of each
  paragraph or section.
* Use comment keywords, such as `XXX` or `TODO`, to indicate places
  where more work is needed.
* Write LaTeX code and configure your editor so that you can
  fold sections and paragraphs.
  This shall allow you to inspect your document's structure as an outline.
  If needed, add fold marks in comments (e.g. `% {{{2`) to facilitate this.
  For example, if your document's structure is in sections and subsections,
  these would be level-1 and level-2 folds.
  You would then start each paragraph with a level-3 fold comment,
  such as the following.
```
% Advantages of model-based development {{{3
```
* Avoid splitting short documents (such as a journal or conference paper)
  into multiple files (e.g. `introduction.tex`, `methods.tex`, `results.tex`,
  `conclusions.tex`).
  Such splitting makes it difficult to work perform a number of tasks with
  your editor:
  * overview the material's composition
    (e.g. the balance of the sections' length),
  * globally search or search and replace a particular term, and
  * move matter from one section to another.
  
  When the document is split, these tasks require you to switch from one
  file to another or to use external tools.
  Feel free to split longer documents (such as a book or a thesis) in order
  to shorten a project's long build and loading time.

## Automate the document build
Ensure that the document can be built with a single command,
and that files that are out of date are appropriately rebuilt.
This saves you from repeatedly executing multiple commands,
makes it easier to work as a team, and
avoids the accidental use of outdated files.
You have four options here.
* You can use the Unix *make* command.
  It's available out of the box, but expressing the need for
  multiple passes over your document is difficult,
  so you may end up processing the document more often than needed.
* A second alternative is
  [latexmk](https://www.ctan.org/pkg/latexmk),
  which comes bundled with most LaTeX distributions.
  See [this StackExchange answer](https://tex.stackexchange.com/a/40759/10140)
  for a complete example.
* A third alternative is the use of
  [BSD Owl](https://github.com/michipili/bsdowl) scripts,
  as documented
  [here](https://github.com/michipili/bsdowl/blob/master/doc/LaTeXDocument.md).
  Both *latexmk* and *BSD Owl* will help you create clean and functional
  build setups.
  Choose the one that is easier to install on your system and matches your
  taste.
* A fourth alternative is
  [SCons](https://scons.org/).
  SCons' configuration files, typically named `SConstruct`,
  are Python scripts.
  One such is demonstrated
  [here](https://tex.stackexchange.com/a/26573/8272).
  You may find a short overview of supported commands/packages
  [here](https://github.com/SCons/scons/wiki/LatexSupport).

## Use Continuous Integration

Once you have automated your build, you are ready for the next step:
continuous integration (CI) for LaTeX documents.
Executing an automated build on every commit allows you to
easily spot accidentally forgotten auxiliary files such as images or tables.
You can also easily isolate changes that broke the build and that are
sometimes hard to debug later.
Moreover, CI creates a defined way to build your project,
on which every collaborator can rely on.

Thanks to [travis-ci-latex-pdf](https://github.com/harshjv/travis-ci-latex-pdf),
there is a ready-made solution for [Travis CI](http://travis-ci.org/)
that you just need to copy into your project to get started.

Another alternative involves integrating the
[ChkTeX](http://www.nongnu.org/chktex/) linter into the build process.
This checks documents for certain LaTeX anti-patterns, such as the use of
`...` instead of the typographically correct `\dots` or other such violations.
Users can define their own rules, too, for example to enforce consistency
in the spelling of certain phrases.
Now it is up to you to create ChkTeX rules to enforce the guidelines in this
document.
If you do, please share them.

## Automate the management of bibliographic references
* Create one or more centrally-managed bibliography files for your
  work, and list those in a `\bibliography` command in all documents you write.
* Using [BibTeX](https://en.wikipedia.org/wiki/BibTeX) or
  [Biber](https://en.wikipedia.org/wiki/Biber_(LaTeX)) you can then
  automatically create the document's list of references in the style
  prescribed by the publisher.
*  Use `\cite` commands to reference specific bibliographic entries.
* If you need to reference a specific page or chapter, include this information
  in square brackets before the key.
  For example, write: `… educational use~\cite[p.~8]{LR89}.`
* If your publisher requires in-text author-year citations, use
  [BibLaTeX](http://mirrors.ibiblio.org/CTAN/macros/latex/contrib/biblatex/doc/biblatex.pdf)
  or the
  [natbib](https://www.ctan.org/pkg/natbib?lang=en) package in conjunction
  with the `\citet`, `\citep`, and `\citeauthor` commands.
  For example, write:
  * `\citet{jon90} proposed …` to get `Jones et al. (1990) proposed …`
  * `Another argument \citep{jon90} …` to get
    `Another argument (Jones et al., 1990) …`.
  * `As \citeauthor{jon90} argued …` to get
    `As Jones argued …` (e.g. on a subsequent mention in the same paragraph).
* Include in your BibTeX entries all required fields and as many of the
  optional fields as possible. Copy and consult
  [this file](https://github.com/dspinellis/latex-advice/blob/master/schema.bib)
  to remember which fields are available, which are required, and which
  are supported for each entry type.
* Obtain a BibTeX entry for a publication's DOI using the
  [doi2bib](https://www.doi2bib.org/) service.
  In many cases the quality of its results is higher than that provided
  by the publisher's digital library.
* If you copy-paste a BibTeX entry from a digital library,
  edit it carefully to ensure a consistent high-level of quality.
  * Write the title using
    [title capitalization](http://grammarist.com/style/title-capitalization/),
    e.g. write `The Elements of Programming Style`, rather than
    `The elements of programming style`.
    This ensures that the title will appear correctly
    if a particular bibliography style requires title capitalization.
  * Put a title's characters that should always be capitalized in braces,
    e.g. `The {C} Programming Language`, or `Fifty Years of {M}oore's Law`.
    This ensures these characters will not be converted to lowercase
    in bibliography styles that do not require title capitalization.
  * Use a consistent format for referring to conferences, for example,
    `{ICSE} '08: Proceedings of the 30th International Conference on Software Engineering`.
    It would be a mistake to use a different format, e.g.
    `12th Working Conference on Mining Software Repositories (MSR 2015), Proceedings of` in another entry.
  * Don't include a URL for entries that have a DOI.
  * Specify an em-dash or en-dash where required, e.g.
    `The Entity-Relationship Model---Toward a Unified View of Data` or
    `The Evolution of {C} Programming Practices: A Study of the {U}nix Operating System 1973--2015`.
  * Use LaTeX special characters for authors whose names contain non-ASCII
    characters, e.g.
    `author = {Yann-Ga\"{e}l Gu\'{e}h\'{e}neuc and Herv\'{e} Albin-Amiot}`.
    This change may not be required if you are using
    [Biber](https://en.wikipedia.org/wiki/Biber_(LaTeX)), provided that
    the digital library correctly provides the author names in Unicode.
* Use consistent, easily derivable, short names for your bibliography entries.
  One practical scheme is as follows.
  * The single author's surname first three letters, followed by the
    last two year digits, e.g. `Ker08` for an article published
    by `Brian Kernighan` in 2008.
  * Up to four multi-author surname initials followed by the last two year
    digits, e.g. `DMG07` for an article published by
    `Duvall, Paul M. and Matyas, Steve and Glover, Andrew` in 2007.
  * The letter`a`, `b`, `c`, etc appended to the above keys in the case
    of clashes.

  Through this scheme you can easily search for an existing entry in your
  bibliography files and saved articles, because you can quickly derive
  the key used.
  Also this scheme will help you avoid duplicate entries, because they
  corresponding keys will clash.
* If you save the PDF document associated with a bibliography entry in
  your files, name it using the entry's key, e.g. `LR89.pdf`.
* If you print (or photocopy) a document associated with a bibliography
  entry, write the corresponding key on its top right corner.
  This will help you reference it in your work.
  You can also use this key to file the printout so that you can find it
  in the future.
* If you prefer to edit your BibTeX files with a desktop application
  rather than an editor, consider using [JabRef](https://www.jabref.org/).

## Use style files
Conference organizers and publishers often supply style files that
determine the appearance and formatting of a document and its references.
Download them and use them.
Seeing your document in its published look
boosts your motivation,
helps you appreciate how your readers will experience it, and
minimizes rework by forcing you to comply with the publisher's
requirements while you develop your document.

## Use third-party LaTeX packages
You can obtain many formatting goodies by incorporating into
your document third-party LaTeX packages.
Most LaTeX distributions provide a package management system
that simplifies the installation and maintenance of packages.
Here are some popular LaTeX packages your may want to know about.
* [algorithmicx](https://www.ctan.org/pkg/algorithmicx): Display good-looking pseudocode
* [amsmath, amssymb](https://www.ctan.org/pkg/amsmath): AMS mathematical facilities
* [amsthm](https://www.ctan.org/pkg/amsthm): Typesetting theorems (AMS style)
* [booktabs](https://www.ctan.org/pkg/booktabs): Publication quality tables
* [cite](https://www.ctan.org/pkg/cite): Improved citation handling
* [fancyhdr](https://www.ctan.org/pkg/fancyhdr): Extensive control of page headers and footers
* [geometry](https://www.ctan.org/pkg/geometry): Flexible and complete interface to document dimensions
* [hyperref](https://www.ctan.org/pkg/hyperref): Extensive support for hypertext
* [listings](https://www.ctan.org/pkg/listings): Typeset source code listings
* [minted](https://www.ctan.org/pkg/minted): Typeset source code listings with highligthing
* [natbib](https://www.ctan.org/pkg/natbib): Flexible bibliography support
* [PGF/TikZ](https://www.ctan.org/pkg/pgf): Create PostScript and PDF graphics
* [setspace](https://www.ctan.org/pkg/setspace): Set space between lines
* [siunitx](http://www.ctan.org/pkg/siunitx): A comprehensive (SI) units package
* [url](https://www.ctan.org/pkg/url): Verbatim with URL-sensitive line breaks
* [xcolor](https://www.ctan.org/pkg/xcolor): Driver-independent color extensions
* [xspace](https://www.ctan.org/pkg/xspace): Define commands that appear not to eat spaces
* [cleveref](https://www.ctan.org/pkg/cleveref): Intelligent cross-referencing

## Avoid explicit formatting
* Avoid the use of explicit formatting within the text.
  Instead, use commands that specify why your text should be formatted
  in a special way.
  For example, rather than specifying a large bold font, use
  `\section` and rather than specifying italic text use `\emph`.
* If your document requires that particular text should be formatted
  in a special way, declare a new command or environment to format it.
  This will allow you to easily adjust the formatting throughout the
  document.
  For example,
  the following command sets a document's *finding* in boldface
```
\newcommand{\finding}[1]{\textbf{#1}}
```
  while the following environment declaration will list
  a software's license in a small font keeping its formatting.
```
\newenvironment{license}{\verbatim\scriptsize}{\normalsize\endverbatim}
```

## Mathematics
* Set all math, including formulas, stand-alone math symbols,
  and negative numbers in LaTeX's math mode.
  See the following example, which includes a little-known formula
  and in-text references to its elements.
```
\[
E = mc^2
\]
This formula states that the equivalent energy ($E$)
can be calculated as the mass ($m$) multiplied
by the speed of light ($c$) squared.
```
* Use balanced `\left` and `\right` commands to markup (balanced)
  bracketing elements.
  This ensures that the elements will be correctly sized, according
  to the formula they include.
  For example, write `\left(\cos(x) + i \sin(x)\right)^n`.
* Use special markup for character sequences math mode.
  LaTeX assumes an implicit multiplication in character sequences it
  encounters in math mode, and sets the text with corresponding spacing.
  * If the characters indicate one of
  [LaTeX-supported operators](https://en.wikibooks.org/wiki/LaTeX/Mathematics#Operators)
  then use that operator command, e.g. `\mod`, `\max`, or `\sin`.
  * If the characters indicate an unsupported operator,
    declare it using the *amsmath* command `\DeclareMathOperator`,
    e.g.  `\DeclareMathOperator{\sha}{sha}`.
  * If the characters indicate a multi-character variable name,
    set it in italics or roman using e.g. `\mathit{Delta}` or
    `\mathrm{Delta}`.
* Indent elements and break lines in order to express
  the logical structure of the formula you are writing.
  For example, write:
```
F_{n} =
  \cfrac{1}{\sqrt{5}} \left(\cfrac{1 + \sqrt{5}}{2}\right)^n
  - \cfrac{1}{\sqrt{5}} \left(\cfrac{1 - \sqrt{5}}{2}\right)^n
```
  Keep the operators as the first character on a line.
* If you write a lot of math in LaTeX, acquaint yourself and follow
  A.J. Hildebrand's
[Top Ten List](http://www.math.illinois.edu/~ajh/tex/tips-topten.html).

## Use symbolic references
*  Mark sections, tables, figures, and equations using the `\label` command,
   and reference them using the [cleveref](https://www.ctan.org/pkg/cleveref)
 `\Cref` command.
* Avoid referencing floating environments by name
  (e.g. `see Figure~\ref{fig:foo}`);`\Cref` can create that for you:
  `see~\Cref{fig:foo}`.
* Use self-descriptive label names.
  For example, use `tab:statResults` rather than  `table1`.
  Use the `tab:` prefix when labelling tables,
  the `fig:` prefix  when labelling figures, and
  the `sec` prefix  when labelling sections.

## Tables
* Avoid the use of rules (`\hline` and `|`) in tables.
  They are unsightly and this is the reason you will rarely see them
  in professionally typeset books.
* Specify the alignment of numbers to the right,
  text to the left, and single symbols on the center.
* When listing numbers with a decimal point align them on the decimal
  point.
  You can [easily](https://tex.stackexchange.com/a/2747/10140)
  do this by using the [siunitx](http://www.ctan.org/pkg/siunitx) package.
* Use hard tabs before each column's `&` separator to start each column's
  data on the same LaTeX source document column.
  See the following example.
```
tr -cs  & 1     & \X    & --    & \V    & 1 \\
sort w  & 0     &       & --    & \X    & 0 \\
fmt     & 1     & \X    & --    & \V    & 1 \\
tr A-Z  & 1     & \V    & 1     &       & 1 \\
sort -u & fmt   & \X    & --    & \V    & 1 \\
```
* If the column labels are too long, start them on separate lines,
  keeping the columns aligned with the data.
  Example:
```
% Column labels
Command and options
        & Input requirements
                & Matched
                        & Connected
                                & Matched
                                        & Connected \\
\hline
% Data
tr -cs  & 1     & \X    & --    & \V    & 1 \\
sort w  & 0     &       & --    & \X    & 0 \\
...
```
* If you want horizontal rules, use `\toprule`, `\midrule` and
  `\bottomrule` from the `booktabs` package.
  Example:
```
\toprule
Header \\
\midrule
Table row 1 \\
Table row 2 \\
\bottomrule
```

## Figures
* Use vector rather than bitmap images.
  So, avoid including a `.jpeg` or `.png` image if you can include
  a PDF created from vector data, e.g.  by direct export from
  [Inkscape](https://inkscape.org/en/) or
  [GraphViz](http://graphviz.org/) or by converting an `.svg` file.
* Use the same font in all your figures.
  This does not need to be the same font as the one you use for the document's
  text, it could be a sans-serif one, but it should be the same e.g.
  in charts and diagrams.
* Ensure figure fonts are scaled to match the size of the document's
  font.
  Eyeballing is often enough.
  For exact measurements you have two options.
  If you scale the figure by an explicit amount (e.g. 1 or 0.5) you
  should use a font of the corresponding size (e.g. 11 points or 22 points).
  If you scale the figure by an arbitrary amount to match the document's
  column width, , you can load the resultant PDF in Inkscape and measure the
  font size there.

## Floats
* Allow large displayed elements (figures, tables, listings) to have
  a floating position by using a *figure* or *table* environment.
* Put floats in your document before your first reference to them.
* Use `\centering` within a figure or table environment to center its
  contents.
  This avoids the extra space introduced by the `center` environment.
* Don't sweat too much about float positioning while writing.
  If your write for a journal, this will be handled during production;
  if you will provide a camera-ready version, leave this for the last
  minute.

## Typography
* Be consistent in the rules you follow.
  Some style guides offer contradicting advice,
  e.g. regarding the placement of footnotes before or after punctuation
  or the spacing around the *em-dash*.
  This isn't a license to randomly use both the one or the other
  alternative within the document.
  Adopt your publisher's style or consistently follow the same rule
  throughout your document.
* Use three dashes (`---`) to create the *em-dash*,
  which separates parenthetical phrases.
  According to the *Chicago Manual of Style* (2.13),
  an em-dash has no space before or after it.
  (Note that *The Associated Press Stylebook* specifies spacing.)
* Use two dashes (`--`) to create an *en-dash*.
  Use this to specify number ranges, e.g. `2009--2015`.
* Use `\dots` to produce an ellipsis (`...`) punctuation symbol.
* Put footnotes *after* punctuation symbols, unless your publisher's
  style specifies otherwise.
  For example, write `… is true.\footnote{See also …}`
* Avoid underlined text.
  Underlining is used for emphasis in handwritten text and was also
  carried over in this capacity in typewritten documents.
  With modern printers and software you don't need to underline,
  because you can use a different font style (bold or italic) for emphasis.
* Avoid sequences of uppercase text.
  Text written in all-uppercase characters is an eyesore and
  considerably more difficult to read than its lowercase equivalent.
  Lowercase letters have been designed for easy reading,
  uppercase were originally designed for easy chiseling on stone.
* Use small caps rather than an unsightly sequence of capital letters
  for writing abbreviations.
  For example, write `\textsc{sql}`, rather than `SQL`.
  Note that some publishers do not follow this style.
  If you are supplying the final camera ready version
  (e.g. for conference proceedings), use small caps.
  If a publisher will edit your document and the publisher does not use
  small caps, avoid their use;
  a common frustrating mistake is for publishers to remove the
  `\textsc` command, without writing the text in uppercase.
* Display pseudo-code using the
  [algorithmicx](https://www.ctan.org/pkg/algorithmicx)
  package and environment.
  See [this wikibooks](https://en.wikibooks.org/wiki/LaTeX/Algorithms)
  entry for more details.
* Display source code snippets using the
  [listings](https://www.ctan.org/pkg/listings) (simpler)
  or the [minted](https://www.ctan.org/pkg/minted)
  (nicer, but with extra dependencies) package.
  Make sure your code does not go over the paragraph length;
  if it does, adjust line breaking and/or font size accordingly.

## LaTeX formatting
* Don't spend too much time on formatting your document.
  Concentrate on communicating your ideas and on making your document
  maintainable.
  Excessive formatting tweaks can be counterproductive:
  you waste time implementing them and
  your publisher may find it more difficult to use your document.
* Use a tilde before `\cite` `\ref` etc. to avoid a line break immediately
  before the reference.
  For example, write `Some also use logging statements~\cite{Spi06e}`.
* Put a backslash after a non-sentence ending period to ensure this will
  not be followed by the period-ending spacing.
  For example, write `In 1962 Watson et al.\ famously found …`
* Do not use math mode to achieve typographic effects in plain text.
  For example write  `RQ\textsubscript{2}` rather than `$RQ_2$`, if you
   must subscript the number of your research question 2.
  Similarly use `\textsuperscript` for plain superscripted text (e.g.
  Wisper Quiet\texsuperscript{TM}).
* Match opening and closing quotes using one or two single-opening
  (`‘` or `‘‘`) and single-closing (`’` or `’’`) quote characters.
  Do not use the keyboard's double quote symbol (`"`).
  Alternatively you can use the `\enquote{}` macro from the `csquotes`
  package, which will automatically set the right quotes for the
  configure language and can handle nested quotes correctly.
* Use the new local font style commands
  (e.g. `\texttt{}`, `\textsc{}`, or `\textbf{}`)
  rather than the old switches (e.g. `{\tt }`, `{\sc }`, `{\bf }`)
* To obtain a PDF document with the widely available Times/Helvetica/Courier
  font set, rather than the LaTeX, less widely used, Computer Modern fonts,
  use the following packages.
```
\usepackage{mathptmx}
\usepackage[scaled=.90]{helvet}
\usepackage{courier}
```

## See also
* A.J. Hildebrand. [LaTeX Tips: Basic tips](http://www.math.uiuc.edu/~ajh/tex/basics.html)
* Wikibooks. [LaTeX/Tips and Tricks](https://en.wikibooks.org/wiki/LaTeX/Tips_and_Tricks)
* John Regehr. [John's Small Collection of LaTeX Tips](https://john.regehr.org/latex/)
* Mark Trettin (author of German version) and Jürgen Fenn (English translation).
  [An essential guide to LaTeX2ε usage: Obsolete commands and packages](https://ctan.org/tex-archive/info/l2tabu/english/)
* Martin J. Osborne [Some common (la)tex errors](https://www.economics.utoronto.ca/osborne/latex/LTXERR.HTM)
* Diomidis Spinellis. [The Elements of Computing Style](https://www.computingstyle.com/)
* StackExchange Academia. [Does submitting a paper in Latex format enhance the chances of acceptance?](https://academia.stackexchange.com/questions/82641/does-submitting-a-paper-in-latex-format-enhance-the-chances-of-acceptance)
*  Knauff M, Nejasmic J (2014) An Efficiency Comparison of Document Preparation Systems Used in Academic Research and Development. PLoS ONE 9(12): e115069. https://doi.org/10.1371/journal.pone.0115069

## License
[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
