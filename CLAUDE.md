# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Application

No build system or dependencies. Open `cc_modulo1_directorio.html` directly in any modern browser. No server required.

## Architecture

Single-file vanilla JS/HTML/CSS application — the entire app lives in `cc_modulo1_directorio.html`.

**Data layer:** All data persists to `localStorage` under the key `cc_dir` as a JSON object:
```js
{ clientes: [], vendedores: [], proveedores: [] }
```
`load()` reads from localStorage; `save()` writes back. There is no backend.

**Navigation model:** Two levels of navigation managed by `showPage(id, btn)` (top-level tabs) and `showIsec(pre, id, btn)` (subsections within a tab).

**Modules (tabs):**
- **Resumen** — dashboard with stats and recent entries (`renderResumen()`)
- **Clientes** — client CRUD (`guardarCliente`, `editarCliente`, `eliminarCliente`, `renderClientes`)
- **Vendedores** — seller CRUD (`guardarVendedor`, `editarVendedor`, `eliminarVendedor`, `renderVendedores`)
- **Proveedores** — supplier CRUD (`guardarProveedor`, `editarProveedor`, `eliminarProveedor`, `renderProveedores`)

**Data models:**
- Cliente: `{id, nombre, wa, pais, tipo, notas, fecha}`
- Vendedor: `{id, nombre, wa, pais, estado, pct, notas, saldoPrestamo, fecha}`
- Proveedor: `{id, nombre, wa, pais, tipo, notas, fecha}`

**UI conventions:**
- Language: Spanish (es-AR locale for dates)
- CSS custom properties for colors: `--azul`, `--cian`, `--verde`, `--rojo`, `--am`
- Badges rendered via `tipoBadge()`, `tipoPBadge()`, `estadoBadge()`
- Avatar initials via `ini(nombre)`
- Font: Nunito (Google Fonts CDN)
