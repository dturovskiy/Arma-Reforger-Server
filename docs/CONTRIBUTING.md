# Як правильно редагувати репозиторій Arma-Reforger-Server

Цей репозиторій — **документація + шаблони** для адміністрування Arma Reforger Dedicated Server через LinuxGSM (`armarserver`), включно з SOP/планами, моддингом і сценаріями. :contentReference[oaicite:1]{index=1}

## 1) Принципи (щоб не “розвалити” репо)

### 1.1. Репо — це “джерело правди” по процесам
- Будь-яка інструкція повинна:
  - мати **чіткий результат** (що має отримати адмін в кінці),
  - містити **команди/файли/шляхи**,
  - вказувати **де джерело істини** (найчастіше `./armarserver details` + `server.json`). :contentReference[oaicite:2]{index=2}

### 1.2. Не зберігай секрети
- Заборонено комітити:
  - паролі (`adminPassword`, RCon, panel passwords),
  - приватні IP/домен,
  - токени, ключі, приватні ключі SSH.
- Для прикладів використовуй плейсхолдери: `YOUR_ADMIN_PASSWORD`, `YOUR_SERVER_IP` тощо. :contentReference[oaicite:3]{index=3}

### 1.3. Мінімум “магії”, максимум відтворюваності
- Якщо додаєш новий SOP — додавай **чекліст** + **критерій готовності** (“що означає, що зроблено правильно”).

---

## 2) Структура і “що куди правити”

### 2.1. `README.md` — тільки навігація + короткий зміст
`README.md` має залишатися індексом:
- посилання на ключові документи в `docs/`,
- короткий quick-start (5–15 рядків),
- офіційні джерела,
- банер/скріншот в кінці (опційно). :contentReference[oaicite:4]{index=4}

**Не перетворюй README в “книгу”** — деталі виносимо в `docs/*`.

### 2.2. `docs/instruction.md` — адмінка LinuxGSM (операційний гайд)
Тут живе:
- інсталяція, запуск/stop/restart,
- update/validate/backup,
- troubleshooting (логи/debug),
- базові правила роботи адміна. :contentReference[oaicite:5]{index=5}

### 2.3. `docs/PLAN.md` — план запуску + SOP чеклісти
Тут живе:
- хостинг → мережа/порти → конфіг → сценарій → моди → регламенти. :contentReference[oaicite:6]{index=6}

### 2.4. `docs/MODDING.md` — Workbench/Workshop/залежності
Тут живе:
- офіційні лінки BI,
- “Hello World mod”,
- як підключати сценарій-мод через `mods[]` + `scenarioId`. :contentReference[oaicite:7]{index=7}

### 2.5. `docs/SCENARIO_PLAN.md` — план розробки PvP 128 сценарію
Тут живе:
- дизайн-спека станів POI/баз,
- прогрес/відкат через ретранслятори,
- ліміт 6 баз, No-Assault HQ,
- цивільні як штраф/анти-аб’юз,
- етапи робіт + тест-план. :contentReference[oaicite:8]{index=8}

### 2.6. `assets/` — картинки, діаграми, банери
- Тримай **один стиль імен**: `repo-banner.png`, `diagram-frontline.png`, `server-json-example.png`.
- Для README краще **PNG** (надійніше рендериться всюди), SVG — тільки якщо він “простий” і без текстового рендер-хаосу.

---

## 3) Робочий процес редагування (правильно)

### Варіант A — через Git (рекомендовано)
1) Форк або клон:
```bash
git clone https://github.com/dturovskiy/Arma-Reforger-Server.git
cd Arma-Reforger-Server
````

2. Створи гілку під задачу:

```bash
git checkout -b docs/ports-sop
# або: fix/readme-links, feat/scenario-section, chore/assets-banner
```

3. Правки → коміт:

```bash
git status
git add README.md docs/PLAN.md
git commit -m "docs: уточнено SOP по портам і NAT"
```

4. Push → PR:

```bash
git push -u origin docs/ports-sop
```

5. У PR описуй:

* що змінилось,
* де перевірено (preview markdown),
* чи не поламались лінки/шляхи.

### Варіант B — GitHub Web UI (швидко, але акуратно)

* Для дрібних правок (опечатки, посилання).
* Для великих змін — краще Git локально.

---

## 4) Стандарти Markdown (щоб все було читабельно)

### 4.1. Заголовки

* Один `#` на документ.
* Розділи: `##`, підрозділи: `###`.

### 4.2. Код-блоки завжди з мовою

* bash:

```bash
./armarserver details
```

* json:

```json
{ "game": { "mods": [] } }
```

* cron:

```cron
*/5 * * * * /home/armarserver/armarserver monitor > /dev/null 2>&1
```

### 4.3. Чеклісти

В SOP/PLAN — тільки чеклісти:

* [ ] дія
* [ ] перевірка
* [ ] критерій “готово”

### 4.4. Посилання

* Внутрішні: `/docs/PLAN.md`
* Зовнішні: повні URL.
* Після перейменування файлів — **обов’язково** пройтись по README і виправити лінки.

---

## 5) Перевірки перед merge (мінімальний QA)

### 5.1. Лінки

* Відкрий README і кожен документ в GitHub preview.
* Клікни всі внутрішні посилання (щоб не було 404).

### 5.2. JSON-приклади

* Якщо додаєш `server.json` приклад — перевір `jq`:

```bash
jq . docs/examples/server.json >/dev/null
```

### 5.3. Уникай “брехні в інструкціях”

* Якщо пишеш “порти такі-то” — підкреслюй: **джерело істини = `./armarserver details`**. ([GitHub][2])

---

## 6) Рекомендовані поліпшення структури (за потреби)

### 6.1. Додай `docs/examples/`

Туди:

* `server.json.example`
* приклади `mods.json` (набір модів)
* шаблони systemd/ufw

### 6.2. Додай `CHANGELOG.md`

Фіксуй:

* зміни в SOP,
* зміни модсету,
* зміни сценарію.

### 6.3. Додай `CONTRIBUTING.md`

Якщо репо публічне і очікуються PR/Issue — винеси туди правила з цього документа.

---

## 7) Відомі “хвости”, які варто виправляти при правках

* У README в блоці “структура репозиторію” має бути актуальна назва файлу сценарію (`docs/SCENARIO_PLAN.md`). Якщо там інша назва — синхронізувати. ([GitHub][3])
* У `LICENSE` замінити `YOUR_NAME_OR_ORG` на реального автора/орг. ([GitHub][4])

---

[1]: https://github.com/dturovskiy/Arma-Reforger-Server "GitHub - dturovskiy/Arma-Reforger-Server: UA гайди для Arma Reforger Dedicated Server на Linux через LinuxGSM: розгортання, конфіг, сценарії, моди, SOP."
[2]: https://raw.githubusercontent.com/dturovskiy/Arma-Reforger-Server/main/docs/instruction.md "raw.githubusercontent.com"
[3]: https://raw.githubusercontent.com/dturovskiy/Arma-Reforger-Server/main/README.md "raw.githubusercontent.com"
[4]: https://raw.githubusercontent.com/dturovskiy/Arma-Reforger-Server/main/LICENSE "raw.githubusercontent.com"
