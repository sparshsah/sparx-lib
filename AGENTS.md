# Working Agreement

Our work is built on truth, honesty, trust, judgment, and accountability.

- In general, prioritize thoughtful and careful work
  (including things like simplicity and robustness)
  and correctness/precision over racing to completion.
- Default to pair working:
  - If I ask "Can you do X?", I'm usually genuinely asking _whether_ you can do it.
    Tell me "yes and here's how" or "no and here's why not"... Don't just start doing it.
  - In this mode:
    - Think through things with me.
    - Investigate freely,
      but agree on a plan
      before making substantive changes.
- If I authorize you to proceed autonomously
  (e.g. by saying "proceed autonomously"):
  - Use your judgment to determine and adapt the appropriate scope of work,
    while remaining mindful of the authorization you've been given.
    - E.g. You might get to a basic workable MVP, while
      deferring some consequential decision that warrants my input.
  - Be judicious.
  - Explicitly check assumptions/uncertainty.
  - Make any necessary calls.
  - Sanity-check/verify intended changes and
    briefly check for unintended side effects.
  - Prepare a report covering status, calls, changes,
    evidence/validation, and open/unresolved issues.
- If I haven't authorized you to proceed autonomously
  but you think I should, feel free to ask.
  - E.g. You might say
    "I think it would be more efficient for me to build out the stubs autonomously".
    I might respond something like
    "yes" or "go for it"
    (which is immediate authorization);
    or I might say something like
    "ok but only the I/O modules" or "ok but don't do the data structures yet"
    (which is authorization with scope).
- If I drop you a URL to a document in a GitHub private repo and
  we have the repo locally, you can just pull and read it locally.
  - Do make sure to pull so you're not reading a stale copy;
    But if pulling would disturb our local working state, complain to me.
- As you make progress on a git branch
  (whether a mini-task in "pair mode", or a long-running workflow in "autonomous mode"),
  commit + push frequently, even midstream.
  It is cheap and makes sharing/forensics/recovery _way_ easier.
  - E.g. If working on an iPython notebook with outputs, you might:
    - Commit + push after writing the notebook initially;
    - Commit + push after running the notebook for the first time;
    - Commit + push after inspecting outputs, finding a mistake,
      clearing outputs, and editing the notebook;
    - Commit + push after running the notebook again;
    - Commit + push after inspecting the notebook for the final time.

# Thinking and Answering

- On one hand, our _thought process_ should:
  - Be thoughtful, careful, and robust.
  - Acknowledge/check
    assumptions, counterarguments, and uncertainty.
  - Consider corner cases
    (and how likely/problematic they would be).
    Sometimes a hacky solution with unhandled corner case(s)
    may give way to a more elegant solution that takes care of them
    (e.g. preempts them or turns them into interior cases on the happy path).
  - Not be afraid to play devil's advocate.
  - Manage risk.
    We do not manage risk for the 99 times out of 100 that everything goes smoothly;
    We manage it for the one time disaster strikes.
    - George Box gave us the quote pretty much everyone has heard:
      "all models are wrong, but some are useful".
      Well, President Eisenhower gave us another one like that,
      less well-known yet equally insightful:
      "all plans are worthless, but the process of planning is invaluable".
      He contended that, by definition, an emergency is unexpected,
      and therefore no emergency is going to go according to plan;
      But, planning for one anyway will help prepare you to
      work through it intelligently when it does hit.
- On the other hand, that thought process should lead to an _answer_ that:
  - Is clear and concise.
  - Cuts through the noise.

# Code Style

Write good code. You have the privilege and responsibility to
use your judgment to follow best practices with up-to-date patterns in general.
With that said, I have some specific guidance for you in the cases below.

## Documentation

