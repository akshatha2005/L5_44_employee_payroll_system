# Lab 5: Reflection

## 1. Which issues were the easiest to fix, and which were the hardest?

The easiest were the Flake8 style issues and the simple Pylint warnings: removing the unused `math` import and the unused `statutory_buffer` variable, changing `== None` to `is None`, adding a final newline, wrapping a long line, and adding docstrings. Each was a small local edit with no effect on behaviour.

The hardest was `eval(deduction_expr)` in `compute_net_salary`. Replacing it was not a one-line change, because the function accepted an arbitrary string expression such as "base * 0.85". I had to change the function's interface to take a numeric `deduction_rate` and compute `base * (1 - rate)`, and update the caller. The mutable default argument and the bare `except: pass` also needed thought, since I had to decide what the function should return when an employee ID is unknown.

## 2. Did the tools report any false positives or misleading warnings?

Some warnings were only partly accurate. Pylint and Flake8 reported `base` as an unused variable in `compute_net_salary`, but it was only "unused" because it was referenced inside the string passed to `eval`, so the tools could not see the use. The real problem was `eval` itself, which Bandit flagged. Bandit's B324 warning about MD5 is also context dependent: MD5 is acceptable for a non-security checksum, but an employee identifier hash in a payroll system could be used for security, so switching to SHA-256 was the safer choice. The tools also missed things. The hardcoded `payroll_key` secret was not flagged, and the `%d` format that silently truncates decimal bonuses (350.75 logged as 350) was only reported as a style issue.

## 3. How could static analysis be integrated into a real development workflow?

I would run Flake8 and Pylint as pre-commit hooks so style and quality problems are caught before code is committed. Bandit would run in a CI pipeline (for example GitHub Actions) on every pull request, failing the build on high or medium severity findings. Teams can set a minimum Pylint score as a merge requirement and share one configuration file so everyone gets the same rules. Static analysis should support code review, not replace it, because the tools missed the hardcoded secret and the truncating format string.

## 4. What improvements did you observe in code quality, security and maintainability?

The Pylint score went from 5.32/10 to 10.00/10, Bandit went from 5 issues (2 High, 1 Medium, 2 Low) to none, and Flake8 went from 7 issues to none, with the program output unchanged (net salary still 4122.5). Security improved most: no more arbitrary code execution through `eval`, no shell injection through `os.system`, a stronger hash, a cryptographically secure token, and no secret stored in the source. Reliability improved because errors are no longer silently swallowed and the shared mutable default list is gone. Maintainability improved through docstrings, snake_case names, a `with` block for file handling, and a main guard so importing the module no longer runs the program.
