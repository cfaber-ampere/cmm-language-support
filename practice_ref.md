# PRACTICE Script Language — Crystallized Cheat Sheet

> Minimal, high-signal reference for writing PRACTICE (*.cmm) scripts.

## Core concepts
- **Scripts** run with `DO file.cmm` or `RUN file.cmm` (clears old PRACTICE stack first).
- **Lines** execute sequentially unless altered by control-flow commands.
- **Blocks** use parentheses `(` ... `)` for multi-line bodies.
- **Macros** (variables) are referenced with `&name`.

## Script structure essentials
```practice Documents/PRACTICE/practice_ref/practice_ref_crystallized.md
; comment

DO subscript.cmm arg1 arg2
ENTRY &arg1 &arg2

IF &arg1=="foo"
(
    PRINT "matched"
)
ELSE
    PRINT "not matched"

ENDDO
```

## Macros (variables)
- **GLOBAL &name**: visible everywhere; persists for session.
- **LOCAL &name**: visible within current block and nested blocks/subroutines/scripts.
- **PRIVATE &name**: visible only within declaring block and sub-blocks.

Recommended: start scripts with explicit macro declarations.
- `PMACRO.EXPLICIT` enforces explicit `GLOBAL/LOCAL/PRIVATE`.
- `PMACRO.IMPLICIT` ends explicit-enforcement range.

## Control flow
### Conditional
```practice Documents/PRACTICE/practice_ref/practice_ref_crystallized.md
IF <condition>
(
    ; ...
)
ELSE IF <condition>
(
    ; ...
)
ELSE
(
    ; ...
)
```
- `IF` must be followed by a space.

### Loops
```practice Documents/PRACTICE/practice_ref/practice_ref_crystallized.md
WHILE <condition>
(
    ; ...
)

RePeaT 10.
(
    ; ...
)

RePeaT
(
    ; ...
)
WHILE <condition>
```

### Jumps
- `GOTO <label>`: local jump within current script.
- `JUMPTO <label>`: global jump, clears stack nesting.

Labels must start at column 1 and end with `:`.

## Subroutines
### Preferred style (SUBROUTINE + PARAMETERS + RETURNVALUES)
```practice Documents/PRACTICE/practice_ref/practice_ref_crystallized.md
SUBROUTINE InitMem
(
    PRIVATE &addr
    PARAMETERS &addr
    ; ...
    RETURN "TRUE()"
)

GOSUB InitMem "0x10000"
RETURNVALUES &ok
```

### Label-based style
```practice Documents/PRACTICE/practice_ref/practice_ref_crystallized.md
GOSUB mySub 0x100 10. "abc"
...
mySub:
    ENTRY &address &len &string
    RETURN
```

## Parameter passing
- **ENTRY**: fetch arguments in scripts or label-subroutines.
- **PARAMETERS**: fetch string parameters (recommended with SUBROUTINE).
- **RETURN / RETURNVALUES**: return results to caller.

## Output
- `PRINT` — plain output (no formatting decoration).
- `ECHO` — decorated output (hex prefix, etc.).
- `PRINTF` — printf-style formatted output.
- `SPRINTF` — formatted output to a macro.

## Input
- `ENTER` — line or token-based input from AREA window.
- `INKEY` — read a single keystroke.

## GUI/AREA control (minimal)
- **AREA windows** are text I/O panes; default is `A000`.
- `AREA.Create <name>` — create AREA window.
- `AREA.Select <name>` — route PRINT/ECHO/INPUT to AREA.
- `AREA.view [<name>]` — show AREA window.
- `AREA.RESet` — reset AREA system (close I/O routing).
- `WinPOS <x> <y> <w> <h> , , , <name>` — position/name a window.
- `WinCLEAR <name>|TOP` — close a window.
- `WinExt` / `WinResist` — pre-commands to externalize or resist window closing.

## File I/O
```practice Documents/PRACTICE/practice_ref/practice_ref_crystallized.md
OPEN #1 file.txt /Read
READ #1 %LINE &line
CLOSE #1

OPEN #2 out.txt /Create
WRITE #2 "Hello" &value
CLOSE #2
```
- `APPEND` appends text similarly to `PRINT`.
- `WRITEB` writes binary data (requires `/Binary` on `OPEN`).

## Script execution control
- `STOP` — pause script (resume with `CONTinue`).
- `CONTinue` — resume stopped script (optionally to line/file).
- `ENDDO` — return from current script.
- `END` — terminate all PRACTICE scripts, clear local stack.

## Command reference (minimal)
### A–D
- `APPEND <file> <args...>` — append formatted text to file.
- `BEEP` — host acoustic signal.
- `CLOSE #<n>` — close file.
- `DECRYPT <key> <enc> [out]` — decrypt file (paired with ENCRYPT).
- `DO <file> [args...]` — run script; use `ENTRY` to fetch args.
- `DODECRYPT <key> <enc> [args...]` — run encrypted script.

