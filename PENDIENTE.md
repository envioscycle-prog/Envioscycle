# PENDIENTE — cc_sistema.html

## Estado actual

`cc_sistema.html` ya fue **generado** (246 KB, ~1392 líneas).  
Está en `/home/user/Envioscycle/cc_sistema.html` pero **no fue verificado ni commiteado**.

---

## Qué se hizo

El script Python `/tmp/gen_final.py` (o el bloque inline equivalente) construyó el archivo unificado aplicando las siguientes transformaciones:

### HTML
- Una sola `<topbar>` con logo + tabs de módulo: **Directorio | Streaming**
- Sub-nav de Directorio (`#subnav-dir`): Resumen | Clientes | Vendedores | Proveedores
- Sub-nav de Streaming (`#subnav-stream`): Resumen | Registrar | Inventario | Alertas | Comisiones | Tarifario
- Tasa-bar (`#tasa-bar`) oculta por defecto, se muestra solo al activar Streaming
- `#mod-directorio` contiene las 4 páginas del Módulo 1
- `#mod-streaming` contiene las 6 páginas del Módulo 2
- Conflicto de ID `stats-grid` resuelto → renombrado a `stream-stats-grid` en Streaming

### JavaScript
- `db` = directorio compartido (`cc_dir`)
- `dbStream` = datos de streaming (`cc_stream4`)
- `saveDir()` / `saveStream()` / `loadAll()` en lugar de las funciones `save()`/`load()` individuales
- `showMod(mod, btn)` — alterna entre módulos
- `showDirPage(id, btn)` — navegación interna de Directorio
- `showStreamPage(id, btn)` — navegación interna de Streaming
- `showIsec(pre, id, btn)` — tabs internos de Clientes/Vendedores/Proveedores
- Helpers compartidos: `ini()`, `tipoBadge()`, `estadoBadge()`, `tipoPBadge()`, `getDias()`, `getEst()`, `bCls()`, `isCombo()`, `fmtDate()`
- Todas las referencias `directorio.xxx` → `db.xxx` en el código de Streaming
- Todas las referencias `db.registros` / `db.tarifario` / `db.tasas` → `dbStream.xxx`
- Botones de alerta en `renderDash()` → llaman `showStreamPage('alertas', ...)` en lugar de `.tab[3].click()`
- `editar(id)` actualizado para navegar con `#subnav-stream .stab`
- Mensajes de advertencia: "Módulo 1" → "Directorio"

---

## Lo que falta hacer en la próxima sesión

### 1. Verificación visual (CRÍTICO — abrir en browser)
Abrir `cc_sistema.html` en el navegador y probar:
- [ ] Tab **Directorio** muestra sub-nav y páginas correctas
- [ ] Tab **Streaming** muestra tasa-bar + sub-nav de Streaming
- [ ] Guardar un cliente en Directorio → aparece en el select de Registro de Streaming
- [ ] Guardar un vendedor activo → aparece filtrado en Streaming
- [ ] Guardar un proveedor → aparece en tarifario
- [ ] CRUD completo de Clientes / Vendedores / Proveedores funciona
- [ ] Registrar un servicio de Streaming, guardar, aparece en Inventario y Dashboard
- [ ] Editar un registro de Streaming → navega correctamente al tab Registrar
- [ ] Alertas de vencimiento funcionan
- [ ] Comisiones calculan bien
- [ ] Tarifario: agregar, actualizar precio, historial, eliminar
- [ ] Export CSV funciona
- [ ] Tasas del día se persisten
- [ ] localStorage: datos en `cc_dir` y `cc_stream4` separados

### 2. Posibles bugs conocidos a revisar
- El bloque `let dbStream={...}` extraído del script puede tener truncamiento — verificar que el tarifario inicial (10 entradas) esté completo en el archivo generado
- La función `renderTarifario()` tiene template literals con backticks — verificar que el script Python no haya escapado caracteres incorrectamente
- `editar()` en Streaming: verificar que el `setTimeout` y la navegación al tab Registrar funcione correctamente

### 3. Commit y push (solo después de verificar)
```bash
cd /home/user/Envioscycle
git add cc_sistema.html
git commit -m "Add cc_sistema.html — unified Directorio + Streaming in single file"
git push -u origin claude/init-project-setup-NgQSU
```

### 4. Actualizar CLAUDE.md
Documentar `cc_sistema.html` como el archivo principal del sistema unificado.

---

## Archivos en el repo
- `cc_modulo1_directorio.html` — módulo original de directorio (NO modificar)
- `cc_modulo2_streaming.html` — módulo original de streaming (NO modificar)
- `cc_sistema.html` — sistema unificado (generado, pendiente de verificación)
- `CLAUDE.md` — documentación del proyecto
