# ii-agent-updates

Хостинг обновлений расширения **ИИ-Агент для 1С:УТ**.

Раздаётся через **Cloudflare (Worker со статикой)**, папка `./public` (см. `wrangler.jsonc`).

Публичный адрес: **https://ii-agent-updates.muhinv451.workers.dev**

## Структура

```
public/
├─ latest.json            манифест текущей версии (его читает расширение)
└─ releases/
   └─ ИИ_Агент_X.Y.Z.cfe  файлы расширений (все версии — для отката)
```

## Публичные адреса (после подключения Cloudflare Pages)

- Манифест: `https://ii-agent-updates.muhinv451.workers.dev/latest.json`
- Файл релиза: `https://ii-agent-updates.muhinv451.workers.dev/releases/ИИ_Агент_X.Y.Z.cfe`

## Формат `latest.json`

| Поле | Назначение |
|---|---|
| `version` | Версия расширения, напр. `1.4.0` |
| `url` | Прямая ссылка на `.cfe` этой версии |
| `changelog` | Что нового (показывается в баннере) |
| `released` | Дата релиза `ГГГГ-ММ-ДД` |
| `min_platform` | Мин. версия платформы 1С |
| `mandatory` | `true` — обновление обязательное (баннер нельзя закрыть) |

## Как выпустить релиз

1. В Конфигураторе: **Расширения → Сохранить в файл** → `ИИ_Агент_X.Y.Z.cfe`.
2. Положить файл в `public/releases/`.
3. Обновить `public/latest.json` (version, url, changelog, released).
4. `git tag vX.Y.Z` + `git push --tags` — Cloudflare Pages задеплоит автоматически.

## Откат

Файлы прошлых версий остаются в `releases/`. Для отката — вернуть в
`latest.json` поля `version`/`url` на предыдущую версию и запушить.
