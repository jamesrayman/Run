# Run

A universal utility for running programs

## Usage

```text
run filename.ext
```

Run the program `filename.ext` using the rule `ext`, which is specified in `rule/ext`.

```text
run ext2 filename.ext
```

Run the program `filename.ext` using the rule `ext2`.

```text
run rule
```

Run the rule `rule` with filename set to empty string (this is for special rules like `clean` and `help`).

## Rule Specification

All rules are specified in files located in the `rule` directory. Each rule is a series of commands, one per line. The type of command is determined by the first character in the line.

| First character | Significance |
|-----|------------------------|
| `!` | The rest of that line is executed as a bash command. If any command fails, `run` stops and exits with the same exit status. |
| `>` | It will mark the file following it as a compiled object. Following on the same line is the list of prerestiquites for that object. The following block of lines specify how to compile that object. Compilation only occurs if objects are not up to date. If one of the prerestiquites does not exist, `run` exits 1. Compilation instructions can not be nested. |
| `$` | The line ends a block. |
| `#` | The line is a comment. |

The string `@` is replaced with `filename` (absolute path not including `.ext`, special characters not escaped) wherever it is found in the rule file. The string `@@` is replaced with the project directory of `run` wherever it is found in the rule file.

## List of rules

The following list of rules are all associated with their expected languages and run as expected:

* `cpp`
* `java`
* `py`

### `help`

Display the help file

### `clean`

Delete all compiled objects in the given directory or all compiled objects if no directory is given.
