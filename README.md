# Project Tofu

Beancount plugin that smooths expenses accounts

## Examples

### Depreciation

Example entries the user keep in their journal:

```beancount
2021-05-18 * "Belfield Music" "New guitar etc"
    Expenses:Hobbies                         2999.00 AUD
        policy: "depreciation"
        proxy: 2999.00 AUD
        periods: 3
        interval: "month"
        residual: 2000 AUD
        buffer: Assets:Equipment:ESP-Horizon
    Expenses:Hobbies                           15.00 AUD
        note: "Jim Dunlop Jazz III"
    Liabilities:Credit:AMEX-Pt

2021-05-20 * "Desky Desk" "New desk for home office" #desky-desk
    Expenses:Digital:Device                  1024.00 AUD
        policy: "depreciation"
        proxy: 1 DESKY
        periods: 2
        interval: "month"
        buffer: Assets:Equipment
    Liabilities:Credit:AMEX-Pt

2021-05-19 * "KFC" ""
    Expenses:Dining-Out                        12.80 AUD
    Liabilities:Credit:AMEX-Pt
```

Effective entries after applying `tofu` policy `depreciation`:

```beancount
2021-05-18 * "Belfield Music" "New guitar etc"
    Assets:Equipment:ESP-Horizon             2999.00 AUD
    Expenses:Hobbies                           15.00 AUD
        note: "Jim Dunlop Jazz III"
    Liabilities:Credit:AMEX-Pt

2021-05-19 * "KFC" ""
    Expenses:Dining-Out                        12.80 AUD
    Liabilities:Credit:AMEX-Pt

2021-05-20 * "Desky Desk" "New desk for home office" #desky-desk
    Assets:Equipment                            1 DESKY {1024.00 AUD, 2021-05-18}
    Liabilities:Credit:AMEX-Pt

2021-06-01 * "Expense recognised"
    Assets:Equipment:ESP-Horizon             -333.00 AUD
    Expenses:Hobbies                          333.00 AUD

2021-06-01 * "Expense recognised" #desky-desk
    Assets:Equipment                            1 DESKY {512.00 AUD, 2021-05-18}
    Assets:Equipment                           -1 DESKY {1024.00 AUD, 2021-05-18}
    Expenses:Digital:Device                   512.00 AUD

2021-07-01 * "Expense recognised"
    Assets:Equipment:ESP-Horizon             -333.00 AUD
    Expenses:Hobbies                          333.00 AUD

2021-07-01 * "Expense recognised" #desky-desk
    Assets:Equipment                           -1 DESKY {512.00 AUD, 2021-05-18}
    Expenses:Digital:Device                   512.00 AUD

2021-08-01 * "Expense recognised"
    Assets:Equipment:ESP-Horizon             -333.00 AUD
    Expenses:Hobbies                          333.00 AUD
```

## Project Maturity

1 - Ideation

Maturity is is a linear scale that attempts to report the readiness of the
artifact (project/script/etc.) for use by the intended end-users (e.g. the
public).

```[ 1 - Ideation ] -> [ 2 - Prototype ] -> [ 3 - Alpha ] -> [ 4 - Beta ] -> [
5 - Generally Releasable ]```

- *Ideation* is the first phase, and simply means the artifact is still just a
  bunch of ideas. There is likely not even any code, or other implementation
yet.
- *Prototype* is the second phase, and means implemenation has begun (e.g.
  code) but is very rough, likely examples and other snippets which haven't
been designed very well.
- *Alpha* is the third phase, and means much of the core functionality has been
  implemented. The artifact is usable, but likely has limitations/restrictions
and bugs that need to be fixed.
- *Beta* is the fourth phase, and means we almost have a finished artifact! The
  focus now should shift to testing and polishing activities.
- *Generally Releasable* is the fifth (and final) phase, and means we are
  'done'! At least to the intended definition of the artifact. This doesn't
mean changes won't happen in the future, but simply means the artifact is
mature enough that the target end-users are able to use it fully and with
reasonable expectation that it won't crash/fail.

