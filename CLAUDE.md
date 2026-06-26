# Mailing — repo de templates HTML para Vinoteca Ligier

Templates HTML (`templates/*.html`) usados por el wizard de Vercel
(`mdd145-prog/ligier-app`) y por el endpoint `send-transactional` para armar
emails de campañas masivas y transaccionales 1:1.

---

## Skills compartidas — convención

Las skills en `.claude/skills/` (cuando existan) son **COPIAS** de
**https://github.com/mdd145-prog/LIGIER** — la biblioteca central de skills
empresariales (organizada por área: ventas/marketing/logística/etc.).

### Regla dura

**NO editar `.claude/skills/*` en este repo.** Cualquier cambio que se haga acá
se va a perder en la próxima sincronización.

Si una skill compartida necesita cambios:

1. Editar en LIGIER: `<area>/<nombre-skill>/SKILL.md`. Commit + push en LIGIER.
2. Volver a este repo y correr `./scripts/sync-skills.sh` (cuando exista — pendiente).
3. Commitear la copia actualizada en Mailing.

### Skills consumidas hoy

_Ninguna todavía._ Cuando se sumen, listar acá con:

- Nombre, área en LIGIER, para qué se usa en Mailing.

### Skills compartidas existentes en LIGIER (catálogo)

Para referencia (no necesariamente consumidas por este repo):

| Skill | Área en LIGIER | Para qué sirve |
|---|---|---|
| `lgr-armado-mailing` | `marketing/` | Reglas duras para armar piezas de mailing: stock, presentación 750ml, promos validadas con cart real |
| `redactor-ligier` (en lgr) | _pendiente migrar a LIGIER_ | Voz, copy y propuesta de venta |

---

## Cómo trabajar en este repo

- **HTML de templates** se edita libremente acá. Es la fuente de verdad de
  la estética: tipografías, colores, layout, banner 6×5, etc.
- **Copy** (subjects, hero, bajadas, CTAs) lo dicta `redactor-ligier` —
  cuando se le aplique a una pieza de este repo, seguir esa guía.
- **Productos y promos** que se muestran los maneja
  `seleccion-productos-mailing` desde `ligier-app` (el wizard / el endpoint
  `send-transactional`). Este repo solo provee el HTML base; la inyección
  de productos pasa client-side en Vercel.

## Repos relacionados

- `mdd145-prog/LIGIER` — biblioteca central de skills empresariales.
- `mdd145-prog/ligier-app` — wizard de campañas + endpoint transaccional (Vercel).
- `mdd145-prog/lgr` — sistema de gestión (Laravel + React).
