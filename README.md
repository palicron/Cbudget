# Cbudget

A personal budgeting application for Windows 11, built as a **learning project to practise modern C++**.

I am a gameplay programmer with five years of C++, mostly in Unreal Engine. Cbudget is my first desktop application. It is a deliberate exercise in standard C++ (outside an engine) and in how real desktop software is structured.

## What it is

Cbudget tracks expenses, income, and investments, and projects where my finances are heading. Everything runs locally: financial data stays on the PC, and no account or subscription is needed.

## Purpose

- **Practise standard C++23:** ownership and RAII, value types, `std::expected` error handling, `std::chrono` dates, overflow-safe integer arithmetic, and API design.
- **Learn desktop application architecture:** event-driven UI instead of a game loop, separating UI from logic, and a clean boundary between languages.
- **Work like a professional team:** a written design, a step-by-step plan, test-first development, and code review on every change.
- **Build a tool I actually use** instead of paying for a commercial budgeting app.

## Planned features

**Entries and categories**
- Record expenses and income with category, account, date, and note.
- Multiple currencies (main currency COP). Each foreign entry keeps the original amount, the exchange rate used, and the converted amount. Rates come from free sources (the official Colombian TRM for USD) and can be overwritten with the amount on the bank statement.
- Spending per category, monthly budget vs actual, month-over-month change, fixed vs variable expenses.
- Recurring items (rent, salary, subscriptions) and transfers between accounts.

**Investments**
- Term deposits (CDT) and other interest-earning investments: value at maturity or at any date, gross and net of withholding tax, and real return after inflation.

**Indicators and projections**
- Monthly income vs spending, savings rate, net worth over time.
- Runway: how many months I could live without income.
- "What will I have at date X": current balances and investments plus monthly savings, compounded.

**Import**
- Import bank statement files (CSV / OFX).

**Out of scope for version 1:** live bank API sync, cloud sync, mobile access, multiple users.

## Architecture

```
UI:       C# + WinUI 3 (XAML)      thin layer: displays data, forwards user actions
Interop:  extern "C" API (DLL)     plain C boundary, called from C# via P/Invoke
Core:     standard C++23 library   all logic: money, ledger, investments, projections, storage
```

- The **core** uses only ISO C++ and the standard library (plus SQLite), with no Windows headers. It is built and tested on its own.
- Money is stored as integer minor units, never floating point.
- Errors are returned as values (`std::expected` / `std::optional`), never thrown across the C boundary.

## Tech stack

| Area | Technology |
|---|---|
| Core language | C++23 (MSVC, Visual Studio 2026) |
| UI | C#, WinUI 3 (Windows App SDK) |
| Interop | `extern "C"` DLL + P/Invoke |
| Storage | SQLite (local file) |
| Tests | doctest |
| Dependencies | vcpkg (manifest mode) |
| Formatting | clang-format |
| Platform | Windows 11 |

## Status

In early development. The design and the plan for the C++ core are done; implementation starts with the core library and its tests.

| Milestone | State |
|---|---|
| Design | done |
| 1. C++ core (money, ledger, investments, indicators, projections) | in progress |
| 2. Storage (SQLite) | planned |
| 3. Interop DLL | planned |
| 4. WinUI 3 application | planned |
| 5. Bank statement import | planned |

## License

[MIT](LICENSE)
