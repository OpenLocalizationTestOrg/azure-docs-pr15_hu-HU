<properties 
    pageTitle="Az adatkezelési átjáró az adatok gyári |} Microsoft Azure"
    description="Adatok áthelyezése fiókok között a helyszíni és felhőbeli adatok az átjáró beállítása. Azure Data Factory az adatkezelési átjáró segítségével helyezze át az adatokat." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="jingwang"/>

# <a name="data-management-gateway"></a>Az adatkezelési átjáró
Az adatkezelési átjáró egy ügyfél ügynök, amely másolása a helyszíni környezetben telepíteni kell az adatokat a felhőben, és a helyszíni adatok tárolók között. Az adatok gyári által támogatott a helyszíni adatok tárolja a [támogatott adatforrások](data-factory-data-movement-activities.md##supported-data-stores) szakaszban felsorolt. 

> [AZURE.NOTE] Átjáró jelenleg csak a tevékenység másolás és a tárolt eljárás tevékenység az adatok gyári. Még nem lehetséges az egyéni tevékenység az átjáró a helyszíni adatforrás eléréséhez használni. 

Ez a cikk a forgatókönyv a kiegészíti a [adatok áthelyezése fiókok között a helyszíni és felhőbeli adatokat tárolja](data-factory-move-data-between-onprem-and-cloud.md) cikk. Az útmutató hozzon létre egy folyamat, amely az átjáró a helyszíni SQL Server-adatbázisból származó adatok áthelyezése az Azure blob használ. Ez a cikk az adatkezelési átjáró részletes információt tartalmaz.   

## <a name="overview"></a>– Áttekintés

### <a name="capabilities-of-data-management-gateway"></a>Adatkezelési átjáró funkciók
Az adatkezelési átjáró a következő lehetőségeket nyújtja:

- Modell helyszíni adatforrások és felhőbeli adatforrások adatok gyári és áthelyezés-adatok között.
- Egyetlen munkaablak, üveg figyelemmel kísérésére és a kezelés betekintést kap abba, hogy az adatok gyári lap az átjáró állapotát a van.
- A helyszíni adatforrások biztonságosan kezelése.
    - Nincs módosítás szükséges a vállalati tűzfal. Átjáró csak a kimenő HTTP-alapú kapcsolatok nyissa meg az internet segítségével.
    - Titkosítsa a hitelesítő adatait a helyszíni adatok tárolja a tanúsítvány.
- Adatok hatékony mozgatása – adatátvitel ezzel párhuzamosan automatikus rugalmassá a hálózatkimaradási problémákkal logika próbálkozzon újra.

### <a name="command-flow-and-data-flow"></a>Munkafolyamat parancs és a adatfolyam
Használatakor egy példány tevékenységet a helyszíni és felhőbeli között adatok másolása, a tevékenység használja az átjárók adatok átvitele a helyszíni adatforrás felhőből és fordítva.

Itt magas szintű adatok folyamat és az átjáró adatainak másolása lépései összefoglalása: ![adatfolyam átjáró segítségével](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1.  Adatok Fejlesztőeszközök az átjárók létrehoz egy Azure Data Factory az [Azure portálon](https://portal.azure.com) vagy a [PowerShell-parancsmag](https://msdn.microsoft.com/library/dn820234.aspx)használatával. 
2.  Adatok Fejlesztőeszközök szolgáltatást hoz létre csatolt adatokat tároló helyszíni az átjáró megadásával. A csatolt szolgáltatás beállításának részeként adatok fejlesztő beállítás hitelesítő adatok alkalmazása hitelesítéstípusok és a hitelesítő adatok megadásához.  A beállítás hitelesítő adatok alkalmazás párbeszédpanelen a adatokat tároló tesztelje a kapcsolatot, és az átjáró hitelesítő adatok mentését kommunikál.
3. Átjáró a tanúsítvány társított átjáró (adatok fejlesztő által biztosított), a hitelesítő adatokat a hitelesítő adatok mentése a felhőbe előtt titkosítja.
4. Adatok gyári szolgáltatás kommunikál az átjáró ütemezési & felügyeleti feladatok egy megosztott Azure szolgáltatás bus várólista használó vezérlő csatorna keresztül. Amikor egy tevékenység projekt másolása kell kell megrúgni, a Data Factory várakozási sorba hitelesítő adatokkal együtt a kérést. Átjáró kikapcsolása a feldolgozás után a várólista lekérdezési lép működésbe.
5.  Az átjáró használatával visszafejti a hitelesítő adatok ugyanazzal a tanúsítvánnyal és a helyszíni adatok áruház megfelelő hitelesítés típusa és a hitelesítő adatait, majd csatlakozik.
6.  Az átjáró adatokat másolja a helyszíni áruházból egy felhőbeli tárhelyről, és fordítva a adatok során a Másolás tevékenység beállításaitól függően. Az ebben a lépésben az átjáró közvetlenül kommunikál felhőalapú tárhelyek szolgáltatásokból, például a Azure Blob-tárolóhoz biztonságos (HTTPS) csatornán keresztül.

### <a name="considerations-for-using-gateway"></a>Az átjáró használatával kapcsolatos szempontok
- Több helyszíni adatforrások az adatkezelési átjáró egyetlen példány használhatók. Jó helyen jár **csak egy Azure adatok gyári területhez tartozik egy egyetlen átjárópéldány** és nem osztható meg a másik adatok gyári.
- Beállíthatja, hogy **csak egy példánya az adatkezelési átjáró** egy egyetlen számítógépre telepítve. Tegyük fel, hogy, a helyszíni adatforrások eléréséhez szükséges két adatok figyelőgyárak van, telepítenie kell ezeket az átjárók két helyszíni számítógépen. Más szóval az átjáró meghatározott adatokat gyár területhez tartozik
- Az **átjáró nem kell az adatforrás ugyanazon a gépen**. Azonban az átjáró az adatforrás közelebbi problémákat csökkenti az idő az átjáró csatlakozni az adatforráshoz. Azt javasoljuk, hogy az átjáró telepítette egy számítógépre, amely egy helyszíni adatforrás üzemeltető különbözik. Ha az átjáró és az adatforrás különböző számítógépeken, az átjáró nem versenyt erőforrások az adatforrással.
- Beállíthatja, hogy a **különböző számítógépeken csatlakoztatása az azonos több átjáró a helyszíni adatforrást**. Például előfordulhat, hogy két adatok gyárak szolgáló két átjáró, de az ugyanazt a helyszíni adatforrás regisztrált mindkét az adatok gyárak.
- Már telepítve van a számítógépén a **Power BI** példa felszolgálásához az átjárók, telepítse az **Azure Data Factory külön átjárót** egy másik számítógépen.
- Még ha **készült ExpressRoute**átjáró kell használni.
- Az adatforrás tekinti egy helyszíni adatforrás (tűzfal mögött van) is **készült ExpressRoute**használatakor. Az átjáró használja a szolgáltatás és az adatforrás közötti kapcsolatot létesíteni.
- Meg kell **használni az átjáró** akkor is, ha az adatokat tároló az **Azure IaaS virtuális**meg a felhőben. 

## <a name="installation"></a>Telepítés

### <a name="prerequisites"></a>Előfeltételek
- A támogatott **operációs rendszer** csak Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012-ben, a Windows Server 2012 R2. Az adatkezelési átjáró a tartományvezérlőnek a telepítés jelenleg nem támogatott.
- .NET-keretrendszer 4.5.1 vagy újabb szükséges. Ha telepíti az átjáró a Windows 7-ben a számítógépen, telepítse a .NET-keretrendszer 4.5 vagy újabb verzió. Lásd: a [.NET-keretrendszer rendszerkövetelményeiről](https://msdn.microsoft.com/library/8z6watww.aspx) további információt. 
- Ajánlott **konfiguráció** az átjárót futtató számítógépen legalább 2 GHz-es, 4 magmintákat, 8 GB RAM és 80-GB szabad.
- Ha az állomásgép hibernálás az átjáró nem adatok kérelmekre válaszolni. Ezért állítsa be a megfelelő **Energiaséma** azon a számítógépen az átjáró telepítése előtt. A gép hibernálás van beállítva, az átjáró telepítési üzenet kéri.
- Telepítse és állítsa be az adatkezelési átjáró a sikeres kattintva a gépen rendszergazdának kell lennie. A többi felhasználó vehet az **Adatok adatkezelési átjáró felhasználók** helyi Windows csoporthoz. Az adatkezelési átjáró konfigurációkezelőjének eszköz használhatja az átjáró beállítása a csoport tagjai. 

Másolás tevékenység fut a meghatározott gyakorisággal történik, mint az Erőforrás kihasználtsága (Processzor, memória) a számítógépen is követi a csúcs és tétlen időpontokkal azonos minta. Erőforrás-kihasználtság is függ erősen áthelyezése adatok mennyiségét. Ha több példányt feladat előrehaladását, erőforrás-kihasználtság csúcs időszakokban részletezésből látni. 

### <a name="installation-options"></a>Telepítési beállítások
Az adatkezelési átjáró telepíthető az alábbi módon: 

- A letöltés MSI telepítőcsomag a [Microsoft letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=39717).  Az MSI a meglévő adatok az adatkezelési átjáró a legújabb verzióra frissíteni megőrzi az összes beállításokkal is használható.
- **Töltse le és telepítse az adatok átjáró** hivatkozásra kattintva a kézi beállítás, vagy **közvetlenül a számítógépre telepíthető** EXPRESS beállítás alatt. [Az adatok helyszíni és felhőbeli közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál használatával kapcsolatos részletes útmutatást gyors telepítése. A kézi lépés megnyitja a letöltőközpontból.  Az utasításokat a letöltése és telepítése az átjáró letöltőközpontból szerepelnek, az alábbi szakasszal. 

### <a name="installation-best-practices"></a>Telepítési ajánlott eljárások:
1.  Konfigurálja az állomásgép az átjáró energiaséma, hogy a gép nem hibernálás. Ha az állomásgép hibernálás az átjáró nem adatok kérelmekre válaszolni.
2.  Készítsen biztonsági másolatot az átjáró társított tanúsítványt.

### <a name="install-gateway-from-download-center"></a>Telepítse az átjáró letöltőközpontjából
1. Nyissa meg [a Microsoft adatkezelési átjáró letöltési oldalon](https://www.microsoft.com/download/details.aspx?id=39717). 
2. Kattintson a **Letöltés**gombra, jelölje ki a megfelelő verziót (**32 bites** és **64 bites**), és kattintson a **Tovább**gombra. 
3. Az **MSI** közvetlenül vagy mentse a merevlemezen és végre.
4. A **Kezdőlap** lapon válasszon ki egy **nyelvet** a **Tovább**gombra.
5. A végfelhasználói licencszerződésben találja **Elfogadás** , és kattintson a **Tovább**gombra. 
6. Jelölje ki a **mappát** , az átjáró telepítése, és kattintson a **Tovább**gombra. 
7. **Készen áll a telepítésre** lapon kattintson a **telepítés**gombra. 
8. Kattintson a telepítés befejezéséhez a **Befejezés gombra** .
9. A kulcs lekérése az Azure-portálra. Részletes útmutatást a következő szakaszban olvashat. 
10. **Az adatkezelési átjáró konfigurációkezelőjének** a számítógépen futó a **regisztrálni átjáró** lapon végezze el az alábbi lépéseket: 
    1. A kulcs illeszteni a szöveget.
    2. Tetszés szerint kattintson a **átjáró kulcsa megjelenítése** a fő szöveget.
    3. Kattintson a **Regisztrálás**gombra. 

### <a name="register-gateway-using-key"></a>Átjáró kulccsal regisztrálja

#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Ha még nem hozott létre logikai az átjáró a portálon
Átjáró létrehozása a portálon, és a kulcs beolvasása a **Configure** lap, kövesse az útmutató – [a helyszíni és felhőbeli között adatok áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) című témakörben.    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Ha már létrehozott logikai átjáró a portálon
1. Az Azure-portálon nyissa meg az **Adatok gyári** lap, és kattintson **a csatolt szolgáltatások** csempére.

    ![Adatok gyári lap](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Jelölje ki a portálon létrehozott logikai **átjáró** a **Csatolt szolgáltatások** lap. 

    ![logikai átjáró](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
2. Kattintson az **Adatok átjárók** lap **Töltse le és telepítse az adatok átjáró**.

    ![Letöltési hivatkozás a portálon](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
3. Kattintson a **beállítás** lap **hozza létre újból a billentyűt**. Kattintson az Igen gombra a figyelmeztető üzenet jelenik meg gondosan elolvasása után.

    ![Hozza létre újból a billentyűt](media/data-factory-data-management-gateway/recreate-key-button.png)
4. Kattintson a kulcs melletti Másolás gombra. A kulcs másolja a vágólapra.
    
    ![Kulcs másolása](media/data-factory-data-management-gateway/copy-gateway-key.png) 

### <a name="system-tray-icons-notifications"></a>Tálca ikonjai és értesítések
Az alábbi képen látható az egyes a tálca ikonja látható. 

![tálca ikonjai](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

A rendszer tálca ikonja/értesítő üzenet fölé viszi a kurzort, megjelenik az előugró ablakban az átjáró/frissítési művelet állapotának olvashat.

### <a name="ports-and-firewall"></a>Portokat és a tűzfalat
Nincsenek két tűzfal kell vennie: a szervezet és a helyi számítógépen démon konfigurálva **a Windows tűzfal** központi útválasztó fut, amelyen telepítve van-e az átjáró **vállalati tűzfal** .  

![tűzfalak](./media/data-factory-data-management-gateway/firewalls.png)

Vállalati tűzfal szinten van szükség a következő tartományokat és a kimenő portok konfigurálása:

| A tartománynevek | Portok | Leírás |
| ------ | --------- | ------------ |
| *. servicebus.windows.net | 443, 80 | A szolgáltatás Bus továbbítási (megköveteli a 443-as hozzáférési jogkivonat WIA) TCP felett hallgatók | 
| *. servicebus.windows.net | 9350-9354, 5671 | Nem kötelező szolgáltatás bus továbbítási TCP felett | 
| *. core.windows.net | 443-as | HTTPS | 
| *. clouddatahub.net | 443-as | HTTPS | 
| Graph.Windows.NET | 443-as | HTTPS |
| login.Windows.NET | 443-as | HTTPS | 

A windows tűzfal szintre, de ezeket a kimenő portokat általában engedélyezve vannak. Ha nem, beállíthatja a tartományok és a portokra megfelelően átjárót futtató számítógépen.

#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Adatok másolása egy adatforrás adattár egy gyűjtő adattárhoz

Győződjön meg arról, hogy a tűzfalszabályokat megfelelően engedélyezve vannak a vállalati tűzfal, a Windows tűzfal az átjárót futtató számítógépen, és az adatok tárolására magát. Ezeket a szabályokat engedélyezi mind a forrás csatlakozzon, és sikeresen gyűjtése az átjárót. Az egyes adattár, amely a Másolás részt szabályok engedélyezése.

Ha például **egy helyszíni adatok áruházból egy SQL Azure-adatbázis gyűjtő vagy egy Azure SQL-adatraktár gyűjtése**másolja, tegye az alábbiakat: 

- Kimenő **TCP** kommunikáció engedélyezése a port **1433** a Windows tűzfal és a vállalati tűzfal
- A tűzfal beállításainak az Azure SQL server az átjárót futtató számítógépen IP-címének hozzáadása az engedélyezett IP-címek listája. 


### <a name="proxy-server-considerations"></a>A proxykiszolgáló kiszolgáló kapcsolatos szempontok
Ha a vállalati hálózathoz környezet csatlakozik az internetre proxykiszolgálót használ, állítsa be az adatkezelési átjáró használata, megfelelő proxybeállításokat. Beállíthatja, hogy a proxy az eredeti regisztrációs fázisban. 

![A proxykiszolgáló beállítása regisztráció során](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Átjáró a proxykiszolgálót használ, a felhőbeli szolgáltatástól csatlakozni. Kattintson a hivatkozások **módosítása** kezdeti telepítésekor. Megjelenik a **proxy beállításai** párbeszédpanelt.

![Beállítása a proxykiszolgáló használata Konfigurációkezelő](media/data-factory-data-management-gateway/SetProxySettings.png)

Háromféleképpen konfigurációs: 

- **Ne használja a proxy**: átjáró nem explicit módon használja bármely proxy felhőszolgáltatások csatlakozni.
- **Használat rendszere proxy**: átjáró használja a proxy beállításai, amely a diahost.exe.config van beállítva.  Ha nincs proxy diahost.exe.config van beállítva, átjáró csatlakozik felhőszolgáltatásba közvetlenül nélkül proxyn keresztül.
- **Egyéni proxykiszolgáló használata**: állítsa be a HTTP-proxy állítja, hogy az átjáró diahost.exe.config konfiguráció használata helyett.  Cím és Port szükség.  Felhasználónév és jelszó megadása nem kötelező, attól függően, hogy a proxy hitelesítési beállítást.  Minden beállítás a hitelesítő adatok tanúsítvány az átjáró titkosított és az átjáró állomásgép tárol.

Az adatok adatkezelési átjáró Host szolgáltatás automatikusan újraindul a frissített proxybeállítások mentése után. 

Miután átjáró sikeresen regisztrálva van, ha azt szeretné, megtekintheti és frissítheti a proxybeállítások, használja az adatkezelési átjáró konfigurációkezelőjének. 

1. Indítsa el az adatkezelési átjáró konfigurációkezelőjének.
2. Kattintson a **Beállítások** fülre.
3. Kattintson a **HTTP-Proxy beállítása** párbeszédpanel elindítására **HTTP-Proxy** szakasz **módosítása** hivatkozásra.  
4. Után a **következő** gombra kattint, megjelenik egy figyelmeztető párbeszédpanel a proxy beállításai mentse, és indítsa újra az átjáró Host szolgáltatást a engedélyt kér.

Megtekintheti és frissítheti a HTTP-proxy Configuration Manager eszközzel. 

![Beállítása a proxykiszolgáló használata Konfigurációkezelő](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [AZURE.NOTE] NTLM-hitelesítés a proxykiszolgáló beállítása, ha átjáró Host szolgáltatás fut, a tartományi fiók csoportjában. Ha később a tartományi fiók jelszavának megváltoztatása, ne feledje, frissítse a szolgáltatás beállításait, és indítsa újra megfelelően. Ez a követelmény miatt ajánlott a dedikált tartományi fiók a proxykiszolgáló nem igénylő, gyakran frissíteni a jelszó eléréséhez használt.

### <a name="configure-proxy-server-settings-in-diahostexeconfig"></a>Diahost.exe.config a proxykiszolgáló beállításainak konfigurálása
Ha a **rendszer a proxykiszolgáló használata** a HTTP-proxy beállításai lehetőséget választja, az átjáró a proxy beállítása a diahost.exe.config használja.  Ha nincs proxy diahost.exe.config van megadva, átjáró kapcsolódik felhőszolgáltatásba közvetlenül nélkül proxyn keresztül. Az alábbi eljárás a konfigurációs fájl frissítése ismerteti. 

1.  A Fájlkezelőben másolat készítése a megbízható C:\Program Files\Microsoft adatok kezelése Gateway\2.0\Shared\diahost.exe.config biztonsági másolatot készíteni az eredeti fájlt.
2.  Indítsa el a Notepad.exe futtatása rendszergazdaként, és nyissa meg a szövegfájlt "C:\Program Files\Microsoft adatok kezelése Gateway\2.0\Shared\diahost.exe.config. Az alapértelmezett címke system.net a megkeresése, ahogy a következő kódot:

            <system.net>
                <defaultProxy useDefaultCredentials="true" />
            </system.net>   

    Majd felveheti a proxy server részleteit, az alábbi példában látható módon:

            <system.net>
                  <defaultProxy enabled="true">
                        <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
                  </defaultProxy>
            </system.net>

    További tulajdonságokat a proxy címkére adja meg a szükséges beállításokat, például scriptLocation engedélyezett. Olvassa el az szintaxis- [proxy (hálózati beállítások) elemet](https://msdn.microsoft.com/library/sa91de1e.aspx) .

            <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>

3. Az eredeti helyre a konfigurációs fájl mentését, majd indítsa újra az adatok adatkezelési átjáró Host szolgáltatása, amelynek a felveszi a módosításokat. A szolgáltatás újraindítása: használhatja a szolgáltatásokat kisalkalmazást a Vezérlőpultról, vagy az **Adatkezelési átjáró konfigurációkezelőjének** > kattintson a **Szolgáltatás leállítása** gombra, majd kattintson a **Szolgáltatás indítása**. A szolgáltatás nem indul el, ha valószínű, hogy hibás XML címke szintaxis módosításának alkalmazás konfigurációs fájlba hozzá lett adva.     

Ezek a pontok mellett is szükséges, a vállalat whitelist van Microsoft Azure. Érvényes Microsoft Azure IP-címek listája letölthető a [Microsoft letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Lehetséges jelenségek a tűzfalat, és a proxy-kiszolgálóval kapcsolatos problémák
Ha a következő lehetőségekből hasonló hibákat, akkor valószínű, a tűzfal vagy proxykiszolgáló kiszolgáló, amely a csatlakozást Data Factory átjáró akadályozza és hitelesítse magát helytelen konfigurációja miatt. Előző szakaszban olvashat annak biztosítására, a tűzfal és a proxykiszolgáló megfelelően van-e beállítva.

1.  Amikor megpróbálja regisztrálni az átjárót, a következő hibaüzenetet kapja: "nem sikerült regisztrálni az átjárót billentyűt. Mielőtt megpróbálja újra az átjáró kulcsa rögzítéséhez, győződjön meg arról, hogy az adatkezelési átjáró csatlakoztatott állapotban van, és az adatok adatkezelési átjáró Host szolgáltatás elindul,."
2.  Konfigurációkezelő megnyitásakor állapot megjelenik "A kapcsolat" vagy "Csatlakozás." Windows-eseménynaplók, a "Eseménynapló" megtekintésekor > "Alkalmazás és a szolgáltatásnaplók" > hibaüzenetek, például a következő hiba "Az adatkezelési átjáró", lásd:`Unable to connect to the remote server` 
    `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Nyissa meg a titkosítsa a hitelesítő adatokat 8050 port 
A **Beállítás hitelesítő adatok** alkalmazást használ, a bejövő port **8050** listától hitelesítő az átjáró beállítása egy helyszíni csatolja az Azure-portálon szolgáltatás. Átjáró a telepítés során alapértelmezés szerint az adatkezelési átjáró telepítése megnyitja az átjárót futtató számítógépen.
 
Ha a külső tűzfalat használ, megnyithatja a port 8050 manuálisan. Ha tűzfalat probléma lép fel átjáró a telepítés során, próbálja meg a következő parancs használatával telepítse az átjáró beállítása a tűzfal nélkül.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Ha úgy dönt, hogy ne nyissa meg a port 8050 az átjárót futtató számítógépen, használja a mechanizmusok eltérő áruházból hitelesítő adatainak beállítása a **Beállítás hitelesítő adatok** alkalmazás használatával. Ha például használhatja [Új-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-parancsmag is. Lásd: hogyan adatok hitelesítő adatainak tárolása [beállítás hitelesítő adatok és a biztonság](#set-credentials-and-securityy) szakaszában állíthat be.

## <a name="update"></a>Frissítés 
Alapértelmezés szerint az adatkezelési átjáró automatikusan frissül, amikor az átjáró újabb verzióját érhető el. Az átjáró nem frissül, amíg az ütemezett tevékenységek végzett. Nincsenek további tevékenységek dolgozza fel az átjárót a frissítési művelet befejeztéig. Ha a frissítés sikertelen, átjáró visszaáll a korábbi verziója. 

Az ütemezett frissítés alkalommal jelenik meg a következő helyeken:

- Az átjáró Tulajdonságok lap az Azure-portálon.
- A az adatkezelési átjáró konfigurációkezelőjének kezdőlapja
- Rendszer-tálca értesítési üzenet. 

A Kezdőlap lap, az adatkezelési átjáró konfigurációkezelőjének jeleníti meg a frissítési ütemezés és a legutóbbi az átjáró telepített vagy frissített. 

![Frissítések ütemezése](media/data-factory-data-management-gateway/UpdateSection.png)

Azonnal telepítse a frissítést, vagy megvárja, amíg az ütemezett időpontban automatikusan frissíti az átjárót. Ha például az alábbi képen látható, az értesítő üzenet látható az átjáró Konfigurációkezelőjében együtt a frissítés gomb, amelyre kattintva azonnal telepíteni. 

![Frissítés DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

A tálcán az értesítő üzenet jelenne meg, az alábbi képen látható módon: 

![Rendszer rendszertálcán megjelenő üzenet](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Frissítési művelet (kézi és automatikus) a tálcán állapotának megtekintése. Amikor legközelebb indít átjáró konfigurációkezelőjének, megjelenik egy üzenet az értesítési sávon, hogy az átjáró frissült a [Mi az, hogy új témakör](data-factory-gateway-release-notes.md)mutató hivatkozás együtt.

### <a name="to-disableenable-auto-update-feature"></a>Az automatikus frissítés szolgáltatás engedélyezése és tiltása
Akkor is tiltása és engedélyezése az automatikus frissítés funkció az alábbi lépésekkel: 

1. Indítsa el a Windows PowerShell az átjárót futtató számítógépen. 
2. Váltson a C:\Program Files\Microsoft adatok kezelése Gateway\2.0\PowerShellScript mappát.
3. A Futtatás a következő parancsot, ha szeretné kapcsolni az automatikus frissítés funkció kikapcsolása (Letiltás).   

        .\GatewayAutoUpdateToggle.ps1  -off

4. Visszakapcsolásához azt: 
    
        .\GatewayAutoUpdateToggle.ps1  -on  

## <a name="configuration-manager"></a>Konfigurációkezelő 
Miután telepítette az átjárót, az adatkezelési átjáró konfigurációkezelőjének indíthatja el az alábbi módon: 

- A **Keresés** ablakban írja be az **Adatkezelési átjáró** a segédprogram eléréséhez. 
- Futtassa a végrehajtható **ConfigManager.exe** a mappába: **C:\Program Files\Microsoft adatok kezelése Gateway\2.0\Shared** 
 
### <a name="home-page"></a>A Kezdőlap lapon
A Kezdőlap lap lehetővé teszi, hogy végezze el az alábbi műveletek: 

- (Csatlakozik a felhőbeli szolgáltatástól stb.) az átjáró állapotának megtekintése 
- **Regisztráció** a portálról egy billentyűje segítségével.
- **Állítsa le** és indítása az **adatok adatkezelési átjáró Host szolgáltatás** az átjárót futtató számítógépen.
- **Frissítések ütemezése** a nap egy adott időpontban.
- A dátum, amikor az átjáró lett **utoljára frissítve**megtekintése. 

### <a name="settings-page"></a>Beállítások lap
A beállítások lap lehetővé teszi, hogy végezze el az alábbi műveleteket:

- Megtekintése, módosítása és exportálása az átjáró által használt **tanúsítvány** . A tanúsítvány adatforrások hitelesítő adatainak titkosítására használják.
- **HTTPS-port** módosítása a végpontot. Az átjáró megnyílik az adatforrások hitelesítő adatainak beállítása olyan portot. 
- Az endpoint **állapota**
- **Az SSL-tanúsítvány** megtekintése szolgál az SSL-portál és az átjáró kommunikációját adatforrások hitelesítő adatainak beállítása.  

### <a name="diagnostics-page"></a>Diagnosztikai lap
A diagnosztikai lap lehetővé teszi, hogy végezze el az alábbi műveleteket:

- Részletes **naplózás**engedélyezése, naplók megtekintése az Eseménynapló nevet, és a naplók küldése a Microsoftnak, ha egy sikertelen volt.
- **A kapcsolat tesztelése** adatforrást.  

### <a name="help-page"></a>Súgó lapja
A súgó lapon az alábbi információkat jeleníti meg:  

- Rövid leírást az átjáró
- Verziószám
- Online súgó, adatvédelmi nyilatkozata és licencszerződést mutató hivatkozásokat tartalmaz.  

## <a name="troubleshooting"></a>Hibaelhárítás

- A Windows-eseménynaplók naplózza az átjáró részletes információkat is megkeresheti. A Windows **Eseménynapló nevet** az **alkalmazás- és szolgáltatásnaplók**megtalálhatja őket > **Az adatkezelési átjáró**. Ha a hiba elhárításához átjáró kapcsolatos problémákat, keresse meg a hiba szintű események az Eseménynapló megjelenítő.
- Az átjáró összeomlik **a tanúsítvány lecserélése**után, indítsa újra az **Adatkezelési átjáró adatszolgáltatás** a Microsoft adatkezelési átjáró konfigurációkezelőjének eszközzel vagy a szolgáltatások vezérlőpultja. Ha továbbra is hibaüzenet jelenik meg, előfordulhat, adjon a tanúsítványok Manager (certmgr.msc parancsot) a tanúsítvány eléréséhez az adatkezelési átjáró szolgáltatás felhasználó közvetlen engedélyeinek.  A szolgáltatás az alapértelmezett felhasználói fiók: **NT Service\DIAHostService**. 
- Ha adatok gyári szerkesztő titkosítása gombra kattint a **Hitelesítőadat-kezelő** alkalmazás nem sikerül **titkosítsa** a hitelesítő adatait, ellenőrizze, hogy az alkalmazás az **átjárót futtató számítógépen**futtatott. Ha nem, futtassa az alkalmazást az átjárót futtató számítógépen, és próbálja meg a hitelesítő adatainak titkosítására.  
- Ha látható adatok tárolása kapcsolat vagy illesztőprogram kapcsolatos hibákat, hajtsa végre az alábbi lépéseket: 
    - Indítsa el **Az adatkezelési átjáró konfigurációkezelőjének** az átjárót futtató számítógépen.
    - Kattintson a **Diagnosztika** fülre.
    - Jelölje ki vagy adja meg megfelelő értéket mezők csoportjának **a helyszíni adatforráshoz, ezt az átjárót a kapcsolat tesztelése**
    - Annak ellenőrzéséhez, hogy az átjárót futtató számítógépen, a kapcsolat adatait, és a hitelesítő adatok a helyszíni adatforrás csatlakoztathatja a **kapcsolat tesztelése** gombra. Ha a kapcsolat tesztelése még mindig nem sikerül egy eszközillesztőt telepítése után, indítsa újra az átjáró ahhoz, hogy az utolsó módosítás felveszi.  

    ![A kapcsolat tesztelése](./media/data-factory-data-management-gateway/TestConnection.png)

### <a name="send-gateway-logs-to-microsoft"></a>Átjáró naplók küldése a Microsoftnak
Amikor, lépjen kapcsolatba a Microsoft Support segítség az átjáró problémák megoldásához, akkor ahhoz, hogy az átjáró naplók felszólíthatja. Az átjáró kiadását lehetővé teszi, hogy egyszerűen megoszthatja a szükséges átjáró naplók gombra kattintással az átjáró Konfigurációkezelőjében keresztül.   

1. Váltson az átjáró konfigurációkezelőjének **Diagnosztika** fülre.
 
    ![Az adatkezelési átjáró - Diagnosztika lapon](media/data-factory-data-management-gateway/data-management-gateway-diagnostics-tab.png)
2. A következő párbeszédpanel megjelenítéséhez **naplók küldése** hivatkozásra kattintva: 

    ![Az adatkezelési átjáró – naplók küldése elemre](media/data-factory-data-management-gateway/data-management-gateway-send-logs-dialog.png)
3. (nem kötelező) Kattintson a **naplók megtekintése** , tekintse át az Eseménynapló.
4. (nem kötelező) Kattintson az **adatvédelmi** , tekintse át a Microsoft online szolgáltatások adatvédelmi nyilatkozata. 
3. Ha meg elégedve feltölteni, kattintson a ténylegesen naplók küldése az utóbbi hét napban a Microsoftnak hibaelhárítási **naplók küldése elemre** . Meg kell jelennie a Küldés naplók művelet a következő képen látható módon állapotának:

    ![Az adatkezelési átjáró - küldés naplózza állapota](media/data-factory-data-management-gateway/data-management-gateway-send-logs-status.png)
4. A művelet befejezése után, hogy egy párbeszédpanel az alábbi képen látható módon:
    
    ![Az adatkezelési átjáró - küldés naplózza állapota](media/data-factory-data-management-gateway/data-management-gateway-send-logs-result.png)
5. Megjegyzés: a **jelentés Azonosítót** lefelé, és ossza meg a Microsoft Support. A jelentés Azonosítót keresse meg a hibaelhárítási feltöltött átjáró naplók szolgál.  A jelentés Azonosítót is menti a program az Eseménynapló nevet a hivatkozásnak.  Keresse meg az esemény azonosító "25" megjeleníti, és jelölje be a dátum és idő.
    
    ![Az adatkezelési átjáró - küldés naplózza azonosítója](media/data-factory-data-management-gateway/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archiválás átjáró bejelentkezik az átjáró állomásgép
Vannak bizonyos esetekben, ahol átjáró hibát és átjáró naplók közvetlenül nem lehet megosztani: 

- Kézi az átjáró telepítése és regisztrálja az átjárót;
- Megpróbálja regisztrálni az átjárót a Konfigurációkezelő; regenerált használatával 
- Naplók küldése próbálja meg, és az átjáró host szolgáltatás nem kapcsolható ki;

Ezekben az esetekben átjáró naplók mentése zip-fájlként, és megoszthatja, ha később kapcsolatba a Microsoft-támogatást. Például ha hiba jelenik meg, az átjáró regisztrálása közben az alábbi képen látható:   

![Az adatkezelési átjáró - regisztrációs hiba](media/data-factory-data-management-gateway/data-management-gateway-registration-error.png)

Kattintson a archiválása és naplók mentése és megosztása a Microsoft terméktámogatási a zip-fájl **Archív átjáró** naplók hivatkozásra. 

![Az adatkezelési átjáró – archív naplók](media/data-factory-data-management-gateway/data-management-gateway-archive-logs.png)

### <a name="gateway-is-online-with-limited-functionality"></a>Átjáró mindig online korlátozott szolgáltatásokkal 
Az átjáró állapotát **korlátozott szolgáltatásokkal** online, az alábbi okok valamelyike látni.

- Átjáró a felhőbeli szolgáltatástól szolgáltatás bus keresztül nem tud csatlakozni.
- Felhőbeli szolgáltatástól átjáró szolgáltatás bus keresztül nem tud csatlakozni.

Ha az átjáró mindig online korlátozott szolgáltatásokkal, nem lehet a helyszíni adatok tárolók és az adatok másolása az adatok folyamatok létrehozása az gyári másolás varázsló segítségével.

Megoldás és megoldása (online korlátozott szolgáltatásokkal) alapuló e átjáró nem tud csatlakozni felhőalapú szolgáltatásba vagy más módon. A következő szakaszokban a következő megoldásokat alkalmazhatja. 

#### <a name="gateway-cannot-connect-to-cloud-service-through-service-bus"></a>Átjáró a felhőbeli szolgáltatástól szolgáltatás bus keresztül nem tud csatlakozni
Hajtsa végre az alábbi lépéseket az online vissza az átjáró: 

1. Engedélyezze a kimenő portok 9350-9354 átjárót futtató számítógépen is a Windows tűzfal és a vállalati tűzfal. Lásd: [portokat és a tűzfalat](#ports-and-firewall) szakasz részletes.
2. Az átjáró a proxykiszolgáló beállításainak konfigurálása Lásd: [Proxy kiszolgáló szempontok](#proxy-server-considerations) szakasz részletes. 

Kerülő szerkesztővel adatok gyári Azure portal (vagy) a Visual Studio (vagy) Azure PowerShell.

#### <a name="error-cloud-service-cannot-connect-to-gateway-through-service-bus"></a>Hiba: Felhőszolgáltatásba átjáró szolgáltatás bus keresztül nem tud csatlakozni.
Hajtsa végre az alábbi lépéseket az online vissza az átjáró:
 
1. Engedélyezze a kimenő portok 5671 és 9350-9354 átjárót futtató számítógépen is a Windows tűzfal és a vállalati tűzfal. Lásd: [portokat és a tűzfalat](#ports-and-firewall) szakasz részletes.
2. Az átjáró a proxykiszolgáló beállításainak konfigurálása Lásd: [Proxy kiszolgáló szempontok](#proxy-server-considerations) szakasz részletes.
3. A proxykiszolgálón statikus IP-korlátozás eltávolítása 

Kerülő adatokat gyári szerkesztő Azure portal (vagy) a Visual Studio (vagy) Azure PowerShell is használhatja.
 
## <a name="move-gateway-from-a-machine-to-another"></a>Áthelyezés átjáró számítógépről a másikra
Ez a szakasz lépéseit mozgó átjáró ügyfél egyik számítógépről egy másik számítógépen. 

2. Az portálon nyissa meg a **Data Factory kezdőlapján**, és kattintson a **Csatolt szolgáltatások** csempére. 

    ![Átjárók adatkapcsolat](./media/data-factory-data-management-gateway/DataGatewaysLink.png) 
3. Jelölje ki az átjáró a **Csatolt szolgáltatások** lap **Adatok ÁTJÁRÓK** szakaszában.
    
    ![Az átjáró kijelölve a csatolt szolgáltatások lap](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
4. Kattintson az **adatok átjáró** lap **Töltse le és telepítse az adatok átjáró**.
    
    ![Töltse le az átjáró-hivatkozás](./media/data-factory-data-management-gateway/DownloadGatewayLink.png) 
5. Kattintson a **beállítás** a lap **Töltse le és telepítse az adatok átjáró**majd kövesse az utasításokat követve telepítse az átjáró a számítógépen. 

    ![A lap konfigurálása](./media/data-factory-data-management-gateway/ConfigureBlade.png)
6. A **Microsoft adatkezelési átjáró konfigurációkezelőjének** nyitva hagyása. 
 
    ![Konfigurációkezelő](./media/data-factory-data-management-gateway/ConfigurationManager.png) 
7. A **Configure** fel a portálon kattintson a **kulcs hozza létre** a parancssávon, és kattintson az **Igen** gombra a figyelmeztető üzenet. Kattintson a **másolása gomb** melletti fő szöveget a vágólapra másolja a billentyűt. Az átjáró a régi gépen, amint hozza létre a kulcs megáll.  
    
    ![Hozza létre újból a billentyűt](./media/data-factory-data-management-gateway/RecreateKey.png)
     
8. A **kulcs** illessze be a számítógépen az **Adatkezelési átjáró konfigurációkezelőjének** **Regisztráljon az átjáró** lapon szövegdoboz. (nem kötelező) Kattintson a fő szövegét tekintheti **átjáró kulcsa megjelenítése** jelölőnégyzetet. 
 
    ![Másolás billentyű és a külső.FÜGGV](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
9. Kattintson a **regisztráció** regisztrálja az átjárót a felhőbeli szolgáltatással.
10. A **Beállítások** lapon kattintson a **Módosítás** , jelölje be a régi átjáró használt tanúsítvány, adja meg a **jelszót**, és kattintson a **Befejezés gombra**. 
 
    ![Adja meg a tanúsítvány](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

    Exportálhatja a tanúsítvány a régi átjáró a következő lépésekkel: az adatkezelési átjáró konfigurációkezelőjének indítsa el a régi gépre, kattintson a **tanúsítvány** fülre, kattintson az **Exportálás** gombra, és kövesse az utasításokat. 
10. Az átjáró sikeres regisztráció után meg kell jelennie a **regisztrációs** **regisztrálva** értékű, és **állapota** állapotértéke **Elindítva** a Kezdőlap lapon az átjáró Konfigurációkezelőjével. 

## <a name="encrypting-credentials"></a>Hitelesítő adatok titkosítása 
Titkosítsa a hitelesítő adatok gyári szerkesztő, tegye a következőket:

1. Indítsa el az **átjárót futtató számítógépen**böngészőn, nyissa meg [Azure portált](http://portal.azure.com). Az adatok gyári, ha szükséges, az **Adatok gyári** lap a megnyitott adatok gyári keresni, és kattintson a **Szerző és Deploy** adatok gyári szerkesztő indítása gombra.   
1. Kattintson egy meglévő **csatolt szolgáltatás** a fastruktúrájú nézetben azok JSON definícióját, vagy hozzon létre egy csatolt szolgáltatást, amelyhez adatkezelési átjáró (például: SQL Server vagy Oracle). 
2. A JSON-szerkesztőben **átjárónév** tulajdonság adja meg az átjáró nevére. 
3. Írja be a **connectionString**az **Adatforrás** tulajdonság kiszolgáló nevét.
4. Írja be a **Kezdeti katalógus** tulajdonság-adatbázis neve a **connectionString**.    
5. Kattintson a parancssáv, kattintson a megnyitó **titkosítása** gombjára-egyszer **Hitelesítőadat-kezelő** alkalmazást. Meg kell jelennie a **Beállítás hitelesítő adatok** párbeszédpanelen. 
    ![A beállítás hitelesítő adatok párbeszédpanelen](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
6. **A beállítás hitelesítő adatok** párbeszédpanelen hajtsa végre az alábbi lépéseket:  
    1.  Jelölje be a **hitelesítés** , amelyet az adatok gyári szolgáltatás használata az adatbázishoz való csatlakozáshoz. 
    2.  Írja be annak a felhasználónak, aki rendelkezik hozzáféréssel a **felhasználónév** beállítás az adatbázis nevét. 
    3.  Írja be a jelszót a **jelszó** beállítását a felhasználó számára.  
    4.  Hitelesítő adatainak titkosítására, és zárja be a párbeszédpanelt az **OK** gombra. 
5.  Ekkor megjelenik a **connectionString** **encryptedCredential** tulajdonság.      
        
            {
                "name": "SqlServerLinkedService",
                "properties": {
                    "type": "OnPremisesSqlServer",
                    "description": "",
                    "typeProperties": {
                        "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                        "gatewayName": "adftutorialgateway"
                    }
                }
            }

Az átjárót futtató számítógépen eltérő számítógépről szeretne elérni a portálon, győződjön meg arról, hogy, a hitelesítő adatok Manager alkalmazás tud csatlakozni az átjárót futtató számítógépen. Ha az alkalmazás nem tudják elérni az átjárót futtató számítógépen, akkor nem teszi lehetővé az adatforrás hitelesítő adatainak beállítása, és ellenőrizze az adatforrás-kapcsolatot.  

A **Beállítás hitelesítő adatok** alkalmazás használatakor a portálon a hitelesítő adatokat a megadott az átjárót futtató számítógépen az **Átjáró konfigurációkezelőjének** **tanúsítvány** lapján tanúsítvány titkosítja. 

Ha a hitelesítő adatainak titkosítására API-alapú megközelítés keres, a [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-parancsmag hitelesítő adatainak titkosítására is használhatja. A parancsmaggal a tanúsítványt használja, hogy az átjáró a hitelesítő adatainak titkosítására használatára van beállítva. Titkosított hitelesítő adatok hozzáadása a a JSON-ban a **connectionString** **EncryptedCredential** eleme. A JSON használhatja a [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) parancsmag vagy a adatok Factory-szerkesztőben. 

    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

Van egy további megközelítés hitelesítő adatok Factory-szerkesztő használata beállítást. Ha egy csatolt SQL Server-szolgáltatás hozzon létre a szerkesztőben, és írja be hitelesítő adatait a egyszerű szöveges formátumban, a hitelesítő adatok titkosított, amely az adatok gyári szolgáltatás tulajdonosa tanúsítvány használatával. Az tanúsítvány nem használható, hogy az átjáró használatára van beállítva. Lehet, hogy ezt a megközelítést kissé gyorsabb bizonyos esetekben, azt is kevésbé biztonságos. Emiatt azt javasoljuk, kövesse az eljárás csak teszteléshez fejlesztési /. 


## <a name="powershell-cmdlets"></a>PowerShell-parancsmagok 
Ez a szakasz ismerteti, hogyan hozhat létre és Azure PowerShell-parancsmagok használata az átjárók regisztrálni. 

1. Indítsa el az **Azure PowerShell** rendszergazdai módban. 
2. Jelentkezzen be az Azure-fiók futtassa az alábbi parancsot, majd írja be a Azure hitelesítő adatait. 

    Bejelentkezés-AzureRmAccount
2. A **New-AzureRmDataFactoryGateway** parancsmag használatával logikai átjáró létrehozása az alábbi képlettel történik:

        $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

    **Példa parancs és a kimeneti**:


        PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

        Name              : MyGateway
        Description       : gateway for walkthrough
        Version           :
        Status            : NeedRegistration
        VersionStatus     : None
        CreateTime        : 9/28/2014 10:58:22
        RegisterTime      :
        LastConnectTime   :
        ExpiryTime        :
        ProvisioningState : Succeeded
        Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI

    
4. Azure PowerShell, váltson a mappa: * *C:\Program Files\Microsoft adatok kezelése Gateway\2.0\PowerShellScript\**. Futtatása * *RegisterGateway.ps1* * a helyi változót társított * *$Key** ahogy a következő parancsot. Ez a parancsfájl regisztrál az ügyfél ügynök régebbi verzióiban létrehozott logikai átjáró a számítógépre telepítve.

        PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
        
        Agent registration is successful!

    Az átjáró egy távoli számítógépen regisztrálhatja a IsRegisterOnRemoteMachine paraméter használatával. Példa:
        
        .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true

5. A **Get-AzureRmDataFactoryGateway** parancsmag használatával az adatok gyári egyben az átjárók listájában. Ha az **állapot** oszlopban a **online**, az azt jelenti az átjáró egy használatra kész.

        Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

Távolítsa el a **Eltávolítása-AzureRmDataFactoryGateway** parancsmaggal az átjárók, és frissítse a **Set-AzureRmDataFactoryGateway** -parancsmagok használata az átjáró leírását. Szintaxisát és parancsmagok további részleteket lásd: adatok gyári parancsmagjai – referencia.  

### <a name="list-gateways-using-powershell"></a>Lista átjárók PowerShell használatával

    Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup

### <a name="remove-gateway-using-powershell"></a>Távolítsa el az átjáró PowerShell használatával
    
    Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force 


## <a name="next-steps"></a>Következő lépések
- Lásd: [adatok áthelyezése fiókok között a helyszíni és felhőbeli adatokat tárolja](data-factory-move-data-between-onprem-and-cloud.md) cikk. Az útmutató hozzon létre egy folyamat, amely az átjáró a helyszíni SQL Server-adatbázisból származó adatok áthelyezése az Azure blob használ.  
