# Advice for writing LaTeX documents
If you're writing a scientific book, a paper, or a thesis in
computer science, engineering, mathematics, physics or a related
field, it pays to write it using [LaTeX](https://www.latex-project.org/about/),
especially if your work contains
formulas, symbols, and heavy cross referencing.
Here is advice for doing so, collected over decades of writing
[hundreds of papers and books](https://www.spinellis.gr/pubs/index.html),
mostly using LaTeX.
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

## Write readable LaTeX source code
* Treat the LaTeX source code with love and care.
* Avoid long lines, slitting text somewhere between column 60 and 70.
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
  If needed, add folding comments (e.g. `{{{2`) to facilitate this.

## Automate the document's build
Ensure that the document can be built with a single command,
and that files that are out of date are appropriately rebuilt.
This saves you from repeatedly executing multiple commands,
makes it easier to work as a team, and
avoids the accidental use of outdated files.
Although you can use the Unix *make* command for this purpose,
a better alternative is *latexmk*, which comes bundled with
most LaTeX distributions.
See [this StackExchange answer](https://tex.stackexchange.com/a/40759/10140)
for a complete example.

## Manage bibliographic references using BibTeX
* Create one or more centrally-managed bibliography files for your
  work and list those in a `\bibliography` command in all documents you write.
*  Use `\cite` commands to reference specific bibliographic entries.
* If you need to reference a specific page or chapter, include this information
  in square brackets before the key.
  For example, write: `… educational use~\cite[p.~8]{LR89}.`
* If you save the PDF document associated with a bibliography entry in
  your files, name it using the entry's key, e.g. `LR89.pdf`.
* If your publisher requires in-text author-year citations, use the
  [natbib](https://www.ctan.org/pkg/natbib?lang=en) package in conjunction
  with the `\citet` and `\citep` commands.
  For example, write
  `\citet{jon90}` to get `Jones et al. (1990)` and
  `\citep{jon90}` to get `(Jones et al., 1990)`.

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
YOu can obtain many formatting goodies by incorporating into
your document third-party LaTeX packages.
Most LaTeX distributions provide a package management system
that simplifies the installation and maintenance of packages.
Here are some popular LaTeX packages your may want to know about.
* [amsmath, amssymb](https://www.ctan.org/pkg/amsmath): AMS mathematical facilities
* [amsthm](https://www.ctan.org/pkg/amsthm): Typesetting theorems (AMS style)
* [booktabs](https://www.ctan.org/pkg/booktabs): Publication quality tables
* [cite](https://www.ctan.org/pkg/cite): Improved citation handling
* [doublespace](https://www.ctan.org/pkg/doublespace): Double space environment
* [fancyhdr](https://www.ctan.org/pkg/fancyhdr): Extensive control of page headers and footers
* [geometry](https://www.ctan.org/pkg/geometry): Flexible and complete interface to document dimensions
* [hyperref](https://www.ctan.org/pkg/hyperref): Extensive support for hypertext
* [listings](https://www.ctan.org/pkg/listings): Typeset source code listings
* [natbib](https://www.ctan.org/pkg/natbib): Flexible bibliography support
* [PGF/TikZ](https://www.ctan.org/pkg/pgf): Create PostScript and PDF graphics
* [pslatex](https://www.ctan.org/pkg/pslatex): Use PostScript fonts by default
* [url](https://www.ctan.org/pkg/url): Verbatim with URL-sensitive line breaks
* [xcolor](https://www.ctan.org/pkg/xcolor): Driver-independent color extensions
* [xspace](https://www.ctan.org/pkg/xspace): Define commands that appear not to eat spaces

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

## Set all math in math mode
* Set all math, including both formulas and stand-alone math symbols,
  in LaTeX's math mode.
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
* If you write a lot of math in LaTeX, acquaint yourself and follow
  A.J. Hildebrand's
[Top Ten List](http://www.math.illinois.edu/~ajh/tex/tips-topten.html).

## Use symbolic references
Mark sections, tables, figures, and equations using the `\label` command,
and reference them using the `\ref` command.

## Typography
* Avoid the use of rules (`\hline` and `|`) in tables.
  They are unsightly and this is the reason you will rarely see them
  in professionally typeset books.
* Use vector rather than bitmap images.
  So, avoid including a `.jpeg` or `.png` image if you can include
  a PDF created from vector data, e.g.  by direct export from
  [Inkscape](https://inkscape.org/en/) or
  [GraphViz](http://graphviz.org/) or by converting an `.svg` file.
* Use three dashes (`---`) to create the *em-dash*,
  which separates parenthetical phrases.
* Use two dashes (`--`) to create an *en-dash*.
  Use this to specify number ranges, e.g. `2009--2015`.
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
* Use small caps for writing abbreviations.
  For example, write `\textsc{sql}`, rather than `SQL`.

## LaTeX formatting
* Use a tilde after `\cite` `\ref` etc. to avoid a line break immediately
  before the reference.
  For example, write `Some also use logging statements~\cite{Spi06e}`.
* Put a backslash after a non-sentence ending period to ensure this will
  not be followed by the period-ending spacing.
  For example, write `In 1962 Watson et al.\ famously found …`
* Match opening and closing quotes using one or two single-opening
  (`‘` or `‘‘`) and single-closing (`’` or `’’`) quote characters.
  Do not use the keyboard's double quote symbol (`"`).


## See also
* A.J. Hildebrand. [LaTeX Tips: Basic tips](http://www.math.uiuc.edu/~ajh/tex/basics.html)
* Wikibooks. [LaTeX/Tips and Tricks](https://en.wikibooks.org/wiki/LaTeX/Tips_and_Tricks)
* John Regehr. [John's Small Collection of LaTeX Tips](https://john.regehr.org/latex/)
* Diomidis Spinellis. [The Elements of Computing Style](https://www.computingstyle.com/)
* StackExchange Academia. [Does submitting a paper in Latex format enhance the chances of acceptance?](https://academia.stackexchange.com/questions/82641/does-submitting-a-paper-in-latex-format-enhance-the-chances-of-acceptance)

## License
[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
