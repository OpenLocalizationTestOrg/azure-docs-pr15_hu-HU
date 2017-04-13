<properties 
   pageTitle="StorSimple pillanatkép Manager felhasználói felület |} Microsoft Azure"
   description="A StorSimple pillanatkép Manager felhasználói felület és biztonsági másolat feladatok és a biztonságimásolat-katalógus kezelése használatával ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/25/2016"
   ms.author="v-sharos" />

# <a name="storsimple-snapshot-manager-user-interface"></a>StorSimple pillanatkép Manager felhasználói felület

## <a name="overview"></a>– Áttekintés

A StorSimple pillanatkép kezelő rendelkezik használható felhasználói felületet szeretne készíteni, majd a biztonsági másolatok kezelésére használható. Ebben az oktatóanyagban áttekintése a felhasználói felület, és majd megtudhatja, hogyan használhatja az egyes összetevők. Részletes leírását a StorSimple-pillanatfelvétel kezelő, lásd: [StorSimple pillanatkép Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konzol leírása

A felhasználói felület megtekintéséhez kattintson az asztalon StorSimple pillanatkép Manager ikonra. A konzol ablak, az alábbi ábrán látható módon.

![Ablaktáblák StorSimple pillanatkép Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

A konzol ablak tartalmaz az öt fő elemet. Kattintson a minden elem leírását a megfelelő hivatkozásra.

- [Menüsor](#menu-bar) 
- [Eszköztár](#tool-bar) 
- [Hatókör ablaktáblán](#scope-pane) 
- [Eredmények ablaktáblája](#results-pane) 
- [Műveletek ablaktáblában](#actions-pane) 

Emellett a StorSimple pillanatkép kezelő [billentyűparancsokkal és számos parancsikont](#keyboard-navigation-and-shortcuts)támogatja.

### <a name="console-accessibility"></a>Konzol kisegítő lehetőségek

A StorSimple pillanatkép Manager felhasználói felület támogatja a kisegítő lehetőségek a Windows operációs rendszer és a Microsoft Management Console (MMC), valamint bizonyos StorSimple pillanatkép Manager-specifikus billentyűparancsot által biztosított. 

- A Windows kisegítő lehetőségek leírását ugorjon a [billentyűparancsok a Windows](https://support.microsoft.com/kb/126449). 

- Az MMC kisegítő lehetőségek leírását kattintson a [Kisegítő lehetőségek az MMC 3.0](https://technet.microsoft.com/library/cc766075.aspx)

- A StorSimple pillanatkép kezelő kisegítő lehetőségek leírását lépjen a [billentyűparancsokkal és kezelésére szolgáló billentyűparancsok](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Menüsor

A menüsor megjelenítése az konzol ablak tetején [fájl](#file-menu), [művelet](#action-menu), [Nézet](#view-menu), [a Kedvencek](#favorites-menu), [ablak](#window-menu)és menük [segítségével](#help-menu) tartalmazza.

Kattintson a minden elem a menüsávon kattintson a menü az elérhető parancsok megjelenítéséhez. A következő példa bemutatja a **Nézet** menü kijelölt a menüsávon.

![A Nézet menü, kijelölt](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Fájl menü

A **fájl** menü tartalmazza a szabványos Microsoft Management Console (MMC) parancsot.

#### <a name="menu-access"></a>Az access menü

A **fájl** menü megjelenítéséhez kattintson a **fájl** a menüsávon. A következő menü jelenik meg.

![StorSimple pillanatkép Manager fájl menüje](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menü leírása

Az alábbi táblázatban látható elemek a **fájl** menüben.

| Menüpont | Leírás |
|:----------|:-------------|
| Új       | Kattintson az **Új** hozzon létre egy új konzolt a StorSimple pillanatkép kezelő alapú gombra. |
| Nyissa meg      | **Nyissa meg** a létező konzol megnyitása gombra. |
| Mentés      | Az aktuális konzol mentése a **Mentés** gombra. |
| Mentés másként   | Kattintson a **Mentés másként** hozzon létre egy új, az átnevezett példányt az aktuális konzol. A **Mentés másként** parancs segítségével nézetek testreszabása és mentése későbbi letöltésre. Ha például úgy lehetett létrehozni StorSimple pillanatkép Manager beépülő modul, mutasson az adott kiszolgálókhoz. |
| Hozzáadása és eltávolítása beépülő modul | Kattintson a **beépülő modul hozzáadása/eltávolítása** hozzáadása és eltávolítása a beépülő modul és rendszerezheti a **hatókör** ablaktáblán található csomópontok. További információért lépjen a [Hozzáadás, eltávolítás, és rendszerezése beépülő és az MMC 3.0 bővítmények](https://technet.microsoft.com/library/cc722035.aspx). |
| Beállítások   | Kattintson a **Beállítások** módosítása a konzol ikonra, adja meg a felhasználói hozzáférés módok és engedélyek és a szabad lemezterület növelése konzol fájlok törlése gombra. |
| A fájl elérési utak listájában | Kattintson a számozott listában meg kell nyitnia a legutóbb megnyitott fájl elérési. |
| Kilépés      | Kattintson a **Kilépés** a **fájl** menü bezárása. |
 
### <a name="action-menu"></a>A művelet menü

A **művelet** menü segítségével választhat a rendelkezésre álló műveletek. Az elérhető elemeket attól függenek, hogy ez a beállítás a **hatókör** vagy **eredmény** ablaktáblában.

#### <a name="menu-access"></a>Az access menü

A **művelet** menü megtekintéséhez tegye a következők valamelyikét:

- Kattintson a jobb gombbal egy elemet a **hatókör** vagy **eredmény** ablaktáblában.

- Jelöljön ki egy elemet a **hatókör** vagy **eredmények** ablaktáblán, és kattintson a **művelet** a menüsávon. 

Például ha, jelölje be a felső csomóponthoz **hatókör** mezőre, majd kattintson a jobb gombbal vagy a menüsávon kattintson a **művelet** , a következő menü jelenik meg.
 
![StorSimple pillanatkép Manager művelet menü](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

A **Műveletek** ablaktáblában (jobb oldalán lévő a konzol), a **művelet** menü műveletek azonos listáját tartalmazza. Ezenkívül a **Műveletek** ablaktábla tartalmazza a **Nézet** menü beállításokat, amelyek lehetővé teszik az **eredményeket** tartalmazó ablaktábla egyéni nézet létrehozása.

![A Nézet menü munkaablak megnyitása](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menü leírása

Az alábbi táblázat tartalmazza StorSimple pillanatkép Manager műveletek betűrendes listája. 

- A **művelet** oszlopban csomópontok és az eredmények elvégezhető műveletek sorolja fel. 

- A **navigációs** oszlop ismerteti, hogy választhatja ki a műveletet a megfelelő **művelet** menü megjelenítendő. Több **művelet** menük néhány műveletet jelennek meg. Az alábbi műveletek jelöljön ki egy **navigációs** beállítást a listajeles felsorolás. 

- A **Leírás** oszlopban ismerteti, hogyan lehet az egyes művelettel a **művelet** menü vagy műveletek ablaktáblában, és megtudhatja, mit jelent.

>[AZURE.NOTE] A **munkaablak** és a **művelet** menüket tartalmaz további beállításokat, például a **Nézet**, **Új ablak innen**, **frissítés**, **Lista exportálása**és **segítséget**. Ezek a beállítások érhetők el az MMC részeként, és nem adott StorSimple pillanatkép Manager. A táblázat az alábbi lehetőségek leírását tartalmazza.
 
| Művelet  | Navigációs  | Leírás  |
|:--------|:------------|:-------------|
| Hitelesítés | Kattintson az **eszközök** csomópontra, és kattintson a jobb gombbal egy eszközt, az **eredmény** ablaktáblában. | Kattintson a **hitelesítés** , az eszköz beállított jelszót. |
| Adatfeliratsor  | Bontsa ki a **Biztonságimásolat-katalógust**, bontsa ki a **Felhőben pillanatképek**, kattintson a biztonsági dátummal és az **eredmények** ablaktáblájában válassza ki a mennyiségi. | Kattintson a **Adatfeliratsor** készítsen másolatot a felhőben pillanatfelvétel és kijelölhet egy helyen tárolja azt. |
| Eszköz beállítása | Kattintson a jobb gombbal a **eszközök** csomópontot. | Kattintson az **eszköz beállítása** egy egy vagy több eszközt a Windows host csatlakozhat konfigurálása elemre. |
| Biztonsági másolat házirend létrehozása | Tegye a következők valamelyikét:<ul><li>Kattintson a jobb gombbal **biztonsági házirendeket**.</li><li>Kattintson vagy **Mennyiségi csoportok**kibontása, és kattintson a jobb gombbal egy mennyiségi csoportot.</li><li>Kattintson vagy bontsa ki a **Biztonsági másolat katalógus**, és kattintson a jobb gombbal egy mennyiségi csoportot.</li></ul> | Kattintson a **Biztonsági másolat házirend létrehozása** mennyiségi csoport ütemezett biztonsági konfigurálása. |
| Mennyiségi csoport létrehozása | Tegye a következők valamelyikét:<ul><li>Kattintson a **kötet** csomópontra, és kattintson a jobb gombbal a kötet, az **eredmény** ablaktáblában.</li><li>Kattintson a jobb gombbal a **Mennyiségi csoportok** csomópontot.</li></ul> | Kattintson a **Hangerő csoport létrehozása** kötet hozzárendelése egy mennyiségi csoporthoz. |
| Törlése | Kattintson a csomópontra vagy eredmény (Ez a cikk jelenik meg, hogy sok **művelet** menük és **Műveletek** ablaktáblák.) | Kattintson a **Törlés** a csomópont vagy a kijelölt eredmény törlése gombra. Amikor megjelenik a megerősítési párbeszédpanelről, erősítse meg, vagy a törlés megszakításához. |
| Részletek | Kattintson az **eszközök** csomópontra, és kattintson a jobb gombbal egy eszközt, az **eredmény** ablaktáblában. | Kattintson a **Részletek** , a konfigurációs eszköz részletei című részben találja. |
| Szerkesztése | Kattintson a **Biztonsági másolat házirendek**, és kattintson a jobb gombbal egy házirendet, az **eredmény** ablaktáblában. | Kattintson a **Szerkesztés** mennyiségi csoport biztonsági másolat ütemezésének módosítása gombra. |
| Lista exportálása | Kattintson bármelyik csomópontra vagy eredmény (Ez a cikk jelenik meg az összes **művelet** menük és **Műveletek** ablaktáblák.) | Kattintson a **Lista exportálása** lista vesszővel tagolt (CSV) fájl mentéséhez. Ezután elemzéshez táblázatkezelő programmal importálhatja a fájlt. |
| segítség | Kattintson bármelyik csomópontra vagy az eredményt. (Ez a cikk jelenik meg az összes **művelet** menük és **Műveletek** ablaktáblák.) | Kattintson a **Súgó** online súgó megnyitása külön böngészőablakban. |
| Új ablakban a további lehetőségek | Kattintson bármelyik csomópontra vagy eredmény (Ez a cikk jelenik meg az összes **művelet** menük és **Műveletek** ablaktáblák.) | Kattintson az **Új ablakban a további lehetőségek** megnyitásához StorSimple pillanatkép új ablakot.|
| Frissítés | Kattintson bármelyik csomópontra vagy eredmény (Ez a cikk jelenik meg az összes **művelet** menük és **Műveletek** ablaktáblák.) | Kattintson a **frissítés** frissítése az éppen megjelenített StorSimple pillanatkép Manager ablakot. |
| Eszköz frissítése | Kattintson az **eszközök** csomópontra, és kattintson a jobb gombbal egy eszközt, az **eredmény** ablaktáblában. | Kattintson az **Eszköz frissítése** szinkronizálása egy adott csatlakoztatott eszközt StorSimple pillanatkép Manager. |
| Eszközök frissítése | Kattintson a jobb gombbal a **eszközök** csomópontot. | Kattintson az **Eszközök frissítése** szinkronizálni a csatlakoztatott eszközök listája StorSimple pillanatkép Manager. |
| Kötet újraellenőrzése | Kattintson a jobb gombbal a **kötet** csomópontot. | Kattintson a **kötet újraellenőrzése** kattintva frissítse a listát az **eredmények** ablaktáblájában megjelenő mennyiségének. |
| Visszaállítása | Bontsa ki a **Biztonságimásolat-katalógust**, mennyiségi csoport kibontása, bontsa ki a **Helyi pillanatképek** vagy **Felhőalapú pillanatképek**, és kattintson a jobb gombbal a biztonsági másolatot. | Kattintson a **Visszaállítás** cserélje ki az aktuális mennyiségi csoport adatokat az adatokat a kijelölt biztonsági másolatból. |
| Biztonsági másolat készítése | Tegye a következők valamelyikét:<ul><li>**Mennyiségi csoportok**kibontása, és kattintson a jobb gombbal egy mennyiségi csoportot.</li><li>Bontsa ki a **Biztonsági másolat katalógus**, és kattintson a jobb gombbal egy mennyiségi csoportot.</li></ul> | Kattintson a **Biztonsági másolat készítése** a biztonsági mentési feladat azonnal elindításához. |
| Import megjelenítésének váltása | Kattintson a jobb gombbal a felső csomóponthoz a **hatókör** ablaktáblán (a példák **StorSimple pillanatkép Manager** csomópont). | **Import megjelenítésének váltása** megjelenítése vagy elrejtése mennyiség és az irányítópult StorSimple kezelő szolgáltatás importált társított biztonsági mentés gombra. |

### <a name="view-menu"></a>A Nézet menü

A **Nézet** menü segítségével a **eredménye** ablaktábla tartalmát egyéni nézet létrehozása. A **Nézet** menü elemeivel **Oszlopok hozzáadása/eltávolítása** és **testreszabása** .

#### <a name="menu-access"></a>Az access menü

A **Nézet** menüben a menüsávon, vagy kattintson a **Műveletek** ablaktáblában érheti el.

![StorSimple pillanatkép Manager nézet menüje](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menü leírása

Az alábbi táblázatban látható elemek a **Nézet** menüben.

| Menüpont  | Leírás |
|:-----------|:-------------|
| Oszlopok hozzáadása és eltávolítása | Kattintson az **Oszlopok hozzáadása/eltávolítása** oszlopok hozzáadása vagy törlése az **eredmény** ablaktáblában. |
| Testreszabása | Kattintson a **Testreszabás** megjelenítése és elrejtése a StorSimple pillanatkép Manager konzol ablakban az elemek lehetőségre. |

### <a name="favorites-menu"></a>A Kedvencek menü

A **Kedvencek** menü segítségével hozzáadása, eltávolítása és rendezése lap nézetek és a gyakran használt. 

#### <a name="menu-access"></a>Az access menü

A **Kedvencek** menü a menüsávon érheti el.

![StorSimple pillanatkép Manager Kedvencek menü](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menü leírása

Az alábbi táblázatban látható elemek a **Kedvencek** menüben.

| Menüpont |  Leírás |
|:----------|:-------------|
| Hozzáadás a Kedvencekhez | Kattintson a **Hozzáadás a Kedvencekhez** a jelenlegi nézet hozzáadása a Kedvencek lista. |
| Kedvencek rendezése | Jelölje ki a **Kedvencek rendezése** a Kedvencek mappában tartalmának rendezésére. |

### <a name="window-menu"></a>Az ablak menüben

Az **ablak** menü segítségével felvétele és átrendezése StorSimple pillanatkép Manager console-ablakot.

#### <a name="menu-access"></a>Az access menü

Kattintson a menüsor megjelenítése az **ablak** menü érheti el.

![StorSimple pillanatkép kezelő ablak](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

A menü alján a számozott lista azt mutatja, a windows aktuálisan megnyitott. Kattintson a minden olyan ablak, hogy a listában, az ablak vihet át az előtérben. 

#### <a name="menu-description"></a>Menü leírása

Az alábbi táblázat ismerteti a megjelenő elemek ablak menüben.

| Menüpont  | Leírás |
|:-----------|:-------------|
| Új ablak | Kattintson az **Új ablakot** nyisson meg egy új konzol ablakot (kívül a meglévő ablak). |
| Kaszkádolt   | Kattintson a **kaszkádolt** jeleníthet meg a megnyitott konzol windows kaszkádolt stílus gombra. |
| Egymás alatt | Kattintson a **Mozaik vízszintesen** jeleníthet meg a megnyitott konzol windows csempe (vagy a rács) formátumot. |
| Ikonok elrendezése | Ha több konzolt windows nyissa meg az asztalon, elszórtan összezárása őket, és válassza a **Ikonok elrendezése** rendezze őket a képernyő alján lévő vízszintes sor. |

### <a name="help-menu"></a>Súgó menü

A **Súgó** menü segítségével StorSimple pillanatkép felettes és a az MMC elérhető online súgó. Akkor is megtekintheti, hogy az MMC és StorSimple pillanatkép Manager szoftver verziói, amelyek a számítógépen telepített adatait. 

A **Súgó** menü a menüsávon érheti el. A **Műveletek** ablakból StorSimple pillanatkép témakörök is elérheti.

![StorSimple pillanatkép Manager Súgó menü](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menü leírása

Az alábbi táblázatban látható elemek a Súgó menüben.

| Menüpont  | Leírás  |
|:-----------|:-------------|
| Súgó a StorSimple pillanatkép Manager | Kattintson a **Súgó StorSimple pillanatkép Manager** StorSimple pillanatkép Manager – súgó megnyitása külön ablakban. |
| Súgó témakörei |Kattintson a **súgótémakörök** MMC online súgó megnyitása külön ablakban. |
| A webhely TechCenter | Kattintson a **TechCenter webhely** a Microsoft TechNet technikai Center kezdőlapja külön ablakban való megnyitásához. |
| Microsoft Management Console kapcsolatban | Kattintson a **Microsoft Management Console kapcsolatban** a Microsoft Management Console melyik verziója van telepítve, a rendszer. |
| Tudnivalók a StorSimple pillanatkép Manager | Kattintson a **StorSimple pillanatkép Manager eszközről** a beépülő modul melyik verziója van telepítve, a rendszer. |

## <a name="tool-bar"></a>Eszköztár

Az eszköztáron a menüsor megjelenítése alatt található navigációs és a tevékenység ikonok tartalmazza. Mindegyik ikon egy parancsikont, egy adott tevékenységhez.

### <a name="icon-descriptions"></a>Ismertetése

Az alábbi táblázat ismerteti a megjelenő ikonok, az eszköztárán. 

| Ikon  | Leírás  |
|:------|:-------------| 
| ![Balra mutató nyíl](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) | Kattintson a balra mutató nyíl ikonra kattintva térhet vissza az előző lapra. |
| ![Jobbra nyíl](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) | Kattintson a jobb nyílra kattintva nyissa meg a következő oldalra (Ha a nyílra a szürke, a művelet nem érhető el). |
| ![Fel ikon](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) | Kattintson a Mentés ikonra az (a **hatókör** ablaktábla) konzolfáján egy szinttel feljebb szeretne lépni. |
| ![Megjelenítés/elrejtés konzolfáján](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) | Kattintson a **hatókör** ablaktábla megjelenítése vagy elrejtése a Minden látszik konzol ábrázoló ikonra. |
| ![Lista exportálása](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) | Kattintson az Exportálás lista ikonra lista exportálása CSV-fájlba megadott feltételeknek. |
| ![Súgó ikon](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png)  |Kattintson a Súgó ikonra kattintva nyissa meg az online MMC súgótémakör. |
| ![Műveletek ablaktábla megjelenítése/elrejtése](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) | Kattintson a Minden látszik **Műveletek** ablaktábla ikon megjelenítéséhez vagy elrejtéséhez a **Műveletek** ablaktáblában. 
 
## <a name="scope-pane"></a>Hatókör ablaktáblán

A **hatókör** munkaablak jelenik meg a bal oldali ablaktábla StorSimple pillanatkép Manager felhasználói felület. A console (vagy csomópontra) fa tartalmaz, és a fő navigációs módszer az StorSimple pillanatképként parancsra. 
 
### <a name="scope-pane-structure"></a>Hatókör ablak szerkezete

A **hatókör** ablakban a fastruktúra rendezett kattintható objektumok (csomópontok) sorozata tartalmazza. 

![Hatókör ablaktáblán](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

- Kattintva kibonthatja vagy összecsukhatja a csomópontot, kattintson a csomópontra neve mellett lévő nyíl ikonra.

- Az állapot vagy csomópont tartalmának megtekintéséhez kattintson a csomópont nevét. Az információkat az **eredmények** ablaktáblában jelenik meg. 

A **hatókör** ablaktábla tartalmazza a következő csomópontok: 

- [Eszközök csomópont](#devices-node) 
- [Kötet csomópontot.](#volumes-node) 
- [Mennyiségi csoportok csomópont](#volume-groups-node) 
- [Biztonsági házirendek csomópont](#backup-policies-node) 
- [Katalógus a csomópont biztonsági mentése](#backup-catalog-node) 
- [Feladatok csomópontot.](#jobs-node) 

### <a name="scope-pane-tasks"></a>Hatókör ablak feladatok

A **hatókör** ablaktáblán egy adott csomóponton művelet elvégzéséhez is használhatja. Jelölje ki a tevékenységet, tegye a következők valamelyikét:

- Kattintson a jobb gombbal a csomópontot, és kattintson a megjelenő menüben jelölje ki a tevékenységet.

- Kattintson a csomópontra, és kattintson a **művelet** a menüsávon. Jelölje ki a megjelenő menüben.

- Kattintson a csomópontra, és kattintson a **Műveletek** ablaktáblában jelölje ki a műveletet.

Jelölje ki a csomópontot, és a tevékenység listájának megtekintéséhez használja az alábbi módszereket, csak az adott csomóponton elvégezhető műveleteket jelennek meg.

### <a name="devices-node"></a>Eszközök csomópont

Az **eszközök** csomópont StorSimple eszközök és StorSimple StorSimple pillanatkép Manager csatlakoztatott virtuális eszközökön jelöli. Jelölje ki a csatlakozáshoz és az eszköz beállítása csomópontot, és importálja a társított kötet, a kötet csoportok és a meglévő biztonsági másolatot. Különféle eszközökön egyetlen állomás kapcsolhat össze.

- Bontsa ki a csomópontot, kattintson az **eszközök**mellett lévő nyíl ikonra.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal az **eszközök** csomópont, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Ha szeretné látni beállított eszközök, **eszközök** ablaktáblájában kattintson a **hatókör** . Az eszközök együtt minden egyes az eszközre vonatkozó adatok listája megjelenik az **eredmény** ablaktáblában.

### <a name="volumes-node"></a>Kötet csomópontot.

A **kötet** csomópontot a meghajtók, amelyek megfelelnek a kötet, az állomás, beleértve a iSCSI, és egy eszközt feltárt feltárt csatlakoztatott jelöli. A csomópont segítségével a rendelkezésre álló kötet listájának megtekintéséhez és az egyes kötet mennyiségi csoportokhoz rendelése.

- **Kötet**mellett a nyílikonra kattintva bontsa ki a csomópontot.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal a **kötet** csomópontot, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Kötet listájának megtekintéséhez kattintson a **hatókör** ablaktáblán **kötet** . A lista mennyiségének együtt minden egyes a mennyiségi információt jelenik meg, az **eredmény** ablaktáblában.

### <a name="volume-groups-node"></a>Mennyiségi csoportok csomópont

Mennyiségi csoportok más néven konzisztencia csoportokat. Minden mennyiségi csoport, amely segít a biztonsági másolat műveletek során alkalmazás összhangot alkalmazás kapcsolatos kötet erőforráskészlethez tartozik. Segítségével a **Csoportok mennyiségi** csomópontot szeretne konfigurálni ezekhez a csoportokhoz és interaktív biztonsági másolatok készítése vagy biztonsági ütemtervek létrehozása. 

- **Mennyiségi csoportok**mellett a nyílikonra kattintva bontsa ki a csomópontot.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal a **Mennyiségi csoportok** csomópontot, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Mennyiségi csoportok listájának megtekintéséhez kattintson a **hatókör** ablaktáblán **Mennyiségi csoportok** . A mennyiségi csoportok együtt minden egyes mennyiségi csoportra vonatkozó információk listája megjelenik az **eredmény** ablaktáblában.

### <a name="backup-policies-node"></a>Biztonsági házirendek csomópont

Biztonsági házirendek olyan projekt ütemtervét helyi és felhőbeli pillanatképek. Használja a **Biztonsági házirendek** csomópontot adja meg, milyen gyakran hoz létre a biztonsági és mennyi biztonsági kell tartani. 

- **Biztonsági házirendek**mellett a nyílikonra kattintva bontsa ki a csomópontot.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal a **Biztonsági másolat házirendek** csomópontot, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Biztonsági házirendek listájának megtekintéséhez kattintson a **hatókör** ablaktáblán **Biztonsági házirendeket** . Egyes a házirendre vonatkozó információk együtt a biztonsági házirendek listájának megjelenik az **eredmény** ablaktáblában.

>[AZURE.NOTE] Legfeljebb 64 biztonsági másolatok segítségével megőrizheti.


### <a name="backup-catalog-node"></a>Katalógus a csomópont biztonsági mentése

A **Biztonsági másolat katalógus** csomópontot a helyszíni és külső biztonsági mentések Azure StorSimple mennyiségének listákat tartalmazza. A csomópont mennyiségi csoportok szerint rendezett, és minden egyes mennyiségi csoport tárolóban lévő külön szerkezetek helyi pillanatfelvételek ( **Helyi pillanatkép**s csomópont) és a felhő pillanatképek (a **Felhőben pillanatképek** csomópont). Kibontva, az egyes mennyiségi csoport tároló interaktív, illetve egy beállított házirend megtett sikeres másolatok sorolja fel.

- **Biztonsági másolat katalógus**mellett a nyílikonra kattintva bontsa ki a csomópontot.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal a **Biztonsági másolat katalógus** csomópontot, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Biztonsági pillanatképek listájának megtekintéséhez kattintson a **hatókör** ablaktáblán **Biztonságimásolat-katalógust** . Pillanatképek együtt minden egyes pillanatkép információt listája megjelenik az **eredmény** ablaktáblában.

### <a name="local-snapshots-node"></a>Helyi pillanatképek csomópontot.

A **Helyi pillanatképek** csomópont egy adott mennyiségi csoport helyi pillanatképek sorolja fel. A csomópont a **hatókör** ablaktáblán a **Biztonsági másolat katalógus** csomópont alatt található. Helyi pillanatképek az Azure StorSimple eszközön tárolt adatok mennyiségi pont és az idő másolatait is. A szokásos biztonsági mentése az ilyen típusú létrehozott is, és gyorsan vissza. Helyi pillanatkép helyi biztonsági másolatot ugyanúgy használhatja.

- **Helyi pillanatképek**mellett a nyílikonra kattintva bontsa ki a csomópontot.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal a **Helyi pillanatképek** csomópontot, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Helyi pillanatképek listájának megtekintéséhez kattintson a **hatókör** ablaktáblán **Helyi pillanatképek** . Pillanatképek együtt minden egyes pillanatkép információt listája megjelenik az **eredmény** ablaktáblában.

### <a name="cloud-snapshots-node"></a>Felhőalapú pillanatképek csomópont

A **Felhő pillanatképek** csomópont felhő pillanatképek egy adott mennyiségi csoport listája. A csomópont a **hatókör** ablaktáblán a **Biztonsági másolat katalógus** csomópont alatt található. Felhőalapú pillanatfelvételek a felhőben tárolt adatok mennyiségi pont és az idő példányainak. Felhőalapú pillanatkép megegyezik egy másik, a helyszínen tároló rendszeren replikált pillanatkép. Felhőalapú pillanatképek különösen hasznosak katasztrófa helyreállítási helyzetekben.

- Bontsa ki a csomópontot, kattintson a **Felhőben pillanatképek**mellett lévő nyíl ikonra.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal a **Felhőben pillanatképek** csomópontot, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Felhőalapú pillanatképek listájának megtekintéséhez **Felhő pillanatképek** ablaktáblájában kattintson a **hatókör** . Pillanatképek együtt minden egyes pillanatkép információt listája megjelenik az **eredmény** ablaktáblában.

### <a name="jobs-node"></a>Feladatok csomópontot.

A **feladatok** csomópontot a ütemezett, futó, és a legutóbb elkészült biztonsági feladatokra vonatkozó információkat tartalmazza. 

- **Feladatok**mellett a nyílikonra kattintva bontsa ki a csomópontot.

- Az elérhető műveletek menüjének megjelenítéséhez, kattintson a jobb gombbal a **feladatok** csomópontot, vagy kattintson a jobb gombbal bármelyik olyan csomópontot, amely a kibontott nézetben jelennek meg.

- Az ütemezett feladatok listájának megtekintéséhez bontsa ki a **feladatok** csomópontot, és kattintson az **ütemezett**. A korábban beállított feladatok és információt az egyes feladatok listáját megjelenik az **eredmény** ablaktáblában. 

- A legutóbb befejezett feladatok listájának megtekintéséhez bontsa ki a **feladatok** csomópontot, és kattintson az **utolsó 24 óra**. Az elmúlt 24 óra végrehajtott feladatok listájának megjelenik az **eredmény** ablaktáblában. Az **eredmények** ablaktáblája a minden kész feladattal kapcsolatos információkat is tartalmaz.

- Az éppen futó feladatok listájának megtekintéséhez bontsa ki a **feladatok** csomópontot, és kattintson a **futtatása**gombra. Futó, feladatok és információt az egyes feladatok listájának megjelenik az **eredmény** ablaktáblában.

## <a name="results-pane"></a>Eredmények ablaktáblája

Az **eredmények** ablaktáblája a középső ablaktáblában StorSimple pillanatkép Manager felhasználói felület. Listák és a **hatókör** ablakban kijelölt csomópont részletes állapotinformációkat tartalmaz.

### <a name="example"></a>Példa

Az alábbi példában látható, kattintson a **hatókör** ablakban a **Csoportok mennyiségi** csomópontot. Az **eredmények** ablaktáblában minden csoport részleteivel mennyiségi csoportok listáját jeleníti meg.

![Eredmények ablaktáblája](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Beállíthatja, hogy a részletek megjelennek az **eredmény** ablaktáblában: kattintson a jobb gombbal a **hatókör** ablaktáblán csomópont, kattintson a **Nézet**fülre, és válassza az **Oszlopok hozzáadása/eltávolítása**.

## <a name="actions-pane"></a>Műveletek ablaktáblában

A **Műveletek** munkaablak jelenik meg a jobb oldali StorSimple pillanatkép Manager felhasználói felület. Csomópontot, megtekintése vagy a **hatókör** vagy **eredmények** ablaktáblán adatokat elvégezhető műveleteket tartalmazó menü benne. A **Műveletek** ablaktáblában parancsok a **művelet** menük, a **hatókör** és **eredménye** ablaktábla elemek használható. Minden egyes művelet leírását lásd: a **művelet** menü szakaszában a táblázatban.

### <a name="examples"></a>Példák

Lásd: a következő példában a **hatókör** ablaktáblán bontsa ki a **feladatok** csomópontot, és kattintson az **ütemezett**. A **Műveletek** ablaktáblában az elérhető műveletek az **ütemezett** csomóponthoz.

![Példa a műveletek ablaktáblán ütemezett feladat](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

A **hatókör** ablakban a további lehetőségek megjelenítéséhez bontsa ki a **feladatok** csomópontot **ütemezett**kattintson, és kattintson az **eredmények** ablaktáblájában ütemezett feladat. A **Műveletek** ablaktáblában az elérhető műveletek az ütemezett feladat, az alábbi példában látható módon.

![Példa a műveletek ablaktáblán feladat műveletek](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Navigálás a billentyűzet segítségével és billentyűparancsok

StorSimple pillanatkép kezelő lehetővé teszi a Microsoft Management Console (MMC) és a Windows operációs rendszer kisegítő lehetőségeivel. Egyes billentyűzetfunkciók navigációs és arra, hogy bizonyos a StorSimple pillanatkép Manager, az alábbi szakaszokban ismertetett módon is tartalmaz.
 
- [Navigációs billentyűk](#keyboard-navigation-keys) 
- [Menüsor használható billentyűparancsok](#menu-bar-shortcut-keys) 
- [Hatókör ablakban használható billentyűparancsok](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Navigációs billentyűk

Az alábbi táblázat ismerteti a billentyűket, amelyek segítségével keresse meg a StorSimple pillanatkép kezelő felhasználói felülettel. 

| Navigációs billentyűt  | Művelet  |
|:----------------|:--------| 
| Le nyíl | A le nyílbillentyűvel segítségével függőlegesen áthelyezése a következő menü vagy ablakban elemre. |
| Adja meg | Nyomja le az Enter billentyű lenyomásával művelet végrehajtása, majd folytassa a következő lépéssel. Például lenyomja az Enter billentyűt, jelölje be a **következő**, **az OK gombra**, vagy **létrehozása**, és kattintson a következő lépés a varázsló.|
| ESC | Nyomja le az Esc billentyűt menü bezárása, illetve a Mégse gombra, majd zárja be egy lapot.|
| AZ F1 | Nyomja le az F1 billentyűt az aktív ablak súgótémakör megtekintéséhez.|
| AZ F5 | Nyomja le az F5 billentyűt a csomópont frissítéséhez. |
| F6 | Nyomja le az F6 billentyű lenyomásával viheti a **hatókör** ablakból az **eredmény** ablaktáblában.|
| F10 | Nyissa meg a menüsor megjelenítése az F10 billentyűt lenyomva. |
| Bal nyílbillentyű | A balra billentyűvel vízszintesen áthelyezése a menü közül az előző beállításra. Az előző elemre lépés a menüsávon, a művelet (vagy a helyi) menü az előző elem jelenik meg. |
| Jobb nyílbillentyű | A JOBBRA billentyűvel váltani vízszintesen egy sáv menüpont a Tovább gombra. A következő elemre lépés a menüsávon, a művelet (vagy a helyi) menü az új elem jelenik meg.
| TAB billentyű | A Tab billentyű segítségével helyezze át a konzolban, a következő ablaktáblára vagy az weblapon a következő kijelölés vagy a szöveg mezőbe. |
| Fel nyílbillentyű | A fel billentyűvel segítségével áthelyezése az előző elem függőlegesen menü vagy ablaktáblában. |

### <a name="menu-bar-shortcut-keys"></a>Menüsor használható billentyűparancsok

Az alábbi táblázat ismerteti a billentyűparancsokat a menüsor megjelenítése az. Miután, nyomja le a használható billentyűparancsok és a menü megnyitása, menü billentyűparancsai (a menü az aláhúzott billentyűk) is használhatja. Többet szeretne tudni a menüsor megjelenítése lépjen a [menüsávon](#menu-bar).

| Billentyűparancs | Eredmény                    | Billentyűparancs menü | Eredmény          |
|:---------|:--------------------------|:------------------|:----------------|
| ALT + F    | A **fájl** menü megnyitása.  | N | Megnyílik egy új konzol példányát.   |
|          |                           | O | A **Felügyeleti eszközök** lap megnyitása |
|          |                           | S | A StorSimple pillanatkép kezelője konzol menti.|
|          |                           | A | Ekkor megnyílik a **Mentés másként** lapon. |
|          |                           | M | Ekkor megnyílik a **beépülő modul hozzáadása/eltávolítása** lapon.|
|          |                           | P | Ekkor megnyílik a **Beállítások** lapot. |
|          |                           | H | Online súgó megnyitása|
| ALT + A    | A **művelet** menü megnyitása.| E | Be- és kikapcsolása, azzal kikapcsolja a importálása parancsra.|
|          |                           | W | Megnyit egy új StorSimple pillanatkép Manager konzolt.|
|          |                           | F | Frissíti a StorSimple pillanatkép Manager konzolt.|
|          |                           | L | Megnyitja a **Lista exportálása** lapot. 
|          |                           | H | Online súgó megnyitása|
| ALT + V    | A **Nézet** menü megnyitása.  | A | Ekkor megnyílik az **Oszlopok hozzáadása vagy eltávolítása** lapon. |
|          |                           | U | Ekkor megnyílik a **Nézet szerkesztése** lapon. |
| ALT + O    | A **Kedvencek** menü megnyitása. | A | Megnyitja a **Hozzáadás a Kedvencekhez** lapot. |
|          |                           | O | Megnyitja a **Kedvencek rendezése** lapot.|
| ALT + W    | Az **ablak** menü megnyitása.| N | Ekkor megnyílik egy másik StorSimple pillanatkép kezelő ablakban.|
|          |                           | C | Az összes megnyitott konzol windows kaszkádolt stílus jeleníti meg.|
|          |                           | KÉTMINTÁS T | Az összes megnyitott konzol windows rács mintát jeleníti meg. |
|          |                           | E | A képernyő alján a vízszintes sorban szereplő ikonok elrendezése.|
| ALT + H    | A **Súgó** menü megnyitása.  | H | Online súgó megnyitása|
|          |                           | KÉTMINTÁS T | Ekkor megnyílik a Microsoft TechNet technikai Center weblapot.|
|          |                           | A | Megnyitja a **Microsoft Management Console információ** lapot. |
 
### <a name="scope-pane-shortcut-keys"></a>Hatókör ablakban használható billentyűparancsok

Az alábbi táblázatokban minden csomópont billentyűparancsai megjelenítése a parancsikont a **hatókör** ablaktáblán. 

- [Eszközök csomópont használható billentyűparancsok](#devices-node-shortcut-keys)
- [Kötet csomópont használható billentyűparancsok](#volumes-node-shortcut-keys)
- [Mennyiségi csoportok csomópont használható billentyűparancsok](#volume-groups-node-shortcut-keys)
- [Biztonsági házirendek csomópont használható billentyűparancsok](#backup-policies-node-shortcut-keys)
- [Biztonsági másolat katalógus csomópont használható billentyűparancsok](#backup-catalog-node-shortcut-keys)
- [Feladatok csomópont használható billentyűparancsok](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Eszközök csomópont használható billentyűparancsok

| A helyi menü | Eredmény                               |
|:--------------|:-------------------------------------|
| C             | Ekkor megnyílik a **eszköz** lap. |
| D             | Eszközök és az eszközadatok listáját frissíti.|
| V             | A **Nézet** menü megnyitása. |
| W             | Megnyit egy új, csak a **Részletek** csomópontot a StorSimple pillanatkép Manager konzolt. |
| F             | Frissíti a StorSimple pillanatkép Manager konzolt. |
| L             | Megnyitja a **Lista exportálása** lapot. 
| H             | Online súgó megnyitása|
 

#### <a name="volumes-node-shortcut-keys"></a>Kötet csomópont használható billentyűparancsok

| A helyi menü   | Eredmény                              |
|:----------------|:------------------------------------|
| V               | A kötet listáját frissíti.        |
| V (kétszer) | A **Nézet** menü megnyitása.            |
| W               | Megnyit egy új, csak a **kötet** csomópontot a StorSimple pillanatkép Manager konzolt.|
| F               | Frissíti a StorSimple pillanatkép Manager konzolt.|
| L               | Megnyitja a **Lista exportálása** lapot. 
| H               | Online súgó megnyitása|
 
#### <a name="volume-groups-node-shortcut-keys"></a>Mennyiségi csoportok csomópont használható billentyűparancsok

| A helyi menü   | Eredmény                              |
|:----------------|:------------------------------------|
| G               | Ekkor megnyílik a **mennyiségi csoport létrehozása** lap. |
| V               | A **Nézet** menü megnyitása. |
| W               | Megnyit egy új, csak a **Mennyiségi csoportok** csomópontot a StorSimple pillanatkép Manager konzolt.|
| F               | Frissíti a StorSimple pillanatkép Manager konzolt. |
| L               | Megnyitja a **Lista exportálása** lapot. |
| H               | Online súgó megnyitása|

#### <a name="backup-policies-node-shortcut-keys"></a>Biztonsági házirendek csomópont használható billentyűparancsok

| A helyi menü   | Eredmény                              |
|:----------------|:------------------------------------|
| B               | Megnyitja a **házirend létrehozása** lapot. |
| V               | A **Nézet** menü megnyitása.            |
| W               | Megnyit egy új, csak a **Mennyiségi csoportok** csomópontot a StorSimple pillanatkép Manager konzolt.|
| F               | Frissíti a StorSimple pillanatkép Manager konzolt.|
| L               | Megnyitja a **Lista exportálása **lapot. 
| H               | Online súgó megnyitása|
 
#### <a name="backup-catalog-node-shortcut-keys"></a>Biztonsági másolat katalógus csomópont használható billentyűparancsok

| A helyi menü   | Eredmény                              |
|:----------------|:------------------------------------|
| W               | Megnyit egy új, csak a **Mennyiségi csoportok** csomópontot a StorSimple pillanatkép Manager konzolt. |
| F               | Frissíti a StorSimple pillanatkép Manager konzolt. |
| H               | Online súgó megnyitása|
 
#### <a name="jobs-node-shortcut-keys"></a>Feladatok csomópont használható billentyűparancsok

| A helyi menü   | Eredmény                              |
|:----------------|:------------------------------------|
| V               | A **Nézet** menü megnyitása.            |
| W               | Megnyit egy új, csak a **feladatokat** csomópontot a StorSimple pillanatkép Manager konzolt.|
| F               | Frissíti a StorSimple pillanatkép Manager konzolt.|
| L               | Megnyitja a **Lista exportálása** lapot.     |
| H               | Online súgó megnyitása                   |
 
## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [StorSimple pillanatkép Manager a StorSimple megoldás való](storsimple-snapshot-manager-admin.md)használatáról.
- Ismerje meg, [StorSimple pillanatkép Manager való kapcsolódáshoz és eszközkezelés](storsimple-snapshot-manager-manage-devices.md)használatáról.
