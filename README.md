# fish-jj-prompt

An async [jujutsu (jj)](https://github.com/martinvonz/jj) prompt plugin for [Fish shell](https://fishshell.com/).

## Features

- ⚡ **Fully async** - Non-blocking prompt that stays responsive
- 🎨 **Rich info display** - Shows change ID, bookmarks, descriptions, and status flags
- 🔄 **Smart detection** - Automatically detects jj repos
- 🚀 **Zero impact** - Background processing keeps your shell zippy
- 🎯 **Universal compatibility** - Works with any Fish prompt

## Installation

Install with [Fisher](https://github.com/jorgebucaran/fisher):

```fish
fisher install dacuna/fish-jj-prompt-plugin
```

## Usage

The plugin provides two main functions for integration with any Fish prompt:

### `fish_jj_prompt`

Returns the formatted jj prompt string with all information:

```fish
function fish_prompt
    set --local jj_prompt (fish_jj_prompt 2>/dev/null)
    if test -n "$jj_prompt"
        echo -n $jj_prompt
    end
end
```

### `jj_prompt_is_repo`

Returns 0 (success) if in a jj repository, 1 otherwise. Use this to conditionally show jj vs git prompts:

```fish
function fish_prompt
    if jj_prompt_is_repo
        echo -n (fish_jj_prompt)
    else
        echo -n (fish_git_prompt)
    end
end
```

### Example: Integration with Hydro

If you're using [Hydro prompt](https://github.com/jorgebucaran/hydro), modify your `fish_prompt` function to conditionally show jj or git info:

```fish
function fish_prompt --description Hydro
    # Use jj prompt in jj repos, otherwise use Hydro's git prompt
    set --local vcs_info ""
    if jj_prompt_is_repo
        set vcs_info (fish_jj_prompt)
    else
        set vcs_info $_hydro_color_git$_hydro_git$hydro_color_normal
    end
    
    echo -e "$_hydro_color_start$hydro_symbol_start$hydro_color_normal$_hydro_color_pwd$_hydro_pwd$hydro_color_normal $vcs_info $_hydro_color_duration$_hydro_cmd_duration$hydro_color_normal$_hydro_status$hydro_color_normal "
end
```

## Display

In jj repositories, the prompt displays:

- **Change ID** in bold magenta (e.g., `[k]`)
- **Bookmarks** with sync status (* indicates unsynced)
- **Description** if no bookmarks exist
- **Status flags** like conflict, empty, divergent, hidden
- **Visual history** showing commits from trunk fork point to current

Example prompt in a jj repo:
```
~/project [k] main* (empty) ○ ○ ● 
❱
```

## How It Works

The plugin uses Fish's event system to:

1. Check if you're in a jj repo on directory changes (`PWD`)
2. Spawn background processes to fetch jj info asynchronously
3. Update the prompt without blocking when data is ready

## Requirements

- [Fish shell](https://fishshell.com/) 3.0+
- [jujutsu (jj)](https://github.com/jj-vcs/jj) CLI tool

## License

MIT

