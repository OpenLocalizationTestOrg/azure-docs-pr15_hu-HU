<properties
    pageTitle="Első lépések a napló Analytics |} Microsoft Azure"
    description="Elérheti lépéseket a napló Analytics a Microsoft műveletek Management csomagja (MOBILE) perc alatt."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Log Analytics – első lépések

Elérheti lépéseket a napló Analytics a Microsoft műveletek Management csomagja (MOBILE) perc alatt. Hogyan hozhat létre a MOBILE-munkaterületre, amely hasonlít az ügyfélhez kiválasztásakor két lehetőség közül választhat:

- Microsoft műveletek Management Suite webhely
- Microsoft Azure-előfizetés

Létrehozhat egy ingyenes MOBILE munkaterület a MOBILE webhelyet használ. Másik lehetőségként a MOBILE-munkaterület létrehozása a Microsoft Azure előfizetés is használhatja. Mindkét munkaterületek funkcionális megfelelői, azzal a különbséggel, hogy egy ingyenes MOBILE munkaterület csak küldhet 500 MB-os adatok naponta a MOBILE szolgáltatás. Ha egy Azure-előfizetést használ, is is használhatja, hogy az előfizetés más Azure szolgáltatások eléréséhez. A módszer a munkaterület létrehozásához használt, függetlenül kell létrehoznia a munkaterület a Microsoft-fiókkal vagy szervezeti fiókkal.

Íme a folyamat figyelmébe:

