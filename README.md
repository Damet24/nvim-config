## ⚙️ `init.lua` - Configuración de Neovim

Este archivo contiene la configuración base de Neovim, incluyendo opciones de *Vim*, la instalación y configuración del gestor de paquetes **pckr.nvim**, y la configuración de varios *plugins* esenciales para un entorno de desarrollo moderno.

### 1\. Opciones Base de Vim (vim.opt)

Estas configuraciones establecen el comportamiento básico del editor:

| Configuración | Valor | Descripción |
| :--- | :--- | :--- |
| `vim.opt.relativenumber` | `true` | Muestra los números de línea relativos, facilitando el movimiento. |
| `vim.opt.cursorline` | `true` | Resalta la línea actual donde se encuentra el cursor. |
| `vim.g.mapleader` | `"<Space>"` | Establece la tecla **líder** (`<Leader>`) en la **barra espaciadora**. Se usa para atajos de teclado personalizados. |
| `vim.cmd("syntax off")` | | Desactiva el resaltado de sintaxis básico de Vim. (Esto se reemplaza por **nvim-treesitter**). |
| `vim.opt.clipboard` | `"unnamedplus"` | Permite que el portapapeles de Neovim interactúe con el portapapeles del sistema (registro `+`). |
| `vim.opt.tabstop` | `4` | Número de espacios que representa un tabulador. |
| `vim.opt.shiftwidth` | `4` | Número de espacios para el sangrado automático. |
| `vim.opt.expandtab` | `true` | Convierte los caracteres de tabulación en espacios (indentación suave). |
| `vim.opt.autoindent` | `true` | Mantiene la sangría de la línea anterior al insertar una nueva línea. |
| `vim.o.background` | `"dark"` | Indica que el esquema de color está diseñado para un fondo oscuro. |

### 2\. Gestor de Paquetes (`pckr.nvim`)

**pckr.nvim** es un gestor de paquetes minimalista y rápido.

El siguiente código se encarga de verificar si `pckr.nvim` está instalado. Si no lo está, lo clona desde GitHub y lo agrega a la ruta de tiempo de ejecución de Neovim (`rtp`).

```lua
local function bootstrap_pckr()
	local pckr_path = vim.fn.stdpath("data") .. "/pckr/pckr.nvim"

	if not (vim.uv or vim.loop).fs_stat(pckr_path) then
		vim.fn.system({
			"git",
			"clone",
			"--filter=blob:none",
			"https://github.com/lewis6991/pckr.nvim",
			pckr_path,
		})
	end

	vim.opt.rtp:prepend(pckr_path)
end

bootstrap_pckr()
```

### 3\. Configuración de Plugins

Aquí se listan y configuran todos los *plugins* instalados a través de `pckr.nvim`.

#### **3.1. LSP (Language Server Protocol) y `mason`**

Estos *plugins* permiten la funcionalidad de IDE (autocompletado avanzado, ir a definición, referencias, etc.) utilizando **Servidores de Lenguaje**.

  * `neovim/nvim-lspconfig`: Configuraciones predeterminadas para varios Servidores de Lenguaje (LSP).
  * `mason-org/mason.nvim`: Un gestor de paquetes para instalar y administrar LSP, *formatters* y *linters*.
  * `mason-org/mason-lspconfig.nvim`: Conecta `mason` con `nvim-lspconfig` para instalar y configurar automáticamente los LSP.
      * **Servidores instalados**: `lua_ls` (Lua), `pyright` (Python), `jsonls` (JSON).

#### **3.2. Autocompletado Avanzado (`nvim-cmp`)**

**nvim-cmp** es el motor de autocompletado principal, asistido por **LuaSnip** para *snippets* y **lspkind** para íconos visuales.

| Plugin | Descripción |
| :--- | :--- |
| `hrsh7th/nvim-cmp` | El motor de autocompletado principal. |
| `hrsh7th/cmp-buffer` | Fuente de sugerencias basadas en el texto del *buffer* actual. |
| `hrsh7th/cmp-path` | Fuente de sugerencias para rutas del sistema de archivos. |
| `L3MON4D3/LuaSnip` | Motor de *snippets* (fragmentos de código reutilizables). |
| `saadparwaiz1/cmp_luasnip` | Conector entre **nvim-cmp** y **LuaSnip**. |
| `rafamadriz/friendly-snippets` | Colección popular de *snippets* de estilo VS Code. |
| `onsails/lspkind.nvim` | Añade íconos (*pictograms*) de tipo VS Code a las sugerencias de autocompletado. |
| `hrsh7th/cmp-nvim-lsp` | Fuente de sugerencias de autocompletado basada en los Servidores de Lenguaje (LSP). |