### E–L
- `ECHO [{%attr}] [{%format} <data>...]` — formatted output with decoration.
- `ELSE [IF <cond>]` — conditional else branch.
- `ENCRYPT <key> <src> [out]` — encrypt file.
- `ENCRYPTDO <key> <src> [out]` — encrypt script for DODECRYPT.
- `ENCRYPTPER <key> <src> [out]` — encrypt PER file.
- `ENTER ...` — read input from AREA window to macros.
- `ENTRY ...` — fetch parameters for scripts/subroutines.
- `GLOBAL {&mac}` — create global macros.
- `GLOBALON <event> [action]` — global event handlers.
- `GOSUB <subroutine|label> [args...]` — call subroutine.
- `GOTO <label|line>` — local jump.
- `IF <condition>` — conditional.
- `INKEY [&mac]` — wait for key (optionally store code).
- `JUMPTO <label>` — global jump clearing stack nesting.
- `LOCAL {&mac}` — create local macros.

### M–S
- `ON <event> [action]` — scoped event handlers.
- `OPEN #<n> <file> [/Read|/Write|/Create|/Append|/Binary]` — open file.
- `PARAMETERS {&mac}` — fetch string parameters for subroutine/script.
- `PBREAK.*` — manage script breakpoints:
  - `PBREAK.Set`, `.Delete`, `.ENable`, `.DISable`, `.List`, `.RESet`, `.ON`, `.OFF`.
- `PEDIT <file> [line]` — open PRACTICE editor.
- `PLIST [label|line]` — list current script with breakpoints.
- `PMACRO.*` — macros/stack control:
  - `.list`, `.EXPLICIT`, `.IMPLICIT`, `.IMPLICITPRIVATE`, `.LOCK`, `.UNLOCK`, `.RESet`.
- `PRINT [{%attr}] [{%format} <data>...]` — plain output.
- `PRINTF [{%attr}] "<fmt>" [data...]` — printf-style output.
- `PRIVATE {&mac}` — private macros.
- `PSKIP` — skip current line/block during debug.
- `PSTEP [file [args...]]` — single-step mode (optionally start script).
- `PSTEPOUT` — run callee to return to caller in step mode.
- `PSTEPOVER` — step over callee in step mode.
- `READ #<n> [%LINE] &mac...` — read tokens or line from file.
- `RePeaT <count> <cmd>` / `RePeaT (<block>)` — loop; or `RePeaT ... WHILE`.
- `RETURN [values...]` — return from subroutine.
- `RETURNVALUES &mac...` — fetch return values.
- `RUN <file> [args...]` — run script and clear old stack.
- `SCREEN.*` — screen updates:
  - `SCREEN.ALways`, `.display`, `.OFF`, `.ON`, `.WAIT`.
- `SPRINTF &mac "<fmt>" [data...]` — formatted output to macro.
- `STOP [msg...]` — stop script (resume with CONTinue).
- `SUBROUTINE <name> ( ... )` — define subroutine.

### T–Z
- `WAIT [condition] [period] [/RunTime]` — wait for condition/timeout.
- `WHILE <condition> ( ... )` — loop.
- `WRITE #<n> [%CONTinue] <args...>` — write text to file.
- `WRITEB #<n> [%Byte|%Word|%Long|%BE|%LE] <data>` — write binary.

## Debugging helpers
- `PSTEP` / `PSTEPOVER` / `PSTEPOUT` — single-step controls.
- `PLIST` — list script with breakpoints.
- `PBREAK.*` — manage PRACTICE breakpoints.

## Events (advanced)
- `ON <event> <action>` — scoped event handling.
- `GLOBALON <event> <action>` — session-wide event handling.

## Expressions, literals, and operators (minimal)
- **Booleans**: `TRUE()` / `FALSE()`.
- **Comparisons**: `==`, `!=`, `<`, `<=`, `>`, `>=`.
- **Boolean ops**: `&&` (and), `||` (or), `!` (not).
- **Arithmetic**: `+`, `-`, `*`, `/`.
- **Numbers**: hex `0x10`, binary `0y1010`, decimal `10.` (note trailing dot).
- **Strings**: double quotes, e.g. `"text"`.
- **Addresses**: commonly `P:0x1000`, `A:0x1000`, etc. (target-specific).
- **Ranges**: `0x1000--0x1FFF` (start--end), `0x1000++0x100` (start++size).

## Common functions (frequent basics)
- `STATE.RUN()` — TRUE if target running.
- `FILE.EOF()` / `FILE.EOFLASTREAD()` — end-of-file checks.
- `FOUND()` — TRUE if last command found something (e.g., breakpoints, searches).
- `DATE.TIME()` / `DATE.UnixTime()` — time helpers.

---

## Notes for AI-assisted coding
When asking for help, include:
- target file name (.cmm)
- goal (what to automate)
- relevant macros/inputs/outputs
- expected TRACE32 commands or side effects
