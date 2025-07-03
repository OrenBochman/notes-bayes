
# Ideas to make coursera note taking consistent

this is a preliminary document. Some of the details should be copied from the notes themselves.

1. Many lessons are challenging -- my process it more or less as follows:
   - Create an outline for the module based on the titles of the videos etc.
   - Watch the video once
   - Watch again and Grab the screenshots. Putting these at the top of the section.
   - Write out the math in latex, and add it to the notes. LLM can speed this up ideas by a certain [Gilles Castel](https://castel.dev/post/lecture-notes-1/)
   - If the material is challenging, I will watch the video again and improve the notes.
   - If this is still not enough, I will grab the transcript. Unfortunately the instructors speak out random latex they lecture. This makes following their transcript challenging and makes translating these transcripts next to impossible. However the following process is what I do
     - use this regex `\\. \.\n` to split it into lines
     - look at lines that don't start with a capital letter and unsplit those.
     - use `[]{.mark}` to highlight important bits.
     - if possible use delimiters the math using a`$` sign. This is a generally  waste of time but with an LLM a few edits get things done quickly.
     - The transcripts are later useful for creating a podcast of the lessons using a tool like [podcastgen](https://huggingface.co/spaces/saq1b/podcastgen) or  Notebooklm() etc.
     - Two more points. often in this process I will recall something that I have read in the past, or gain some intuition about the topic and include it in the notes. The other case though is that the instructor in Coursera etc are brilliant researchers, but they may not be the best explainers for a specific topic. They may have learned tons of theory or other courses and this material may be just a special case of that. Let's just say that they are  looking down from the top of the mountain and we are still at a point where all we see are foothills and snow and a voice in  the wilderness saying "be inspired and rise to the top". In realty  though we can't do that so it  helps to look for better way to flatten the learning curve. When I was young and I would look for a book in the University library. There was always one that no matter how hard the class was that made things very clear. (And many that made things very very confusing). The confusing ones had their allure but the clear ones were worth the time to read through. Today though we have access to many more instructors via the internet. It it 10 to 100 times easier to find a better explanation of a topic online than it is to find and to read through a book, particularly as a big part of new mathematics is the initial absorption of new concepts, notation and the new structure of the formalism. Once you have those down, it becomes easier to understand -- and a video walking you through is a better medium for this than a book. Particularly if the interlocutor feels these concepts are his friends and the formalism is just a means to expressing certain intuitions.
     - Transcripts should be added via an import directive to reduce the size of the source files, particularly if you use an llm as a copilot to assist you.

## Goals

1. If the course has goals, these should be listed and then linked to in the relevant section. c.f. notes on RL regarding this.

## Navigation

1. This was originally a book. However I had issues building it with earlier versions of quarto and it still looks bad. At some point I converted it to a web site. Later I added profiles to support both a book and a website. However this requires lots of work and I have not completed this task.
1. The navigation should be consistent across all sections
    - Separate lectures from Exercises.
    - Exercises should be in separate docs to excluding them from the render to comply with the honor code.
    - Videos should be excluded from PDF renderers
1. CLT and the LLN are in appendixes so link to them when referencing these results. Also need to update these appendices as they don't have the requisite proofs
1. Notation should be listed in the first appendix and should be made consistent.
1. all figures should have a    `#fig-` prefix to their id, and should be unique across the document.
1. terminology should be indexed using `\index{term}` so that the index can be generated automatically for pdf. For see entries use `\index{term|see{main-term}}`.

## Environment

This course uses R and python code as well as latex and quarto.

1. Python environment should be reconstituted from `requirements.txt` and via pip
1. Likewise, the R environment should be reconstituted from the `renv.lock` file
1. For use with Codex a docker image should be created that has the requirements installed, and the files mounted as a volume so that they can be edited in the container.

## Code

- Code assignments should be tested via pytest or R `testthat`.
- MCMC can be approximate so these test need to allow for some tolerance.
- Code section should have comments extracted to an annotation list below the code block.

::: {.callout-tip }
## Latex Labels {.unnumbered .unlisted}

To get **latex** labels into the graph in `R` I used 
`xlab = expression(theta)` 
and 
`ylab = expression(paste("f(",theta,"|", x,")")),`

so expression is used to get the latex into the graph. 

~~This is useful for the pdf output, but not for the html output. For html output you can use `htmltools::HTML()` to get the same effect.~~
:::


## Bug fixes

There were many show stoppers developing this repo. I can't remember them all, but I think I stated document a bunch of them. Most were related to MCMC diagnostics taking too much space.

```r
#| label: fig-linear-regression-part1-6-2
mod_csim = do.call(rbind, mod_sim) # combine multiple chains
#autocorr.plot(mod_csim)
nvar <- ncol(as.matrix(mod_csim))
par(mfrow = c(nvar, 1))
par(oma = c(1, 1, 1, 1), mar = c(2, 2, 2, 1))  # tight margins
autocorr.plot(mod_csim, ask = FALSE, auto.layout = FALSE) # <1>
```

1. this failed due to the plot being too large. The other lines tried to fix it, but without auto.layout = FALSE R ignored these efforts and kept giving the same issue. The most annoying issue it that it worked in preview but not in render.

The fix is basically to stack the correlation plots vertically.

1. \bmatrix requires using  `\\` rather than `\newline`
1. In general it better to avoid `|` and use `\mid` as the former may be parsed as a markdown table under certain conditions.
1. alignment not using the `\aligned` environment
1. using the color gray requires `\usepackage[x11names]{xcolor}` and then using `\definecolor{grey}{rgb}{0.5,0.5,0.5}` to define the color.
1. use `\mathcal{N}` for the normal distribution, and `\mathcal{U}` for the uniform distribution.
1. use `\mathrm{Pois}` etc for other distributions.

1. `! LaTeX Error: Not in outer par mode. l.4846 \end{marginfigure}` This error is caused by trying to use a marginfigure in a non outer par mode. This means a image is being placed in the margin inside of a list table etc. THe quick for this fix is to move the image out of the environment or not to put it in the margin.

1. a line with a slash \  created a most cryptic error use l.6678  from index.log and goto that line in index.tex then seach for the location in qmd file to fix.
## indexing


### Entries and Subentries

`\index{main_index_ent}`

|Index Entry Type    |	LaTeX Expression |	Example                |
|--------------------|-------------------|-------------------------|
|Main index entry    |`\index{main_index_ent}`        |	`\index{scheduling}`|
|Sub-index entry     |`\index{main_index_ent!sub_ent}`|	`\index{scheduling!reg}`|
|Sub-sub-index entry |`\index{main_index_ent!sub_ent!sub_sub_ent}`|	`{\index{scheduling!reg!MS}`|

### Page range definitions

use `\index{\ldots\mid(}` `\index{\ldots\mid)}` to define a page range

### See notes

`\index{partitions!harddrive|see{harddrive, partitions}}`

### Different location and text  

`\index{string1@string2}`
where:

- string1 is used to set the location
- string2 is used to set the display text

```
Format: \index{partitions}
\index{partitions!harddrive|see{harddrive,partitions}}
The primary \index{SATA} disk drive on your personal Windows 7
computers is filling up. This drive contains the computer’s
\index{Windows!operating system} and application of software.
\index{harddrive}
\index{partitions!harddrive|see{harddrive, partitions}}
```

### Latex Tricks

1. this is how to add bold greek letters to indicate matrixes or vectors. Use `\boldsymbol` to get the bold greek letters. you need to add `\usepackage{amsmath}` to the preamble of your latex document.

```latex
\boldsymbol{\beta}
```

1. This is how you can use `vphantom` to [horizontaly align the text in the underbraces]{.mark}. I guess, it add a hidden box  with a sum from the second term to adjust the height of the underbrace.

```latex
\underbrace{y_1^2(1 - \phi^2)\vphantom{\sum_{t=2}^T (y_t - \phi y_{t-1})^2}}_{\text{Initial Loss}} 
+ \underbrace{\sum_{t=2}^T (y_t - \phi y_{t-1})^2}_{\text{Remaining Loss } Q(\phi)}

```

### references

- [creating an index in latex](https://latex-tutorial.com/creating-index-latex/)

### Some icons used in the notes

🎥 - Section on a video lecture
📖 - Section covering a reading handout etc
ℛ - Section covering R code
