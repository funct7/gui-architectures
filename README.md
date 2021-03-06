# GUI-Architectures

## Purpose

1. The primary purpose of this repo is to provide a handy reference for GUI architectures.
2. In doing so, the repo attempts to provide a widely-accepted/authoritative definition of terms.
3. Instead of providing simplistic examples that make any architecture seem like it can solve all problems, the repo will attempt to host real-life problems that people have had a hard time solving.

## Rationale

In doing research on GUI architectures, I've come to realize people/organizations tend to implement their own version of some architecture, which is not really what the original authors of the idea espoused. At the same time, people throw in words such as "two-way data binding", "business logic", including numerous others, which, upon being asked what the definition for such words are, they could only vaguely answer. Sometimes the words didn't seem to be as important as they seemed upon close inspection.

Not to say that all situations and terms can be defined clearly, I believe it is paramount to starting a discussion to agree on the definition of terms for the discussion to be productive.

This repo will strive to give an accurate summary of GUI architectures, with the different variants, providing simplifed and real-life examples for closer inspection.

This repo is primarily for personal reference, but if anyone wishes to contribute and share the knowledge, they are most welcome to do so.

## Rubric [WIP]

To provide an easy guide to the characteristics of each GUI architecture, this repo will use the following rubric:

- **Data duplication**: Does the architecture need copies of state across architectural components?
- **Logic duplication**: Does the architecture make users copy and paste code dealing with business logic/behavior?
- **Logic cohesiveness**: Does the architecture put related business/presentation logic closely together or are they spread across architectural components?
- **Responsibility separation**: Does the architecture mix different logic in a single component or are they clearly separated?

- Others:
  - Can architectural components be used only by looking at the interface, not the implementation?

Personal preference is architectures that *score low on data duplication and logic duplication* while getting *high scores on logic cohesiveness and responsibility separation*.
