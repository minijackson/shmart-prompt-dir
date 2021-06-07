Shmart prompt dir
=================

A prompt directory that is both smart and short.

This little piece of Zsh code is heavily inspired by the [P10K] prompt, but is
designed so that you can easily integrate it into your own configuration.

By default, it shortens your current directory from left to right until its
length is less than 30, but will not truncate git directories and the last
directory.

It also supports smart truncation of directories in the [Nix] store.

[P10K]: <https://github.com/romkatv/powerlevel10k>
[Nix]: <https://nixos.org/>

Configuration variables
-----------------------

shmart-prompt-dir is configurable using `zstyle`. For example, to set the
desired maximum length, you can set:

```zsh
zstyle ':shmart-prompt-dir' truncation-length 45
```


Note that even with this configuration, the result can still be over 45 if
shmart-prompt-dir can't truncate everything.

You can also set the style of all "important" directories like so:

```zsh
zstyle ':shmart-prompt-dir:important:*' style "%B%F{red}"
```

| Context | Style | Default value | Description |
|---------|-------|---------------|-------------|
| `:shmart-prompt-dir` | `truncation-length` | 30 | Will try to truncate your prompt as much as possible until its length under this value |
| `:shmart-prompt-dir` | `minimum-length` | 1 | Minimum length of each path component |
| `:shmart-prompt-dir` | `extra-whitespace` | `yes` | Whether to add an extra whitespace after the path |
| `:shmart-prompt-dir:separator` | `style` | `%F{8}` (gray) | Style of path separators (`/`) |
| `:shmart-prompt-dir:shortened` | `style` | `%B%F{8}` (bold gray) | Style of path components that were shortened |
| `:shmart-prompt-dir:kept` | `style` | `%B%F{default}` (bold default color) | Style of path components that were kept as is (e.g. `~`, `~root`, etc.) |
| `:shmart-prompt-dir:failed` | `style` | `$kept` (bold default color) | Style of path components that were kept as is, even though we wanted to truncate them |
| `:shmart-prompt-dir:important:nix` | `enable` | `yes` | Whether to enable the custom truncation of `/nix/store/*` paths |
| `:shmart-prompt-dir:important:nix` | `style` | `%B%F{12}` (bold light blue) | Style of `/nix/store/*` paths |
| `:shmart-prompt-dir:important:nix` | `hash-length` | 4 | Length of the derivation hash |
| `:shmart-prompt-dir:important:git` | `enable` | `yes` | Whether to enable the detection of Git directories |
| `:shmart-prompt-dir:important:git` | `style` | `%B%F{12}` (bold light blue) | Style of Git directories |

Integration examples
--------------------

Simply put this file somewhere in a directory from your `$fpath` and add
`autoload -Uz shmart-prompt-dir` to make the `shmart-prompt-dir` function
available. Calling the function will set the `$reply` variable with your
stylized directory.

### Grml Zsh

```zsh
zstyle -e ':prompt:grml*:*:items:path' token shmart-prompt-dir
```
