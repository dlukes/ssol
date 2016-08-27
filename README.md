Summer School of Linguistics 2016
=================================

Python for linguists: An introductory phrasebook
------------------------------------------------

Course materials and additional information. The notebooks with the code from
the respective sessions are available in the `notebooks` folder. Download them
and open them from within Jupyter (start Jupyter from the Anaconda Navigator).
Some of them (particularly the later ones) have been extended with additional
commentary to make them more self-explanatory. Much of the material **borrows
examples and code from the [NLTK Book](http://www.nltk.org/book/)**, which is
hereby **gratefully acknowledged**.

### Setup

1. get the installer for the Python programming environment we'll be using
   (Anaconda):
  - either on the flash drive under `anaconda/<your operating system>`
  - or via <https://www.continuum.io/downloads> (Python 3.5, 64-bit)
2. copy the `nltk_data` folder into your user's home folder (e.g.
   `C:\\Users\<your username>` on Windows)
3. (pass the flash drive on to the next person)
4. **run the Anaconda installer**
5. (we'll do this together) install the `regex` package

If need be, don't be shy and ask for help!

### Regular expressions

See day 4 notebook for a guided tour of regular expressions.

Code for an interactive regex matcher inside a Jupyter notebook:

```python
import regex as re
import IPython.core.display as ipd
import ipywidgets as ipw

@ipw.interact(regex=ipw.Text(), string=ipw.Textarea())
def findall(dotall=False, multiline=False, ignorecase=False, only_first=False, regex="", string=""):
    if not (regex and string):
        ipd.display(ipd.HTML(""))
        return None
    flags = 0
    if dotall:
        flags |= re.DOTALL
    if multiline:
        flags |= re.MULTILINE
    if ignorecase:
        flags |= re.IGNORECASE
    start = '<span style="background-color: gold">'
    end = "</span>"
    offset_bump = len(start) + len(end)
    offset = 0
    html = string
    matches = []
    for m in re.finditer(regex, string, flags):
        matches.append(m.captures()[0])
        span = m.span()
        sstart, send = span[0] + offset, span[1] + offset
        html = html[:sstart] + start + html[sstart:send] + end + html[send:]
        offset += offset_bump
        if only_first:
            break
    ipd.display(ipd.HTML("<p>regex: <strong>" + regex + "</strong></p>" + "<pre>" + html + "</pre"))
    return matches
```

Summary of regex syntax based on the [NLTK Book](http://www.nltk.org/book/ch03.html):

```
.         # Wildcard, matches any character
\w   \W   # Matches any (non-)word character (careful, the
          # computer's idea about what a word character is might
          # be different from yours)
\d   \D   # Matches any (non-)digit character
\s   \S   # Matches any (non-)space character
\p{...}   # Matches any character with Unicode property ...
\P{...}   # Matches any character without Unicode property ...
^abc      # Matches some pattern abc at the start of a string
          # (or line, if the multiline flag is enabled)
abc$      # Matches some pattern abc at the end of a string
          # (or line, if the multiline flag is enabled)
\babc\b   # Matches some pattern abc surrounded by word boundaries
\Babc\B   # Matches some pattern abc not surrounded by word boundaries
[abc]     # Matches one of a set of characters
[A-Z0-9]  # Matches one of a range of characters
ed|ing|s  # Matches one of the specified strings (disjunction)
*         # Zero or more of previous item, e.g. a*, [a-z]* (also
          # known as Kleene Closure); greedy (match as many as
          # possible)
*?        # The same as *, but non-greedy (match as few as possible)
+         # One or more of previous item, e.g. a+, [a-z]+; greedy
+?        # The same as + but non-greedy
?         # Zero or one of the previous item (i.e. optional), e.g.
          # a?, [a-z]?
{n}       # Exactly n repeats where n is a non-negative integer
{n,}      # At least n repeats
{,n}      # No more than n repeats
{m,n}     # At least m and no more than n repeats
a(b|c)+   # Parentheses indicate the scope of the operators and
          # capture the corresponding groups of characters, which
          # are then accessible accessible with the match.group()
          # match.groups() method
a(?:b|c)+ # Non-capturing version of the parentheses
```
