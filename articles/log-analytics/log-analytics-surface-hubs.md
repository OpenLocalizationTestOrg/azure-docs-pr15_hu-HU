<properties
    pageTitle="A napló Analytics felszíni hubok figyelése |} Microsoft Azure"
    description="A felület központi megoldás segítségével a felület hubok állapotának nyomon követése és azok éppen használata."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Monitor felszíni hubok a napló Analytics

Ez a cikk azt ismerteti, hogyan használhatja a felület központi megoldást napló Analytics Lync-Microsoft Surface központi eszközök a Microsoft műveletek Management csomagja (MOBILE). Log Analytics könnyebben nyomon követheti, valamint azokat éppen használata a felület hubok állapotának.

Minden egyes Surface-hubhoz a Microsoft figyelése Agent. Annak az, hogy küldhet adatok a felület-központban MOBILE ügynök keresztül. A felület hubok és a olvassák naplófájlok, majd a MOBILE szolgáltatás elküldi. Problémák kiszolgálók készül, offline, a naptár nem szinkronizálja, például, vagy ha az eszköz nem lehet bejelentkezni a Skype látható MOBILE a felület központi irányítópulton. Az adatokat az Irányítópult segítségével megállapítható eszközök, amelyek nem fut, vagy, egyéb problémák merülnek fel, és az észlelt probléma javítását esetleg alkalmazása.


## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás

Az alábbi információk segítségével telepítse és állítsa be a megoldást. Annak érdekében, hogy a felület hubok felügyeletét látja el a a Microsoft műveletek Management csomagja (MOBILE), a következőkre lesz szüksége:

- [MOBILE](http://www.microsoft.com/oms)érvényes előfizetést.
- Egy [MOBILE előfizetés](https://azure.microsoft.com/pricing/details/log-analytics/) szintet, amelyek támogatják a figyelni kívánt eszközök száma. MOBILE árak függ attól függően, hogy hány eszközök tehát vannak, és hogy mennyi adatot, folyamatok. Célszerű figyelembe venni ezt, a felület központi bevezetési tervezésekor.

Ezután, fog egy MOBILE előfizetés hozzáadása meglévő Microsoft Azure-előfizetéséhez, vagy közvetlenül a MOBILE portál új munkaterület létrehozása. Módszerek valamelyikével vonatkozó részletes útmutatást a problémának [napló Analytics – első lépések](log-analytics-get-started.md). Miután beállította a MOBILE előfizetés, kétféleképpen igényléséhez felület központi eszközén:

- Automatikusan InTune keresztül
- Manuálisan a **Beállítások** felület központi eszközén.

## <a name="set-up-monitoring"></a>Figyelés beállítása

Az állapot és a napló Analytics használata MOBILE Surface-központját tevékenységének figyelheti. A felület-központját MOBILE igényelhet InTune, vagy helyben **beállításainak** segítségével a az Surface-központban.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Csatlakozás felszíni hubok MOBILE InTune keresztül

A munkaterület-azonosító és a munkaterület kulcsot kell a MOBILE munkaterület, a felület hubok fogja kezelni. Azok a MOBILE portálról elérheti.

InTune Microsoft-termékek, amely lehetővé teszi, hogy a MOBILE beállítások egy vagy több eszközről alkalmazott központi kezelésére. Kövesse ezeket a lépéseket követve állítsa be az InTune eszközökkel:

1. Jelentkezzen be az InTune.
2. Keresse meg a **beállításai** > **csatlakoztatott adatforrásból**.
3. Hozzon létre, vagy egy, a felület központi sablonon alapuló házirend szerkesztéséhez.
4. Nyissa meg a házirend a MOBILE (Azure műveleti háttérismeretek) szakaszt, és adja hozzá a házirend *Munkaterület azonosító* és a *Munkaterület billentyűt* .
5. Mentse a házirend.
6. A házirend hozzárendelése a megfelelő eszközök csoportját.

  ![InTune házirend](./media/log-analytics-surface-hubs/intune.png)

InTune és a MOBILE beállításai majd szinkronizál a cél csoportjában a MOBILE munkaterületen igénylése azokat az eszközöket.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Csatlakozás felület hubok MOBILE beállítások alkalmazás használatával

A munkaterület-azonosító és a munkaterület kulcsot kell a MOBILE munkaterület, a felület hubok fogja kezelni. Azok a MOBILE portálról elérheti.

Ha nem használja az InTune kezelése a környezettől, igényelhet eszközök manuálisan a **beállításokat** egyes felület központi keresztül:

1. A felület-központban nyissa meg a **beállításokat**.
2. Írja be az eszközt rendszergazdai hitelesítő adatait kérő.
3. Jelölje ki **az eszközt**, és a **Figyelés**, csoportban kattintson a **MOBILE beállítások megadása**.
4. Engedélyezze a **figyelése**.
6. A MOBILE beállításai párbeszédpanelen írja be a **Munkaterület-azonosító** , és írja be a **Munkaterület billentyűt**.  
  ![beállítások](./media/log-analytics-surface-hubs/settings.png)
7. Kattintson az **OK gombra** a konfigurálás befejezéséhez.

Egy megerősítő ablak, engedélyezi vagy tiltja a MOBILE konfiguráció lett sikeresen az eszközre alkalmazott. Ha volt, megjelenik egy üzenet arról, hogy a agent sikerült csatlakoztatni a MOBILE szolgáltatás. Az eszköz megkezdi adatokat küld a MOBILE, ahol megtekintheti, és azt működésbe lépnek.

## <a name="monitor-surface-hubs"></a>Felületi hubok monitor

A felület hubok figyelése MOBILE használatával hasonlít sokkal bármely más beiktatott eszközök figyelése.

1. Jelentkezzen be a MOBILE portálra.
2. Nyissa meg a felület központi megoldás csomag irányítópult.
3. Az eszköz állapot látható.

  ![Felületi központ irányítópult](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

[Értesítések](log-analytics-alerts.md) meglévő vagy egyéni napló keresések alapján hozhat létre. Az adatok, a felület hubok gyűjti össze a MOBILE használata esetén is kereshet problémák és a megadott feltételeknek megfelelő eszközén meghatározó értesítés.


## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes felület központi adatok megtekintéséhez.
- [Értesítések](log-analytics-alerts.md) értesítést küld a felület hubok problémákat létrehozása.
