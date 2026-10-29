# JESUS + NOTHING = EVERYTHING

Software note for `JESUS.bas`, a line-numbered BASIC program. It states the equation, keeps the scripture citations as comments, evaluates the equation under Bywater BASIC, prints the result, and returns to the shell.

The program result is the single line `JESUS`.

## Files

| File | Role |
| --- | --- |
| `JESUS.bas` | The program. |
| `README.md` | This document. |

## Requirements

The verified interpreter is Bywater BASIC, banner version **2.20 patch level 2**.

On the machine used to check this program, that interpreter came from the Debian package:

| Item | Value |
| --- | --- |
| Package | `bwbasic` |
| Package version | `2.20pl2-17` |
| Architecture | `amd64` |
| Binary | `/usr/bin/bwbasic` |
| Upstream | [bwbasic on SourceForge](http://sourceforge.net/projects/bwbasic/) |
| Standards | A large superset of ANSI Minimal BASIC (X3.60-1978) and a subset of ANSI Full BASIC (X3.113-1987) |

`JESUS.bas` depends on Bywater BASIC behavior:

- A filename on the command line is loaded and run. You do not type `RUN`.
- String variables end in `$`.
- `+` concatenates strings.
- `REM` comments may contain quotation marks and the infinity sign.
- `SYSTEM` closes the interpreter and returns to the shell.

Other BASIC interpreters are outside this document. GW-BASIC, QBasic, BBC BASIC, and YaBASIC do not share this command-line and `SYSTEM` behavior.

## Install Bywater BASIC 2.20 patch level 2

Debian, Ubuntu, and other apt-based systems. This is the install that was used to run the program.

```bash
sudo apt update
sudo apt install bwbasic
```

Confirm the package and the banner. The banner must say `version 2.20 patch level 2`. At the `bwBASIC:` prompt, type `QUIT`.

```bash
dpkg -l bwbasic
bwbasic
```

`dpkg` on the verification machine reports:

```text
ii  bwbasic  2.20pl2-17  amd64  Bywater BASIC Interpreter
```

The interactive banner is:

```text
Bywater BASIC Interpreter/Shell, version 2.20 patch level 2
Copyright (c) 1993, Ted A. Campbell
Copyright (c) 1995-1997, Jon B. Volkoff
```

If `apt` does not ship `2.20pl2`, install that upstream version and put `bwbasic` on `PATH`. The package homepage is http://sourceforge.net/projects/bwbasic/.

## Run

From this directory:

```bash
bwbasic JESUS.bas
```

From any working directory, pass the file path:

```bash
bwbasic /path/to/JESUS.bas
```

The file is a BASIC source file. Run it with `bwbasic`. Executing `./JESUS.bas` in the shell does not start the interpreter.

`SYSTEM` on line 90 ends the session. The shell prompt returns on its own.

## Expected output

`bwbasic JESUS.bas` prints the interpreter banner, a blank line, the program result, and a final blank line. The process then exits.

```text
Bywater BASIC Interpreter/Shell, version 2.20 patch level 2
Copyright (c) 1993, Ted A. Campbell
Copyright (c) 1995-1997, Jon B. Volkoff

JESUS

```

Exit status is `0`.

Bywater BASIC 2.20pl2 writes a carriage return at the start of each banner line. In a terminal those returns are invisible. A byte capture of the banner lines looks like `\r` plus the text above. The program result line is the ASCII characters `JESUS` followed by a newline. `REM` lines, including the scripture, produce no output.

## What the program computes

The source keeps the original equations as `REM` lines. Bywater BASIC cannot execute them as written:

- `LET` assigns to one variable. `LET JESUS + NOTHING = EVERYTHING` is an equation, so load stops with `Assignment must be to variable`.
- `NOTHING` is tokenized as the operator `NOT` followed by `HING`.
- `∞` is not a legal variable name.
- `END` would return to the `bwBASIC:` prompt and leave the process running. Line 90 uses `SYSTEM` instead.

The executable lines are the same proof in legal Bywater BASIC. `NIL$` is the empty string, which is nothing.

| Lines | Effect |
| --- | --- |
| 11–13 | `JESUS$ = "JESUS" + NIL$`, then `EVERYTHING$ = JESUS$`. Adding nothing leaves `JESUS`. |
| 31 | `JESUS$ = JESUS$ + NIL$`. Adding 0 leaves the same value. |
| 41, 61 | `JESUS$ = EVERYTHING$`. The remaining value is `JESUS`. |
| 70 | `PRINT JESUS$` writes `JESUS`. |
| 90 | `SYSTEM` exits the interpreter. |

Lines 15–20, 35, 45, 51–54, 65, and 66 are comments. They cite Romans 1:17, Romans 3:21–22, 2 Corinthians 5:21, Philippians 3:9, Matthew 6:33, Psalm 103:12, Hebrews 13:8, Colossians 1:17, Ephesians 3:19, 1 Timothy 1:17, and Revelation 22:13. They are not printed.

## Runtime check

```bash
bwbasic JESUS.bas
echo "exit:$?"
```

A successful run prints `JESUS` after the banner and then `exit:0`.