**Mapeos de teclado (`nvim-cmp`):**

| Tecla | Función |
| :--- | :--- |
| `<C-k>` | Seleccionar la sugerencia anterior. |
| `<C-j>` | Seleccionar la sugerencia siguiente. |
| `<C-Space>` | Forzar la aparición del menú de autocompletado. |
| `<CR>` | Confirmar la sugerencia seleccionada sin forzar la selección. |

#### **3.3. Resaltado de Sintaxis Avanzado (`nvim-treesitter`)**

**nvim-treesitter** reemplaza el antiguo sistema de resaltado de sintaxis de Vim con un *parser* más rápido y preciso.

  * `nvim-treesitter/nvim-treesitter`: Motor de *parsing* (análisis sintáctico).
      * **Lenguajes instalados**: `lua`, `python`.
      * `highlight = { enable = true }`: Activa el resaltado de sintaxis basado en Treesitter.

#### **3.4. Formato de Código (`conform.nvim`)**

**conform.nvim** permite formatear automáticamente el código utilizando *formatters* externos.

  * `stevearc/conform.nvim`: Interfaz para formateadores de código.
      * **Formatters configurados:**
          * `lua`: `stylua`
          * `python`: `black`
      * `format_on_save`: Formatea el archivo automáticamente al guardarlo.

#### **3.5. Explorador de Archivos (`nvim-tree.lua`)**

**nvim-tree.lua** proporciona un explorador de archivos estilo árbol dentro de Neovim.

  * `nvim-tree/nvim-tree.lua`: Explorador de archivos.
      * **Configuración clave:** Se abre por defecto a la **derecha** (`side = "right"`) y oculta los archivos con punto (`dotfiles = true`).
      * **Atajo de teclado:**
          * `<C-b>`: Alterna la visibilidad del explorador (`NvimTreeToggle`).

#### **3.6. Tema (`vscode.nvim`)**

  * `Mofiqul/vscode.nvim`: Aplica el popular esquema de color de VS Code.
      * `vim.cmd.colorscheme("vscode")`: Establece el esquema de color.

### 4\. Mapeos y Configuraciones LSP Adicionales

Esta sección define atajos de teclado que se activan *solo* cuando un Servidor de Lenguaje (LSP) está adjunto a un *buffer* (es decir, cuando estás editando un archivo de un lenguaje que tiene un LSP configurado, como Lua o Python).

```lua
local keymap = vim.keymap -- para concisión
vim.api.nvim_create_autocmd("LspAttach", {
	-- ... configuración de autocomando ...
	callback = function(ev)
		local opts = { buffer = ev.buf, silent = true }

		-- set keybinds
		opts.desc = "Show LSP references"
		keymap.set("n", "gR", "<cmd>Telescope lsp_references<CR>", opts) -- Muestra las referencias de una función/variable
		-- (Requiere que Telescope esté configurado)

		opts.desc = "Restart LSP"
		keymap.set("n", "<leader>rs", ":LspRestart<CR>", opts) -- Reinicia el LSP para el buffer actual.

		opts.desc = "Format code"
		keymap.set("n", "<leader>cf", function()
			vim.lsp.buf.format()
		end, opts) -- Formatea el código (si `conform.nvim` no lo hace al guardar).
	end,
})
```

#### **Configuración de Diagnósticos**

Configura cómo se muestran los mensajes de diagnóstico (errores, advertencias, hints, info) proporcionados por los LSP, utilizando íconos específicos.

| Severidad | Ícono | Descripción |
| :--- | :--- | :--- |
| `ERROR` | `   ` | Errores críticos. |
| `WARN` | `   ` | Advertencias. |
| `HINT` | ` 󰠠  ` | Sugerencias o posibles mejoras. |
| `INFO` | `   ` | Información general. |
