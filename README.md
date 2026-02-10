# Arma-Reforger-Server

У Reforger “арсенал” **не має єдиного глобального списку**. Він будує доступні предмети з двох джерел:

1. **Entity Catalog типу `ITEM`** (або *фракційний*, або *загальний / non-faction*) — там для кожного prefab додається “Arsenal data” у вигляді **`SCR_ArsenalItem`**. Менеджер каталогів вміє брати арсенальні предмети окремо з **General Catalog `ITEM`** і з **Faction Catalog `ITEM`**, або з усіх одразу. ([BIS Community][1])
2. Опціонально: **Overwrite Arsenal Config** прямо на об’єкті арсеналу (через `SCR_ArsenalItemListConfig`) — це “переписати список предметів для конкретного арсеналу”. `SCR_ArsenalComponent` має `m_OverwriteArsenalConfig` і методи для отримання “filtered overwrite items”. ([BIS Community][2])

---

## Важливий нюанс про “працює всюди незалежно від сценарію”

**Абсолютно “всюди” зробити неможливо**, бо:

* сценарій може **взагалі не використовувати** стандартні арсенали або може мати **кастомні** (інші prefabs/логіку),
* сценарій/гейммод може **вимкнути типи арсеналу** або фільтрувати інакше. ([BIS Community][2])

Але ти можеш зробити **максимально універсально** для більшості випадків (Conflict/GM і купи кастомних місій), якщо реалізуєш *обидва шари* нижче.

---

# Шар A (найсумісніший): додати зброю в арсенал **кожної фракції** через Faction `ITEM` Catalog

Це “правильний” шлях, який узгоджується з тим, як арсенал задуманий (фракційні списки). Менеджер каталогів прямо має API “дай арсенальні предмети фракції” і бере їх з **Faction Catalog `ITEM`**. ([BIS Community][1])

### Як робиться на практиці

1. **Контент-мод**: твоя зброя як prefab (weapon item).
2. **Arsenal-patch мод** (окремий невеличкий мод):

   * додає dependency на контент-мод зі зброєю,
   * **override/inherit** фракційний `ITEM`-каталог кожної фракції, яку хочеш підтримати (US / USSR / FIA / Civilian / і т.д.),
   * у каталозі додає **новий запис (Entry)**:

     * `Entity Prefab` = твій weapon prefab
     * `Entity Data List` → додаєш **`SCR_ArsenalItem`** і налаштовуєш тип/режим/відображення/вартість/ранг тощо.
3. Повторюєш те саме для **магазинів, обвісів, боєприпасів**, якщо хочеш щоб вони теж були в арсеналі як окремі предмети.

### Чому це не “для будь-якої фракції автоматично”

Бо в коді менеджера каталогів прямо є обмеження: для фракції “тільки один каталог кожного типу” у списку, тому додавання нового “паралельного” `ITEM` каталогу без зміни фракційної конфігурації не завжди спрацює — найчастіше треба саме **override** існуючого `ITEM` каталогу й додати туди свій entry. ([Arma Reforger Explorer][3])

### Конфлікти з іншими модами (дуже часта проблема)

Якщо інший мод теж **override** той самий фракційний `ITEM`-каталог — один з вас “затреться”. Тому популярний підхід — “soft adding”: робиш один “збірний” override-каталог і руками **зливаєш** туди entries з модів-залежностей. Це буквально описують автори таких “arsenal config” модів. ([Arma Reforger][4])

---

# Шар B (найближче до “працює будь-де”): Overwrite Arsenal Config на об’єктах арсеналу

Ціль: навіть якщо фракція інша/кастомна, але в світі стоїть арсенал із `SCR_ArsenalComponent`, ти можеш підсунути йому **персональний список предметів** через `SCR_ArsenalItemListConfig`. ([BIS Community][2])

### Як це використати “універсально”

1. Створюєш `SCR_ArsenalItemListConfig` зі списком твоїх предметів.
2. **Override** стандартні prefabs арсеналів/arsenal boxes (ті, які реально ставляться в Conflict/GM/місіях) і:

   * в полі `OverwriteArsenalConfig` вказуєш свій config,
   * вимикаєш перевірку фракції для overwrite (є відповідний прапорець у компоненті; в API видно що така логіка існує як `m_bCheckFactionForOverwriteArsenalConfig`). ([BIS Community][5])

Це дає ефект “де б не був цей конкретний vanilla-арсенал — там буде моя зброя”.

**Але** якщо сценарій використовує **інші** арсенали (кастомні prefabs) — їх теж треба патчити/override.

---

## Діагностика: як перевірити, що зброя реально потрапила в арсенальні каталоги

Ти можеш у скрипті (Enfusion) разово вивести, чи бачить гра твій предмет як `SCR_ArsenalItem` у каталогах. Є `SCR_EntityCatalogManagerComponent.GetInstance()` і `GetAllArsenalItems()`. ([BIS Community][6])
Важливо: каталоги можуть бути не ініціалізовані “в той самий кадр”, у коді навіть є підказка викликати логіку **на кадр пізніше**. ([Arma Reforger Explorer][7])

---

## Підсумок, що робити тобі

Якщо тобі треба “максимально всюди”:

* **Обов’язково**: Шар A — додати твій weapon prefab + `SCR_ArsenalItem` у `ITEM` каталоги **всіх vanilla фракцій**, які реально використовуються на серверах (US/USSR/FIA/Civ). ([BIS Community][1])
* **Додатково**: Шар B — overwrite стандартні arsenal prefabs через `SCR_ArsenalItemListConfig`, щоб зброя з’являлась навіть поза “правильними” фракційними листами. ([BIS Community][2])

---

[1]: https://community.bistudio.com/wikidata/external-data/arma-reforger/ArmaReforgerScriptAPIPublic/interfaceSCR__EntityCatalogManagerComponent.html "Arma Reforger Script API: SCR_EntityCatalogManagerComponent Interface Reference"
[2]: https://community.bistudio.com/wikidata/external-data/arma-reforger/ArmaReforgerScriptAPIPublic/interfaceSCR__ArsenalComponent.html "Arma Reforger Script API: SCR_ArsenalComponent Interface Reference"
[3]: https://arexplorer.zeroy.com/_s_c_r___entity_catalog_manager_component_8c.html "Arma Reforger Explorer: scripts_Arma_Reforger_v1.1.0.42/scripts/Game/EntityCatalog/SCR_EntityCatalogManagerComponent.c File Reference"
[4]: https://reforger.armaplatform.com/workshop/66DED7D8E3BF7E8D-ArsenalBox-SoftAddingMods "Arsenal Box - Soft Adding Mods - Arma Reforger Workshop"
[5]: https://community.bistudio.com/wikidata/external-data/arma-reforger/ArmaReforgerScriptAPIPublic/interfaceSCR__ArsenalComponent-members.html "Arma Reforger Script API: Member List"
[6]: https://community.bistudio.com/wikidata/external-data/arma-reforger/ArmaReforgerScriptAPIPublic/interfaceSCR__EntityCatalogManagerComponent.html?utm_source=chatgpt.com "Arma Reforger Script API"
[7]: https://arexplorer.zeroy.com/_s_c_r___faction_8c_source.html?utm_source=chatgpt.com "SCR_Faction.c - Arma Reforger Explorer"
