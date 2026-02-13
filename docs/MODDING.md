# Arma Reforger — як робити моди (Workbench) + офіційні посилання

## 1) Офіційні посилання (Bohemia Interactive)

* Modding категорія (вхідна точка): [https://community.bistudio.com/wiki/Category:Arma_Reforger/Modding](https://community.bistudio.com/wiki/Category:Arma_Reforger/Modding)
* Workshop (як працює сховище модів): [https://community.bistudio.com/wiki/Arma_Reforger:Workshop](https://community.bistudio.com/wiki/Arma_Reforger:Workshop)
* Mod Project Setup (створення проекту): [https://community.bistudio.com/wiki/Arma_Reforger:Mod_Project_Setup](https://community.bistudio.com/wiki/Arma_Reforger:Mod_Project_Setup)
* Mod Publishing Process (збірка/публікація): [https://community.bistudio.com/wiki/Arma_Reforger:Mod_Publishing_Process](https://community.bistudio.com/wiki/Arma_Reforger:Mod_Publishing_Process)
* Scenario Framework (створення сценаріїв без скриптингу): [https://community.bistudio.com/wiki/Arma_Reforger:Scenario_Framework](https://community.bistudio.com/wiki/Arma_Reforger:Scenario_Framework)
* Scenario Framework Setup Tutorial: [https://community.bistudio.com/wiki/Arma_Reforger:Scenario_Framework_Setup_Tutorial](https://community.bistudio.com/wiki/Arma_Reforger:Scenario_Framework_Setup_Tutorial)
* Capture & Hold Setup (приклад типового сценарію): [https://community.bistudio.com/wiki/Arma_Reforger:Capture_%26_Hold_Setup](https://community.bistudio.com/wiki/Arma_Reforger:Capture_%26_Hold_Setup)
* Startup Parameters (у т.ч. -listScenarios): [https://community.bistudio.com/wiki/Arma_Reforger:Startup_Parameters](https://community.bistudio.com/wiki/Arma_Reforger:Startup_Parameters)
* Server Config (приклади JSON, mods/scenarioId): [https://community.bistudio.com/wiki/Arma_Reforger:Server_Config](https://community.bistudio.com/wiki/Arma_Reforger:Server_Config)

Офіційні приклади від Bohemia (GitHub):

* Arma Reforger Samples: [https://github.com/BohemiaInteractive/Arma-Reforger-Samples](https://github.com/BohemiaInteractive/Arma-Reforger-Samples)

Офіційний Workshop-портал (з modId/version):

* [https://reforger.armaplatform.com/workshop](https://reforger.armaplatform.com/workshop)

---

## 2) Інструменти

* Встановіть **Arma Reforger Tools / Workbench** у Steam (розділ Tools).
* Переконайтесь, що встановлена гра Arma Reforger (для локального тесту підписаних/запакованих модів).

---

## 3) “Hello World” мод — мінімальний шлях

### 3.1 Створення проекту

* [ ] Створити новий Mod Project у Workbench
* [ ] Вказати назву/ID проекту
* [ ] Перевірити, що проект зʼявився у списку

### 3.2 Додавання простого контенту

* [ ] Створити папку під ваші ресурси (prefabs/configs)
* [ ] Додати/успадкувати існуючий префаб (найбезпечніший шлях)
* [ ] Зберегти, перевірити відсутність помилок ресурсів

### 3.3 Локальний тест

* [ ] Запустити гру з підключеним локальним модом
* [ ] Перевірити, що контент видно/працює

---

## 4) Моди-сценарії (Missions/*.conf) — практичний робочий цикл

### 4.1 Створення кастомної місії (Scenario Framework)

* [ ] Відкрити World Editor
* [ ] Додати потрібні обʼєкти (спавни, зони, точки)
* [ ] Налаштувати Scenario Framework компоненти
* [ ] Зберегти місію як `.conf` у `Missions/`

### 4.2 Пакування/публікація

* [ ] Зібрати мод (bundle)
* [ ] Publish Project → Workshop
* [ ] Перевірити сторінку мода/сценарію на Arma Platform: бачимо `modId` і `version`

### 4.3 Підключення сценарію на сервері

* [ ] Додати мод у `mods[]` (server.json)
* [ ] Вказати `scenarioId` на ваш `.conf`
* [ ] Перезапустити сервер і протестувати підключення клієнта

---

## 5) Практичні поради (щоб не згоріти)

* Робіть окремий “test branch/сервер” для експериментів з модами
* Не змішуйте десятки модів без поетапної перевірки: додавайте партіями
* Якщо сценарій не вантажиться — майже завжди:

  * неправильний `scenarioId` (шлях або GUID)
  * мод не доданий у `mods[]`
  * мод/залежність не докачалась або version mismatch
* Публікуйте релізи з changelog і фіксуйте версії (коли потрібна стабільність)

---

## 6) Де брати modId/name/version швидко

* Arma Platform Workshop сторінка мода: [https://reforger.armaplatform.com/workshop](https://reforger.armaplatform.com/workshop)
* У грі в Workshop інколи є “JSON copy” для набору модів (зручно для server.json)

---

## 7) Рекомендовані “наступні кроки” для команди

* [ ] Завести `docs/mods/` і вести список: “обовʼязкові”, “опціональні”, “заборонені”
* [ ] Вести `CHANGELOG.md` для сервера (апдейти/моди/сценарії)
* [ ] Додати “release checklist” перед оновленням прод-сервера