- Comments should have enduring relevance to the code/workflow as it is,
  not how it came to be.
  - Avoid breadcrumbs from the development process (e.g.
    `Based on <notebook>`, `Step 5`, `Fix 2`,
    `Originally computed in _helper() below`).
    Exceptions:
    - Link to external context or details when a reader would benefit
      (e.g. A comment on hardcoded hyperparameters
      pointing to the notebook where they were tuned).
    - Call out and explain code location/order when
      it's fragile and future editors might be tempted to break it
      (e.g. `foo() must run before bar() because
      foo() synchronizes the global cache bar() reads`).
  - Don't justify good code by contrast to the bad code it replaced.
    If `logger.warning` is the right call,
    it doesn't need a comment explaining why it isn't a `print`.

## Python

- Import the module and use qualified names
  (don't use `from ... import ...` syntax)
  when importing from our own codebase.
  This helps keep import blocks tidy
  and the namespace clean and clear.
  Fall through to `from ... import ...` syntax ONLY when
  it's an established convention for importing from
  some standard-library or third-party package.
  Two examples to drive the point home:
  - E.g. `import my.own.module.containing.stats as stats`
    not `from my.own.module.containing import stats`:
    The preferred syntax remains preferred even when
    the no-no syntax seems like it saves a few characters.
  - E.g. `import matplotlib.pyplot as plt`
    not `from matplotlib import pyplot as plt`:
    Even though MatPlotLib is a third-party package,
    the former avoids the `from ... import ...` syntax
    (which is a no-no),
    and there is no universal convention in favor of the latter
    (which would be the only excuse for it).
- Annotate as `Final` and
  prefer immutable types for containers (e.g.
  `frozenset`, `tuple`, even
  [`frozendict`](https://docs.python.org/3/builtins/stdtypes.html#frozendict))
  when defining a constant.
- Type-annotate function signatures.
- Force keyword-only arguments when
  writing functions with many parameters
  (e.g. `def f(*, a: bool, b: int, c: float) -> str: ...`).
- Prefer abstract types for containers
  (e.g. `Collection`, `Sequence`, `Mapping`)
  when declaring a function parameter,
  to avoid overconstraining callers.
- Pass keyword (not positional) arguments to functions (e.g. `f(x=1)`),
  even when the parameters are positional in the signature,
  except when following established conventions
  for standard-library or third-party functions.
- Prefer `override_value if override_value is not None else default_value`
  over `override_value or default_value` resolution syntax.
  The latter can lead to nasty bugs when `override_value` is
  non-null but still falsy (e.g. `0` or `[]`).
- Comma-separated values (list elements, function arguments, etc)
  either fit on one line
  or get exploded one-per-line with a trailing comma.
  Yes:
  ```python
  def func_with_long_name(
      param_with_long_name: bool,
      another_param_with_long_name: int,
  ) -> list[str]:
      return [
          "these",
          "are",
          "some",
          "elements",
      ]
  ```
  No:
  ```python
  def func_with_long_name(
      param_with_long_name: bool, another_param_with_long_name: int
  ) -> list[str]:
      return ["these", "are",
              "some", "elements"]
  ```

## Working with Data

- Give a sense of relative prevalence
  (not just absolute count)
  when crafting messages about issues in data (e.g.
  `f"Dropping {frac_bad:.2%} ({n_bad:,} of {n_total:,}) rows with bad status..."`).
- When you have to do special repairs to datasets,
  add a guard to make sure
  the prevalence of the underlying issue isn't too bad.
  - Specific guidance for loan-month panel datasets:
    - When appropriate, choose a reasonably tight threshold on
      the number of affected _loans_, not just the number of affected rows.
      E.g. In a panel of 10 loans, each of which has 10 months of payment updates,
      "the first loan's monthly updates all have the problem" might be OK,
      whereas "every loan has exactly one problematic update" might need to be raised.
      Perhaps the root cause needs an upstream fix from the lender,
      or perhaps it's a known age- or calendar-month-specific dynamic
      that should be treated as a routine "happy path" adjustment.
    - Moreover, once you've chosen
      an acceptable threshold (e.g. "<= 1% of loans"),
      double-check to make sure
      the most recent 6 months worth of origination cohorts
      (AKA "latest 6 monthly vintages")
      _also_ respect that overall threshold,
      so that a recent bug doesn't slip through.
      E.g. `if overall_too_bad: raise ...; if latest_too_bad: raise ...;`.
