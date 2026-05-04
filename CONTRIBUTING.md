# Contributing

Thanks for wanting to make this repo better.

## What I will merge

Pull requests welcome for:

- **Typo and grammar fixes** in any of the docs.
- **Factual corrections** -- a command flag changed, a URL moved, a Python or Node version is out of date, an LLM provider deprecated a model, the A2A spec moved on a field name.
- **Additional troubleshooting entries** for problems you hit while working through the book that are not yet covered. Match the existing format: symptom, cause, fix, with a code block where it helps.
- **Clarity edits** to `SETUP.md` if a step tripped you up. If it tripped you up, it will trip up the next reader too.

## What I will not merge

- **Changes to the canonical solution shape.** The code at each `chapter-N-final` tag matches the listings printed in the book. If I accept a PR that restructures the `chapter-4-final` code, every reader following along sees a mismatch between the book and the repo. If you think the canonical shape is wrong, email `contact@yaw.sh` and we can talk about it for the next book revision.
- **New checkpoints or new tools** not covered by the book.
- **Style-only refactors** of the reference code (renaming variables, swapping libraries, reformatting). The shape is intentional.
- **Swapping the default LLM provider or A2A transport** at a tag. The book picks specific defaults to make the prose concrete; alternatives are discussed in the chapter, not at the tag.

## Filing an issue

If you found a real bug in the reference code -- something that does not work as the book says it should -- open an issue with:

1. Which tag you are on (`chapter-4-final`, etc.).
2. Your Python and/or Node version.
3. The exact command you ran.
4. The output you got, including any error message.
5. What you expected.

That gives me enough to reproduce it without a back-and-forth.

If a tag you need does not exist yet, file an issue with the chapter number and a short note on what you're trying to do. Tags fill in by reader pull -- the more readers ask for one, the sooner it lands.

For book content questions (a chapter is unclear, a concept did not land), email `contact@yaw.sh` rather than filing an issue here. The issue tracker is for repo bugs and tag prioritization.