![bevezetési diagram](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Jelentkezzen be a Analytics Előfeltételek és telepítése során megfontolandó kérdések

- Log Analytics teljesen használata a Microsoft Azure előfizetéses van szüksége. Ha nem rendelkezik az Azure előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/) , amellyel bármely Azure szolgáltatás eléréséhez. Vagy hozzon létre egy ingyenes MOBILE-fiókot a [Tevékenységek kezelése csomagja](http://microsoft.com/oms) webhelyén, és kattintson a **próbálja ki az ingyenes**.
- Egy MOBILE munkaterülethez
- Minden egyes Windows rendszerű számítógépről származó adatokat kell futtatni a Windows Server 2008 SP1 gyűjtése kívánt vagy újabb
- A MOBILE [tűzfal](log-analytics-proxy-firewall.md) hozzáférést webcímek szolgáltatás
- Egy [MOBILE napló Analytics továbbító](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (átjáró) kiszolgáló továbbítása a forgalom kiszolgálókról MOBILE, ha Internet-hozzáférés számítógépekről nem érhető el
- Ha Operations Manager, támogatja a napló Analytics műveletek Manager 2012 SP1 UR6 és feletti és műveletek Manager 2012 R2 UR2 és a fenti. A proxykiszolgáló támogatási műveletek Manager 2012 SP1 UR7 és műveletek Manager 2012 R2 UR3 hozzá lett adva. Határozza meg, hogyan azt fogja integrálni MOBILE.
- Határozza meg, ha a számítógépen van-e a közvetlen internetkapcsolata. Ha nem, szükségük a MOBILE service webhelyek eléréséhez átjáró-kiszolgálót. Az összes access HTTPS található.
- Határozza meg, hogy melyik technológiák és kiszolgálók adatokat küld MOBILE. Ha például tartomány vezérlők, SQL Server stb.
- Felhasználók számára a MOBILE és Azure engedélyt.
- Ha adathasználat aggódni, külön-külön a minden megoldást üzembe helyez, és tesztelje a teljesítményre gyakorolt hatás további megoldások hozzáadása előtt.
- Tekintse át az adatok használatát és a teljesítmény, a megoldások és szolgáltatások hozzáadása a napló Analytics. Ide tartoznak a esemény webhelycsoport, napló webhelycsoport, teljesítmény adatok gyűjtése és stb. Még jobban induljon minimális webhelycsoport adathasználat-ig vagy a teljesítményre gyakorolt hatás azonosították.
- Győződjön meg arról, hogy Windows ügynökök nem is kezelik Operations Manager használatával, egyébként az ismétlődő adatok eredményezi. Ez az Azure-alapú-ügynökök engedélyezett Azure diagnosztika rendelkező is érinti.
- Telepítése után ügynökök, ellenőrizze, hogy a agent megfelelően működik-e. Ha nem, annak biztosítására, hogy a titkosítási API ellenőrzése: következő generációs (CNG) billentyű elkülönítési nincs letiltva csoportházirend használatával.
- Néhány napló Analytics-megoldások van további követelmények



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>Regisztráció a 3 lépések az adatkezelési műveletek Suite használata

1. Nyissa meg a [Tevékenységek kezelése csomagja](http://microsoft.com/oms) webhelyet, és kattintson a **próbálja ki az ingyenes**. Jelentkezzen be a Microsoft-fiókkal, például Outlook.com-fiókkal vagy szervezeti fiókkal a vállalati vagy oktatási intézmények által biztosított használata az Office 365-ös vagy más Microsoft-szolgáltatásokkal.
2. Adja meg egy egyedi munkaterület nevét. Munkaterület a kezelési adatokat tároló logikai tároló. Lehetővé teszi, a szervezet más csapatok között partíciót adatok, a saját munkaterületre kizárólagos adatai. Adja meg az e-mail cím és a régió, ahol szeretné használni a tárolt adatok.  
    ![munkaterület létrehozása és előfizetés hivatkozás](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Ezután hozhat létre egy új Azure-előfizetés vagy egy hivatkozást egy meglévő Azure előfizetésbe. Ha azt szeretné, a folytatáshoz az ingyenes próbaverziót használ, kattintson a **Nem gombra**.  
  ![munkaterület létrehozása és előfizetés hivatkozás](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Készen áll az első lépések a tevékenységek kezelése csomagja portálon.

Többet is megtudhat a munkaterület beállítása és csatolása meglévő Azure fiók létrehozása az műveletek Management csomagot a [napló Analytics való hozzáférés kezelése](log-analytics-manage-access.md)a munkaterületek az.

## <a name="sign-up-quickly-using-microsoft-azure"></a>Iratkozzon fel gyorsan a Microsoft Azure használatával

1. Az [Azure portál](https://portal.azure.com) , és jelentkezzen be, tallózással keresse meg a szolgáltatások listában, és válassza a **Napló Analytics (MOBILE)**.  
    ![Azure portál](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Kattintson a **Hozzáadás**gombra, majd jelölje be az alábbi elemek lehetőségeit:
    - **MOBILE munkaterület** neve
    - **Előfizetés** – Ha több előfizetéssel, válasszon egyet az új munkaterület társítani szeretné.
    - **Erőforráscsoport**
    - **Hely**
    - **Réteg árak**  
        ![gyors létrehozása](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Kattintson a **Létrehozás** gombra, és láthatja, hogy a munkaterület adatait az Azure-portálon.       
    ![munkaterület részletei](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. A **MOBILE portál** hivatkozásra kattintva nyissa meg a tevékenységek kezelése csomagja webhely az új munkaterület.

Készen áll a tevékenységek kezelése csomagja portál használatának megkezdéséhez.

Többet is megtudhat a munkaterület beállítani, és a tevékenységek kezelése Suite Azure-előfizetésekhez [napló Analytics való hozzáférés kezelése](log-analytics-manage-access.md)a létrehozott meglévő munkaterületek csatolása.

## <a name="get-started-with-the-operations-management-suite-portal"></a>A tevékenységek kezelése csomagja portál – első lépések
Válassza a megoldások és csatlakozás a használni kívánt kiszolgálók, kattintson a **Beállítások** csempére, és kövesse az ebben a szakaszban szereplő lépéseket.  

![első lépések](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Megoldások hozzáadása** – a telepített megoldások megtekintése.  
    ![megoldások](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Kattintson a **Keresse fel a gyűjtemény** további megoldások hozzáadása gombra.  
    ![megoldások](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Jelölje ki a megoldást, és kattintson a **Hozzáadás**gombra.
2. **Csatlakozás egy adatforrás** - válassza ki, hogyan szeretné, hogy a kiszolgálói környezetben adatok gyűjtése csatlakoztatása:
    - Csatlakozás bármely Windows Server vagy egy ügyfél közvetlenül ügynökszoftvert telepítésével.
    - Kiszolgálók Linux rendszerhez kapcsolódnak a MOBILE Agent Linux.
    - A Windows vagy Linux Azure diagnosztikai virtuális kiterjesztésű konfigurált Azure tároló fiókot használ.
    - System Center Operations Manager használatával csatolja a kezelés csoport, például a teljes Operations Manager üzembe.
    - Engedélyezze a Windows Telemetriai frissítése Analytics használni.
        ![Kapcsolódó források](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Adatok gyűjtése** Állítsa be a saját munkaterületre adatok kitöltéséhez legalább egy adatforráshoz. Ha elkészült, kattintson a **Mentés**gombra.    

    ![adatok gyűjtése](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>Tetszés szerint csatlakoztatása kiszolgálók közvetlenül a tevékenységek kezelése csomagja ügynökszoftvert telepítésével

A következő példa bemutatja, hogyan telepítheti Windows-ügynök.

1. Kattintson a **Beállítások** csempére, kattintson a **Kapcsolódó források** fülre, kattintson egy fülre, a hozzáadni kívánt forrástípus és bármelyik letöltési ügynökkel, vagy ismerje meg, engedélyezésével ügynökszoftvert. Ha például kattintson **Töltse le a Windows ügynök (64 bites)**. Windows-ügynökök a Windows Server 2008 SP1 vagy újabb vagy a Windows 7 SP1 vagy újabb ügynök csak telepítheti.
2. Telepítse az ügynök egy vagy több kiszolgálókon. Egyesével-ügynökök telepíthető, vagy egy meglévő terjesztési megoldás, lehet, hogy egy [Egyéni parancsfájl](log-analytics-windows-agents.md), vagy több automatikus módszerrel használható.
3. Miután Ön fogadja el a licencszerződést, és úgy dönt, hogy a telepítési mappájában, kattintson a **Csatlakozás az ügynök Azure napló Analytics (MOBILE)**.   
    ![ügynökök beállítása](./media/log-analytics-get-started/oms-onboard-agent.png)

4. A következő lapon megadását kéri a munkaterület-azonosító és a munkaterület billentyűt. A munkaterület-azonosító és a billentyűk jelennek meg a képernyőn, ahol letöltött ügynök fájlt.  
    ![ügynök kulcsok](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![kiszolgálók csatolása](./media/log-analytics-get-started/oms-onboard-key.png)
5. A telepítés során rákattintva **Speciális** tetszés szerint a proxykiszolgáló beállítása, és adja meg a hitelesítési adatait. Kattintson a **Tovább** gombra a munkaterület-adatokat képernyőhöz való visszatéréshez.
6. Kattintson a **Tovább gombra** a munkaterület-azonosító és a billentyűk érvényesítéséhez. Ha a minden alkalmazás hibát talál, rákattintva **vissza** javítások elvégzése. A munkaterület-azonosító és a billentyűk érvényesített, kattintson a **telepítés** ügynök telepítésének befejezéséhez.
7. A Vezérlőpulton kattintson a Microsoft figyelése Agent > Azure napló Analytics (MOBILE) fülre. Egy zöld pipa ikonra a tevékenységek kezelése csomagja szolgáltatás kommunikálni az ügynökök fog megjelenni. Első lépésként ekkor körülbelül 5-10 perc.

>[AZURE.NOTE] A kapacitás kezelési és konfigurációs értékelési megoldások jelenleg nem támogatottak-kiszolgálók közvetlenül csatlakozik az műveletek Management csomagot.


A agent System Center műveletek Manager 2012 SP1 vagy újabb is csatlakozhat. Ehhez kattintson a **Csatlakozás az ügynök System Center Operations Manager**lehetőséget. Ha választja, hogy a beállítást választja, adatok küldése a szolgáltatás anélkül, hogy további hardver vagy töltse be a felügyeleti csoportok.

Erről további ügynökök kapcsolódás az műveletek Management csomagot [napló Analytics való csatlakozás Windows](log-analytics-windows-agents.md)mögött.

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Tetszés szerint csatlakozás kiszolgálók System Center Operations Manager használata

1. Jelölje ki a Operations Manager konzol **felügyeleti**.
2. Bontsa ki a **Működési háttérismeretek** csomópontot, és válassza a **Működési háttérismeretek kapcsolat**.

  >[AZURE.NOTE] Attól függően, hogy milyen frissítés összesítő a SCOM használja a csomópont *System Center tanácsadó*, *Működési háttérismeretek*vagy *Műveletek Management csomagja*jelenhet meg.

3. A **műveleti mélyebb regisztrálni** hivatkozásra felé jobb felső sarokban, és kövesse az utasításokat.
4. A regisztrációs varázsló befejezése után kattintson **a számítógép csoport hozzáadása** hivatkozásra.
5. A **Számítógép** keresőmezőbe is kereshet számítógépek és a csoportok Operations Manager ellenőrizni. Válassza a számítógépek és a csoportok, amelyeknek beépített őket a napló Analytics, kattintson a **Hozzáadás**gombra, és kattintson **az OK**gombra. Ellenőrizheti, hogy a MOBILE szolgáltatás adatokat fogad nyissa meg a **használati** csempét a tevékenységek kezelése csomagja portálon. Adatok körülbelül 5-10 perc jelenjen meg.

Erről további Operations Manager kapcsolódás az műveletek Management csomagot a [Operations Manager kapcsolatot a naplóban Analytics](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Tetszés szerint elemezheti az adatokat a felhőbeli szolgáltatások a Microsoft Azure-ban

Az műveletek csomagot, és gyorsan kereshet eseményt, és a felhőszolgáltatások és a virtuális gépeken futó IIS naplók diagnosztika Azure Cloud Services engedélyezése. További háttérismeretek az Azure virtuális gépeken futó a a Microsoft figyelése Agent telepítésével is tartalmaz. Erről további történő beállításáról a [napló Analytics csatlakozás Azure tároló](log-analytics-azure-storage.md)az műveletek Management csomagot használatának Azure környezetben.


## <a name="next-steps"></a>Következő lépések

- [Log Analytics hozzáadása megoldások a megoldástárból](log-analytics-add-solutions.md) funkciókat és a szükséges adatok összegyűjtése.
- Ismerkedés a [napló keresések](log-analytics-log-searches.md) megoldások által gyűjtött részletes információk megtekintése.
- [Irányítópultok](log-analytics-dashboards.md) segítségével mentheti, és saját egyéni kereséseket megjelenítéséhez.
