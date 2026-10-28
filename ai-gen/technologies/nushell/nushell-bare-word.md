In [Nushell](https://www.nushell.sh/book/working_with_strings.html), a text token is interpreted as a bare word string when it satisfies two conditions simultaneously: it is unquoted, and the parser fails to match it to any other specific datatype syntax.
Because Nushell is type-aware, its parser actively scans tokens from left to right. If a token does not strictly match a structured type, it defaults to a bare word string. [1] 
------------------------------
## 🚫 What Instantly Prevents a Bare Word?
A token is never a bare word if it contains any of the following characters (which act as delimiters or structural syntax):

* Spaces or tabs (these separate arguments).
* Quotes (', ", or r#' raw syntax).
* Pipeline or redirection characters (|, e>, out>).
* Brackets, braces, or parentheses ([, ], {, }, (, )).
* Comments (#). [2] 

------------------------------
## 🔍 How the Parser Rules Out Other Types
Before falling back to a bare word string, the parser checks if the token matches any of the following literal formats: [1] 

| If it looks like this... | ...it is parsed as: | Examples |
|---|---|---|
| Digits only (or with -) | Integer | 123, -45 |
| Digits with a dot | Float | 1.5, -0.99 |
| Digits followed by a unit | Duration / Filesize | 5min, 20day, 4.5mb, 10gb |
| True / False / Null | Boolean / Nothing | true, false, null |
| Starts with $ | Variable / Cell Path | $env, $foo |
| Starts with -- or - | Command Flag | --verbose, -f |

If a token is unquoted, has no spaces, and does not match any of those rules, it becomes a bare word.
------------------------------
## ✅ Exact Examples of Bare Words
The following are valid bare words because they avoid spaces, quotes, and primary datatype syntax:

* Basic words: hello, my_variable, build-v2
* File paths: /usr/local/bin, C:\Users\Name\Documents, ../relative/path
* Network addresses & URLs: 192.168.1.1, https://nushell.sh
* Mixed alphanumeric text: v1.0.4, error_code_404, commit-a1b2c3d [2, 3] 

------------------------------
## ⚠️ Special Parsing Behaviors
Once a token is classified as a bare word, two special symbols trigger additional internal rules:
## 1. Globs (* and ?)
If a bare word contains * or ?, the parser treats it as a [Glob type](https://www.nushell.sh/lang-guide/chapters/types/basic_types/glob.html) instead of a standard string literal. [4, 5] 

* open *.txt $\rightarrow$ Evaluated as a glob pattern.
* open "*.txt" $\rightarrow$ Evaluated strictly as a literal file named *.txt. [4] 

## 2. Tilde (~)
If a bare word starts with ~, Nushell automatically expands it to your home directory path before passing it forward. [6] 
------------------------------
Would you like to see how bare words are handled differently when passed to an internal Nushell command versus an external system command? [6] 

[1] [https://www.nushell.sh](https://www.nushell.sh/book/scripts.html)
[2] [https://www.nushell.sh](https://www.nushell.sh/book/working_with_strings.html)
[3] [https://www.nushell.sh](https://www.nushell.sh/book/custom_commands.html)
[4] [https://www.nushell.sh](https://www.nushell.sh/lang-guide/chapters/types/basic_types/glob.html)
[5] [https://www.nushell.sh](https://www.nushell.sh/book/moving_around.html)
[6] [https://github.com](https://github.com/nushell/nushell/issues/9723)
