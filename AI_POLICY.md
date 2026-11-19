# AI usage policy

AI tools are allowed in Lleme. Using them is not a problem; hiding it
is. This policy applies to code, tests, documentation, issues and
review comments.

## Rules

1. **AI is allowed.** You may use any tool, for code, tests and
   documentation.

2. **Disclosure is required on every contributor pull request.** The
   pull request template has an "AI usage" section with three options:
   no AI tools were used, AI-assisted, or mostly AI-generated. For the
   last two, name the tool or tools. The `pr-check` job fails while the
   section is empty.

3. **A human answers for the code.** Whoever opens the pull request has
   read, understood and tested every change, and can explain any line
   of it. Replies during review are your own words, not text pasted
   from an assistant.

4. **No unattended agents.** Pull requests and issues opened by an
   agent with no human following them are closed.

5. **Large changes start with an issue.** The design is agreed before
   the code is written, with or without AI.

6. **Security reports need a reproduction.** Steps and evidence must be
   verified by the person reporting. See `SECURITY.md`.

7. **Origin of the code.** By signing the CLA you state that you have
   the right to submit the content, and that to the best of your
   knowledge it does not reproduce third-party code under an
   incompatible license. This holds whether or not a tool produced it.

8. **Consequences.** A pull request with no disclosure is sent back to
   be corrected. Deliberate omission, or repeatedly submitting code you
   have not reviewed, leads to the pull request being closed and, if it
   happens again, to a block.

## What the disclosure is for

The disclosure does not change the bar for acceptance. A change is
judged on what it does, how it is tested and whether it fits the
project. The disclosure tells the reviewer where to spend attention.

## What counts as AI usage

Disclose a tool when it wrote or rewrote code, tests or text that ends
up in the pull request. This includes chat assistants, coding agents
and multi-line editor completion.

You do not need to disclose single-line completion, spell checking,
translation of your own text, or using a tool only to search or to
read documentation.

When in doubt, disclose.

## Where the disclosure goes

In the pull request description, in the "AI usage" section. Do not add
tool-generated trailers or co-author lines to commit messages; see
`GIT_CONVENTIONS.md`.

## Maintainers

Maintainers use AI tools, and say so once, in `CONTRIBUTING.md`. That
statement is their standing disclosure, so the per pull request
section is not enforced for them. Every other rule in this policy
applies to maintainers as it does to contributors.
