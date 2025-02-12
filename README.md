# clang-format preprocessor patch

This bash script (derived from [p-clang-format](https://github.com/MedicineYeh/p-clang-format)) provides a workaround for ```clang-format``` to allow manual indenting of preprocessor directives.

By default the script prevents formatting for the following directives, and will match the directive and any following characters:

```bash
local prefixes="#pragma #define #if #else #end"
```

This may be adjusted to support more directives by modifying the value of ```prefixes```, each matched pattern is separated by a space.

### Autoformatting with neovim

To support autoformatting with this patch you may use [conform.nvim](https://github.com/stevearc/conform.nvim) to define a custom formatter pointing to the file. Make sure the script is given executable permissions and is in a location detectable by ```$PATH```. 

An example configuration is provided below:

```lua
config = function()
  require("conform").formatters.clang_format_patched =
    { command = "clang-format-patched", args = { "-i", "$FILENAME" }, stdin = false }
  require("conform").formatters_by_ft.c = { "clang_format_patched" }
  require("conform").setup {
    format_on_save = {
      -- These options will be passed to conform.format()
      timeout_ms = 1000,
      lsp_format = "never",
    },
    log_level = vim.log.levels.DEBUG,
  }
```
