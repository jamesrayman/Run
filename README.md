# Run

A universal utility for running programs

## Usage

```text
run target.ext ...
```

Run the program `target.ext` using the rule `ext`, which is specified in `rule/ext`. Rule-specific extra arguments may be passed.

```text
run ext target ...
```

Run the program `target` using the rule `ext`. Rule-specific extra arguments may be passed.

```text
run rule
```

Run the rule `rule` with target set to empty string (this is for special rules like `help`).

## Examples

```text
run tree.cpp
```

Run `tree.cpp` using the rule `cpp`.

``` text
run java prog.txt
```

Run `prog.txt` using the rule `java`.

``` text
run help
```
Run the rule `help`.

## Rule Specification

All rules are specified in files located in the `rule` directory. Each rule is a series of commands, one per line. The type of command is determined by the first character in the line.

| First character | Significance |
|-----|------------------------|
| `!` | The rest of that line is executed as a bash command. If any command fails, `run` stops and exits with the same exit status. |
| `>` | It will mark the file following it as a compiled object. Following on the same line is the list of prerestiquites for that object. The following block of lines specify how to compile that object. Compilation only occurs if objects are not up to date. If one of the prerestiquites does not exist, `run` exits 1. Compilation instructions can not be nested, but can be chained. |
| `$` | The line ends a block. |
| `#` | The line is a comment. |

The substitutions below are performed on the rule file before running. Note that, as of now, special characters not escaped.

| Symbol | Replacement |
|--------|-------------|
| `%`    | absolute path of the target not including `.ext` |
| `@^`   | directory from which the command was run |
| `@:`   | project directory of `run` |
| `@1`   | the first extra argument, the second argument is `$2`, and so on, until `$9` |
| `@*`   | all the extra arguments |
| `@%`   | `%` (escapes the percent sign) |
| `@@`   | `@` (escapes the at sign) |

## List of rules

The following list of rules are all associated with their expected languages and run as expected: `cpp`, `java`, `py`.
View `run help` to see more.

### `help`

Display the help file. No target is expected.

### `clean`

Delete all compiled objects in the given directory or all compiled objects if no directory is given.

## Notes

Packages are not supported for Java.
