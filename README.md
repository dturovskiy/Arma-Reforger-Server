# Arma Reforger Dedicated Server (LinuxGSM)

Репозиторій з інструкціями та шаблонами для адміністрування **Arma Reforger Dedicated Server** через **LinuxGSM** (`armarserver`).

## 📌 Основна інструкція

➡️ **[Повний гайд](/docs/instruction.md)**

---

## 🧭 Швидкий старт (LinuxGSM)

> Детально — у повній інструкції вище.

### Встановлення

```bash
adduser armarserver
su - armarserver
curl -Lo linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh armarserver
./armarserver install
```

### Базові команди

```bash
./armarserver start
./armarserver stop
./armarserver restart
./armarserver console
./armarserver update
./armarserver details
```

---

## 📁 Рекомендована структура репозиторію

```text
.
├─ README.md
└─ docs/
   └─ instruction.md
```

---

## 🔗 Офіційні джерела

* LinuxGSM: Arma Reforger (`armarserver`): [https://linuxgsm.com/servers/armarserver/](https://linuxgsm.com/servers/armarserver/)
* SteamCMD (LinuxGSM docs): [https://docs.linuxgsm.com/steamcmd](https://docs.linuxgsm.com/steamcmd)
* Steam-гайд (server.json + SteamCMD): [https://steamcommunity.com/sharedfiles/filedetails/?id=2809849636](https://steamcommunity.com/sharedfiles/filedetails/?id=2809849636)

Додатково (Bohemia Interactive):

* Startup Parameters: [https://community.bistudio.com/wiki/Arma_Reforger:Startup_Parameters#Hosting](https://community.bistudio.com/wiki/Arma_Reforger:Startup_Parameters#Hosting)
* Server Config: [https://community.bistudio.com/wiki/Arma_Reforger:Server_Config](https://community.bistudio.com/wiki/Arma_Reforger:Server_Config)
* Server Hosting: [https://community.bistudio.com/wiki/Arma_Reforger:Server_Hosting](https://community.bistudio.com/wiki/Arma_Reforger:Server_Hosting)

---

## 🤝 Внесок

PR/Issue вітаються: покращення інструкції, типові кейси (NAT/ports), приклади `server.json`, нотатки по модах та автоматизації.

## 📄 Ліцензія

Додайте LICENSE за потреби (наприклад, MIT).
