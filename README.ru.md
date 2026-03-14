# justdoit

[![en](https://img.shields.io/badge/lang-en-blue.svg)](README.md)
[![ru](https://img.shields.io/badge/lang-ru-green.svg)](README.ru.md)

> Codex-first скилл, который также работает в Claude Code. Он превращает сырой запрос в понятный execution pack и спрашивает подтверждение перед запуском.

От [Ris](https://t.me/ris_ai) — канал про AI-разработку и вайбкодинг

![justdoit](./assets/justdoit.gif)

`justdoit` — репозиторий с одним скиллом и одной главной задачей:
взять расплывчатую задачу, посмотреть реальный контекст проекта, собрать execution pack и только потом коротко предложить запуск.

## Что Он Делает

- сначала смотрит проект, а не придумывает план в вакууме
- собирает `plans.md`, `status.md` и `test-plan.md`
- режет работу на dependency-safe шаги с проверками
- общается с пользователем по-человечески, а не вываливает простыни промптов
- перед execution просит явное подтверждение

## Для Кого

- для фаундеров и операторов, которым нужен внятный handoff в исполнение
- для соло-разработчиков, которым нужен устойчивый план перед длинным run
- в первую очередь для пользователей Codex, а также для пользователей Claude Code, которым хочется меньше тех-шума и больше ясности

## Установка

### Codex

Можно просто попросить Codex:

```text
Use $skill-installer to install https://github.com/serejaris/justdoit/tree/main/skills/justdoit
```

Эту строку можно просто дать другому агенту Codex, и он сам поставит скилл из репозитория.

Ручная установка:

```bash
cp -R skills/justdoit ~/.codex/skills/
```

После установки перезапусти Codex.

### Claude Code

Формат `SKILL.md` совместим со скиллами Claude Code. Скопируй директорию скилла:

```bash
# На уровне пользователя (доступно во всех проектах)
mkdir -p ~/.claude/skills
cp -R skills/justdoit ~/.claude/skills/

# На уровне проекта (только в этом проекте)
mkdir -p .claude/skills
cp -R skills/justdoit .claude/skills/
```

Потом используй `/justdoit <задача>` в Claude Code.

## Структура Репозитория

- [skills/justdoit](./skills/justdoit/) — сам installable skill
- [assets/justdoit.gif](./assets/justdoit.gif) — визуал для README
- локальные execution docs вроде `plans.md`, `status.md` и `test-plan.md` генерируются во время run и специально не коммитятся в этот репозиторий

## Чем Отличается

- сначала читает репозиторий, потом планирует
- создаёт durable-файлы, а не живёт только в памяти чата
- формулирует запуск на человеческом языке
- ждёт явного `запускай`, прежде чем перейти в execution loop

## Лицензия

MIT
