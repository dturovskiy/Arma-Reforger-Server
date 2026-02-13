# ✅ Arma Reforger Dedicated Server (LinuxGSM) — як правильно працювати із сервером

Ця інструкція — для адміністрування сервера через **LinuxGSM** (скрипт `armarserver`). Також є короткий блок для **Windows/SteamCMD**.

---

## 🔗 Офіційні інструкції (джерела)

* LinuxGSM: Arma Reforger (`armarserver`): [https://linuxgsm.com/servers/armarserver/](https://linuxgsm.com/servers/armarserver/)
* SteamCMD (LinuxGSM docs): [https://docs.linuxgsm.com/steamcmd](https://docs.linuxgsm.com/steamcmd)
* Steam-гайд (приклад `server.json` + SteamCMD): [https://steamcommunity.com/sharedfiles/filedetails/?id=2809849636](https://steamcommunity.com/sharedfiles/filedetails/?id=2809849636)

Додатково (документація Bohemia Interactive):

* Startup Parameters: [https://community.bistudio.com/wiki/Arma_Reforger:Startup_Parameters#Hosting](https://community.bistudio.com/wiki/Arma_Reforger:Startup_Parameters#Hosting)
* Server Config: [https://community.bistudio.com/wiki/Arma_Reforger:Server_Config](https://community.bistudio.com/wiki/Arma_Reforger:Server_Config)
* Server Hosting: [https://community.bistudio.com/wiki/Arma_Reforger:Server_Hosting](https://community.bistudio.com/wiki/Arma_Reforger:Server_Hosting)

---

## 0) Важливе перед стартом (сумісність / база)

* LinuxGSM `armarserver` розрахований на популярні дистрибутиви (Ubuntu 20.04 LTS / Debian 11 / EL8) та 64-bit.
* Мінімум: tmux >= 1.6, glibc >= 2.27 (якщо дистрибутив інший — перевіряйте вручну).
* Steam AppID сервера: **1874900** (Arma Reforger Dedicated).

---

## 1) Встановлення через LinuxGSM (рекомендовано)

> Робимо окремого користувача під сервер (безпека).

```bash
adduser armarserver
su - armarserver
curl -Lo linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh armarserver
./armarserver install
```

ℹ️ Під час `install` LinuxGSM може поставити залежності автоматично (через sudo або якщо запускати як root).

---

## 2) Команди щоденного життя (запуск/зупинка/консоль)

Усі команди:

```bash
./armarserver
```

Найчастіші:

```bash
./armarserver start
./armarserver stop
./armarserver restart
```

Жива консоль (якщо підтримується):

```bash
./armarserver console
```

Вийти з консолі **правильно**: `CTRL+b`, потім `d`.

⚠️ `CTRL+c` — **вбиває** сервер.

---

## 3) Оновлення сервера (правильний процес)

Стандартно (перевіряє і перезапускає лише якщо треба):

```bash
./armarserver update
```

Примусове оновлення напряму через SteamCMD:

```bash
./armarserver force-update
```

Перевірка цілісності (SteamCMD validate):

```bash
./armarserver validate
```

Оновлення самого LinuxGSM:

```bash
./armarserver update-lgsm
```

---

## 4) Де конфіг, порти, паролі? — команда details (обовʼязково)

Перед тим як щось відкривати у firewall/роутері або редагувати конфіги:

```bash
./armarserver details
```

✅ `details` показує ключове: локацію, конфіг-файли, порти, параметри, і це найнадійніший спосіб не помилитися.

---

## 5) Налаштування сервера (server.json) — приклад і логіка

Arma Reforger керується JSON-конфігом (часто це `server.json`). Зазвичай він містить: назву/регіон, bind address/port (ігровий порт), admin password, сценарій (scenarioId), ліміт гравців, видимість/пароль, моди (масив `mods`).

Приклад (адаптуйте під себе):

```json
{
  "dedicatedServerId": "ar-gm-%profilename",
  "region": "EU",
  "gameHostBindAddress": "YOUR_SERVER_IP",
  "gameHostBindPort": 2001,
  "gameHostRegisterBindAddress": "YOUR_SERVER_IP",
  "gameHostRegisterPort": 2001,
  "adminPassword": "YOUR_ADMIN_PASSWORD",
  "game": {
    "name": "YOUR_SERVER_NAME",
    "scenarioId": "{59AD59368755F41A}Missions/21_GM_Eden.conf",
    "playerCountLimit": 16,
    "visible": true,
    "password": "",
    "supportedGameClientTypes": ["PLATFORM_PC"],
    "gameProperties": {
      "serverMaxViewDistance": 1600,
      "battleEye": true,
      "fastValidation": true
    },
    "mods": []
  }
}
```

⚠️ Де саме лежить `server.json` у вашій інсталяції LinuxGSM — **дивіться через** `./armarserver details`.

---

## 6) Логи, діагностика, “чому не стартує/вилітає”

Логи зазвичай тут:

```bash
/home/armarserver/logs
```

Debug-режим (показує “живий” вивід у термінал):

```bash
./armarserver debug
```

---

## 7) Backup (резервні копії)

```bash
./armarserver backup
```

Створює архів tar.bz2 всього сервера (корисно перед оновленнями/експериментами).

---

## 8) Автоматизація (cron) — рекомендований мінімум

Відкрити редактор cron:

```bash
crontab -e
```

Рекомендовані задачі:

```cron
*/5 * * * * /home/armarserver/armarserver monitor > /dev/null 2>&1
*/30 * * * * /home/armarserver/armarserver update > /dev/null 2>&1
0 0 * * 0 /home/armarserver/armarserver update-lgsm > /dev/null 2>&1
```

---

## 9) SteamCMD: безпека логіна (важливо!)

* Для SteamCMD інколи потрібен Steam-логін.
* **Рекомендовано створити окремий Steam-акаунт тільки для сервера.**
* Не використовуйте основний акаунт: пароль може зберігатися/світитися у plain text та/або логах.
* Steam Guard через смартфон може бути проблемним для авто-оновлень; email-код часто простіший.

Деталі: [https://docs.linuxgsm.com/steamcmd](https://docs.linuxgsm.com/steamcmd)

---

## 10) Якщо сервер “не видно” у грі

1. Перевірте, що сервер працює:

```bash
./armarserver monitor
./armarserver details
```

2. Переконайтесь, що відкриті порти = ті, що у `details` + ті, що задані у `server.json` (наприклад, часто 2001).
3. Якщо ви за NAT — зробіть port forward на роутері на IP машини з сервером.
4. Для тесту — підключайтесь Direct Join по IP:PORT у грі.

---

## 11) Коротко: Windows/SteamCMD (якщо НЕ LinuxGSM)

(для довідки; повний гайд у Steam-джерелі)

* Завантажити SteamCMD, встановити серверні файли: `+login anonymous +force_install_dir ... +app_update 1874900 +quit`.
* Запуск: `ArmaReforgerServer.exe -config "...server.json" -profile "...saves"`.

Гайд: [https://steamcommunity.com/sharedfiles/filedetails/?id=2809849636](https://steamcommunity.com/sharedfiles/filedetails/?id=2809849636)

---

## ✅ Рекомендований робочий процес адміна

1. Змінюєте налаштування → `./armarserver restart`.
2. Перед великими змінами → `./armarserver backup`.
3. Регулярно → `./armarserver update`.
4. Якщо проблеми → `./armarserver debug` + дивимось `/home/armarserver/logs`.
5. Порти/конфіги/шляхи → завжди через `./armarserver details`.
