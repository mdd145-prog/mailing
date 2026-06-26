---
description: Baja el server de preview, chequea Mailchimp y resume lo pendiente
---

Cerrá la sesión de trabajo en este proyecto. Ejecutá en este orden:

## 1. Bajar server de preview

Matá el proceso que escucha en el puerto 8765:

```bash
lsof -ti:8765 | xargs -r kill -9 2>/dev/null && echo "Server bajado" || echo "(no había server corriendo)"
```

## 2. Chequear drafts/paused en Mailchimp

Listá las campañas en estado `paused` o `save` (drafts olvidados). NO toques las que están en `schedule` ni en `sent` — esas son intencionales.

```bash
curl -s "https://us20.api.mailchimp.com/3.0/campaigns?count=20&status=save,paused&fields=campaigns.id,campaigns.status,campaigns.create_time,campaigns.settings.subject_line,campaigns.settings.title" \
  --user "anystring:$MAILCHIMP_API_KEY" \
  | python3 -c "
import sys, json
d = json.load(sys.stdin)
rows = d.get('campaigns', [])
if not rows:
    print('(sin drafts ni paused)')
else:
    print('Drafts/paused encontrados (revisar manualmente si se eliminan):')
    for c in rows:
        print(f\"  {c['id']} · {c['status']:<7} · creado {c.get('create_time','')[:10]} · {c['settings'].get('title','(sin título)')[:60]}\")
"
```

Si hay drafts viejos, mencionalos pero NO los borres automáticamente — el usuario decide.

## 3. Estado del repo

Corré en paralelo:

- `git status` — mostrá untracked y modificados
- `git diff --stat HEAD` — qué archivos cambiaron y cuántas líneas
- `git log origin/main..HEAD --oneline` — commits locales sin pushear

## 4. Resumen final

Devolvé un mensaje tight con:

- ✓ Server bajado (o nota si no había)
- Drafts/paused en Mailchimp si los hay (con ID para chequear después)
- Archivos sin commitear (con cuáles)
- Commits sin pushear (si los hay)
- Una sola línea sugiriendo el próximo paso ("hacer commit", "pushear", "todo limpio", etc.)

No hagas commit, push, ni borres drafts vos solo. Esto es solo un resumen para que el usuario decida.
