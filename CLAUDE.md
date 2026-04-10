# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Application

No build system or dependencies. Open any HTML file directly in any modern browser. No server required.

**Main file:** `cc_sistema.html` — the unified system (Directorio + Streaming in one file).  
The individual module files (`cc_modulo1_directorio.html`, `cc_modulo2_streaming.html`) are kept as originals; do not modify them.

## Files

| File | Description |
|------|-------------|
| `cc_sistema.html` | **Main app** — unified system with Directorio and Streaming modules |
| `cc_modulo1_directorio.html` | Original Módulo 1 (Directorio) — do not modify |
| `cc_modulo2_streaming.html` | Original Módulo 2 (Streaming) — do not modify |

## Architecture — cc_sistema.html

Single-file vanilla JS/HTML/CSS application.

### Data layer

Two separate `localStorage` keys:

| Key | Variable | Contents |
|-----|----------|----------|
| `cc_dir` | `db` | `{ clientes: [], vendedores: [], proveedores: [] }` |
| `cc_stream4` | `dbStream` | `{ registros: [], tarifario: [...], tasas: {...} }` |

- `saveDir()` writes `db` to `cc_dir`
- `saveStream()` writes `dbStream` to `cc_stream4`
- `loadAll()` reads both keys on startup
- The Streaming module reads directly from `db` (no separate copy)

### Navigation model

Three levels:

1. **Module tabs** (topbar) — `showMod(mod, btn)` switches between `Directorio` and `Streaming`
2. **Sub-nav bar** — `showDirPage(id, btn)` / `showStreamPage(id, btn)` switch pages within each module
3. **Inner tabs** (Directorio only) — `showIsec(pre, id, btn)` switches between form and list views

The tasa-bar (currency rates) is shown only when Streaming is active.

### Modules

**Directorio** (`#mod-directorio`) — sub-nav: Resumen | Clientes | Vendedores | Proveedores
- **Resumen** — dashboard with stats and recent entries (`renderResumen()`)
- **Clientes** — client CRUD (`guardarCliente`, `editarCliente`, `eliminarCliente`, `renderClientes`)
- **Vendedores** — seller CRUD (`guardarVendedor`, `editarVendedor`, `eliminarVendedor`, `renderVendedores`)
- **Proveedores** — supplier CRUD (`guardarProveedor`, `editarProveedor`, `eliminarProveedor`, `renderProveedores`)

**Streaming** (`#mod-streaming`) — sub-nav: Resumen | Registrar | Inventario | Alertas | Comisiones | Tarifario
- **Resumen** — dashboard with stats and recent records (`renderDash()`)
- **Registrar** — service registration form with currency conversion and gain calculation
- **Inventario** — full records table with filters (`renderInv()`)
- **Alertas** — expiry alerts (`renderAlertas()`)
- **Comisiones** — commission breakdown by seller (`renderComisiones()`)
- **Tarifario** — purchase price list with history (`renderTarifario()`)

### Data models

- Cliente: `{id, nombre, wa, pais, tipo, notas, fecha}`
- Vendedor: `{id, nombre, wa, pais, estado, pct, notas, saldoPrestamo, fecha}`
- Proveedor: `{id, nombre, wa, pais, tipo, notas, fecha}`
- Registro: `{id, num, cliente, vendedor, servicio, plan, pct, moneda, ventaMonto, ventaBS, compraBS, gBS, gUSDT, comNativa, comUsdt, proveedor, correo, clave, perfil, pin, inicio, vence, notas, tasasUsadas, fecha}`
- Tarifario entry: `{id, n, c, prov, hist: [{f, m}]}`

### Shared helpers (used by both modules)

`ini()`, `tipoBadge()`, `estadoBadge()`, `tipoPBadge()`, `getDias()`, `getEst()`, `bCls()`, `isCombo()`, `fmtDate()`

### UI conventions

- Language: Spanish (es-AR locale for dates)
- CSS custom properties: `--azul`, `--cian`, `--verde`, `--rojo`, `--am`
- Badges via `tipoBadge()`, `tipoPBadge()`, `estadoBadge()`
- Avatar initials via `ini(nombre)`
- Font: Nunito (Google Fonts CDN)
- Selects in Streaming warn when Directorio has no data (e.g. "⚠️ No hay clientes — agregá desde Directorio")
