
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
    - The transcripts are later useful for creating a podcaset of the lessons using a tool like [podcastgen](https://huggingface.co/spaces/saq1b/podcastgen) or  Notebooklm() etc.

   - It seems prudent to review the transcript and highlight the bits that are important. Unfortunately the instructors read out the math as they teach. This makes extracting these details quite difficult. I find that in such cases it helps to start with the transcript, write out the math in latex and then put the important non math bits in the notes. This is rather laborious but it my process at this time. It lets me review the math in depth and fill in any gaps in the derivations.
1. Transcripts should be added via an import directive to reduce the size of the source files.

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
1. terminology should be indexed using `\index{term}` so that the index can be generated automatically for pdf. For see entries use `\index{term|\see{main-term}}`.

## Environment

This course uses R and python code as well as latex and quarto.

1. Python environment should be reconstituted from `requirements.txt` and via pip
1. Likewise, the R environment should be reconstituted from the `renv.lock` file
1. For use with Codex a docker image should be created that has the requirements installed, and the files mounted as a volume so that they can be edited in the container.

## Code

- Code assignments should be tested via pytest or R testthat.
- MCMCM can be approxiate so these test need to allow for some tolerance.
- Code section should have comments extracted to an annotation list below the code block.

