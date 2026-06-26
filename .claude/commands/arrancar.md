---
description: Levanta server de preview, muestra estado del repo y campañas disponibles
---

Arrancá la sesión de trabajo en este proyecto. Ejecutá los siguientes pasos en este orden y devolveme un resumen tight al final:

## 1. Server de preview

Si el puerto 8765 está ocupado, matá el proceso antes de levantar (no podés tener dos servers en el mismo puerto):

```bash
lsof -ti:8765 | xargs -r kill -9 2>/dev/null
```

Levantá `python3 -m http.server 8765` en background desde `campanas/`:

```bash
cd "/Users/martin/Desktop/proyectos ia/mailing/campanas" && python3 -m http.server 8765
```

(usá `run_in_background: true`)

## 2. Estado del repo

Corré en paralelo:

- `git status` (mostrá rama, untracked, modificados)
- `git log --oneline -5`
- `ls campanas/*.html` (lista de campañas one-off disponibles)

## 3. Fase actual del proyecto

Leé el `README.md` de la raíz, sección "Replanteo v4" + listá brevemente las plantillas en `templates/`. Si hay plantillas untracked, marcalas como "Fase 2 en curso".

## 4. Drafts/scheduled en Mailchimp

Listá las campañas en estado `save` (draft) y `schedule` (programadas) para ver qué hay pendiente:

```bash
curl -s "https://us20.api.mailchimp.com/3.0/campaigns?count=20&status=save,schedule&fields=campaigns.id,campaigns.status,campaigns.send_time,campaigns.settings.subject_line,campaigns.settings.title" \
  --user "anystring:$MAILCHIMP_API_KEY" \
  | python3 -c "import sys,json; d=json.load(sys.stdin); rows=[(c['id'], c['status'], c.get('send_time','')[:16] or '-', c['settings'].get('subject_line','')[:60]) for c in d.get('campaigns',[])]; [print(f'{r[0]} · {r[1]:<9} · {r[2]} · {r[3]}') for r in rows] if rows else print('(sin drafts ni programadas)')"
```

## 5. Resumen final

Devolvé al usuario un mensaje tight con:

- URL base del preview: `http://localhost:8765/`
- Una línea por campaña en `campanas/` con su URL clickeable
- Rama y cantidad de archivos sin commitear
- Estado de la fase (cuál está cerrada, cuál está en curso)
- Drafts/scheduled de Mailchimp listados

No abras tabs del browser automáticamente — el usuario las abre cuando quiera. Solo dejá las URLs disponibles.
