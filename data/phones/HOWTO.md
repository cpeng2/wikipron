Phones
======

A `.phones` file is a list of permitted phones; any pronunciation which is not
totally composed of the permitted phones will be filtered as a postprocessing 
step.

What they filter
----------------

There are several types of segments which should be filtered by a `.phones`
file:

-   Typos
-   Invalid IPA transcriptions (e.g., extra length indicated with /ːː/)
-   non-native segments (e.g., a transcription of *Bach* as ending in the
    voiceless velar fricative /x/)

When creating a `.phones` file for broadly transcribed data, the goal is often
to create something that approximates a list of phonemes. However, there may 
be segments that are properly considered pure allophones but appear in broad 
transcriptions. However, such segments may be quite frequent in the data and 
removing all pronunciations that contain them would greatly reduce the amount 
of available data. Therefore, we prefer to simply add a comment of the form 
`# Allophone of ...`; such annotations will ultimately be used improve 
Wiktionary itself.

Wiktionary has [transcription
guidelines](https://en.wiktionary.org/wiki/Appendix:English_pronunciation) for
many languages, which you can reference when building a `.phones` file; you can
also refer to the [Phoible](https://phoible.org/) inventories, but these often
line up poorly with the language-specific Wiktionary transcription guidelines.

How to submit a `.phones` file
------------------------------

We welcome user submissions for `.phones` files from linguists. Note that we use
the [fork and pull](../../CONTRIBUTING.md) model for contributions.

1.  Make a list of all phones or phonemes, in descending-frequency order, using
    the appropriate file in [`../scrape/tsv`](../scrape/tsv). The script
    [`list_phones.py`](lib/list_phones.py) is available to facilitate this
    step. Running `./list_phones.py ../tsv/<some-TSV-file> > foo.phones`
    generates `foo.phones` that you can edit by the following steps.
2.  Remove typos, invalid IPA transcriptions, and non-native segments. The
    `.phones` file generated by `list_phones.py` shows (as comments signaled by
    `#` for each phone/phoneme) the frequency of each phone/phoneme and several
    of its example word-pronunciation pairs, which should help you decide which
    phones/phonemes to remove. For the phones or phonemes to retain, remove the
    comments of counts and example word-pronunciation pairs.
3.  For a broadly transcribed list, add comments about allophony.
4.  Run [`postprocess`](postprocess).
5.  In [`../scrape`](../scrape) run `./scrape --restriction=<your-lang> &&
    ./postprocess`. This may take a while.
6.  Add the `.phones` file, the filtered `.tsv` file(s), and the summary files
    using `git add`. The `.phones` file must use the [NFC Unicode 
    normalization](https://en.wikipedia.org/wiki/Unicode_equivalence#Normalization).
    If you used `../src/list_phones.py` to create the `.phones` file, then it
    should be in this form already. Otherwise, in [`lib`](lib), you can run
    `./normalize.py <your-file> NFC` to put your file in the correct form.
7.  Commit using `git commit`, push to your branch using `git push`, and then
    file a pull request.

The `.phones` file format is a UTF-8 encoded file with one segment per line,
with optional comments formatted as two spaces, `#`, one space, and then a
sentence or sentence fragment with appropriate punctuation (e.g.,
`tʰ  # Allophone of /t/.`). Please include a blank line at the end of the file.
The `.phones` file should have the same name as the corresponding TSV file, but
with a `.phones` extension instead of `.tsv`.
