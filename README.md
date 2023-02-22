# Terminal Emulators integration
[Shell integration](https://iterm2.com/documentation-escape-codes.html) for Xonsh.

The following terminal emulators are supported
- [iTerm2](https://iterm2.com/documentation-shell-integration.html)
- [kitty](https://sw.kovidgoyal.net/kitty/shell-integration/)
- [WezTerm](https://wezfurlong.org/wezterm/shell-integration.html) with CWD; Input, Output, and Prompt zones; and User Vars for tracking additional shell state

**Note**: If identifying current terminal fails, `iTerm2` hooks are loaded.

PRs welcome on improving the support to more terminal programs :)


## Installation

To install use pip:

``` bash
xpip install xontrib-term-integrations
# or: xpip install -U git+https://github.com/jnoortheen/xontrib-term-integrations
```


## Usage

``` bash
# this modifies the $PROMPT function. So load it after setting $PROMPT if you have a custom value
xontrib load term_integration
```

Set user vars[^1] via the helper `set_user_var` function:
```xsh
from xontrib_term_integrations.utils import set_user_var
set_user_var('my_term_user_var','value_of_my_term_user_var')
```

## Contributing

Please make sure that you
* Document the purpose of functions and classes.
* When adding a new feature, please mention it in the `README.md`. Use screenshots when applicable.
* [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) style should be used
  for commit messages as it is used to generate changelog.
* Please use [pre-commit](https://pre-commit.com/) to run qa checks. Configure it with

```sh
pre-commit install-hooks
```

## Known issues

- (WezTerm) Multiline prompt is partially supported:
  - every continuation line is semantically marked by default:
    ```xsh
    if True:
    #↓ continuation prompt
    .....     echo 1
    #     ↑ input
    ```
    so you can select `    echo 1` as a `SemanticZone` with a mouse multiclick, but you can't select both lines as one zone (and would need to map some combo of commands to hack around it)
  - if you set `$MULTILINE_PROMPT_PRE=''`, `$MULTILINE_PROMPT_POS=''`, then continuation lines won't be marked, you'd be able to select all the lines as one `SemanticZone` (unles the _right_ prompt interferes), but that will also include `..` continuation markers (so you'd either need to disable them in Xonsh or add some extra WezTerm lua parsing hack to trim them)
    </br> (follow this [WezTerm discussion](https://github.com/wez/wezterm/discussions/3130) for updates)
- (WezTerm) Semantic _right_ prompt not separated from the next-line _left_ prompt ([issue](https://github.com/wez/wezterm/issues/3115))
- WezTerm is _not_ recognized in root shells due to [this issue](https://github.com/wez/wezterm/issues/3114)

[^1]: Variables associated with a given pane rather than a process. [WezTerm](https://wezfurlong.org/wezterm/shell-integration.html#user-vars), [iTerm2](https://iterm2.com/documentation-escape-codes.html)
