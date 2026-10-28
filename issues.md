# Lab 5: Issues Found and Fixed

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| os.system with string concatenation | Security (Bandit B605, High) | 58 | Shell command built from a variable allows command injection | Removed the shell call and used print() |
| Weak MD5 hash | Security (Bandit B324, High) | 47 | MD5 is cryptographically broken | Replaced with hashlib.sha256 |
| eval() on a string | Security (Bandit B307 Medium, Pylint W0123) | 42 | Executes arbitrary code passed as text | Replaced with a numeric deduction_rate: base * (1 - rate) |
| random.randint for token | Security (Bandit B311, Low) | 51 | Predictable pseudo-random numbers | Used secrets.randbelow |
| Bare except with pass | Bug (Bandit B110, Flake8 E722, Pylint W0702) | 26-27 | Silently hides errors such as an unknown employee ID | Catch KeyError and return False |
| Mutable default argument | Bug (Pylint W0102) | 15 | transaction_logs=[] is shared across calls | Default to None and create the list inside the function |
| Hardcoded secret key | Security (not flagged by tools) | 62 | Secret stored in source code and printed | Read from PAYROLL_MASTER_KEY environment variable, no longer printed |
| %d log formatting | Bug (Pylint C0209, Flake8 E501) | 19 | Truncates decimal bonuses; line too long | f-string with :.2f, wrapped line |
| Comparison with == None | Style (Flake8 E711, Pylint C0121) | 32 | Should use identity check | Changed to is None |
| Unused import and variables | Style (F401, F841, W0611, W0612) | 6, 31, 41 | Dead code | Removed math and statutory_buffer; base is now used |
| open() without with | Pylint (W1514, R1732) | 55 | File not closed on error, no encoding | with open(..., encoding="utf-8") |
| Redundant if/else | Pylint (R1703, R1705) | 34 | Needless branching | Return the comparison directly |
| Naming, docstrings, final newline, main guard | Pylint (C0103, C0116, C0304), Flake8 W292 | 15, 73 and others | Style issues; main() ran on import | Renamed to process_salary, added docstrings, newline, and a main guard |

## Results
- Pylint: 5.32/10 before, 10.00/10 after
- Bandit: 5 issues (2 High, 1 Medium, 2 Low) before, 0 after
- Flake8: 7 issues before, 0 after
