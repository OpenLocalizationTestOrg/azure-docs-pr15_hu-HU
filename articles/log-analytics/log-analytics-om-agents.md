<properties
    pageTitle="Csatlakozás Operations Manager napló Analytics |} Microsoft Azure"
    description="A meglévő System Center Operations Manager befektetés és bővített funkciók használata a napló Analytics, Operations Manager integrálhatja a MOBILE munkaterülettel."
    services="log-analytics"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="magoedte"/>

# <a name="connect-operations-manager-to-log-analytics"></a>Log Analytics Operations Manager csatlakoztatása

A meglévő System Center Operations Manager befektetés és bővített funkciók használata a napló Analytics, Operations Manager integrálhatja a MOBILE munkaterülettel.  Ebben a csoportban adhatja meg MOBILE lehetőségek kihasználhatja továbbra is az Operations Manager használata közben:

- Folytassa az IT-szolgáltatások Operations Manager állapotának ellenőrzése
- Integráció a ITSM megoldásokkal az esemény és a probléma management támogató fenntartása
- A helyszíni és nyilvános felhő IaaS virtuális gépeken futó, hogy figyelheti a Operations Manager rendszerbe ügynökök életciklusának kezelése

System Center Operations Manager integrálása felveszi az value a szolgáltatás műveletek vonatkozó stratégia sebességének és gyűjteni, tárolása és Operations Manager adatainak elemzése a MOBILE hatékonyságának növelése szerint.  MOBILE egyeztetéséhez és a hibák, a problémák azonosítására és a meglévő probléma kezelési folyamat támogató reoccurrences útburkoló segítségével.   A teljesítményt, esemény és riasztási adatokat, a rich-irányítópultokkal és jelentéskészítési lehetőségek kattintva jelenítse meg az adatok jellemző módon szemügyre veszi a keresőprogram rugalmasan MOBILE elérhetővé teszi a Operations Manager complimenting erősségét mutatja be.

A jelentéskészítés az Operations Manager felügyeleti csoporthoz ügynökök adatgyűjtés alapján a napló Analytics adatforrások és megoldások MOBILE előfizetése engedélyezte, akkor a kiszolgálóról.  Attól függően, hogy a megoldás engedélyezte, ezek a megoldások adatainak vagy küldjük közvetlenül egy Operations Manager management server a MOBILE webszolgáltatás, illetve az ügynök felügyelt rendszer gyűjtött adatok mennyisége miatt közvetlenül a agent MOBILE webszolgáltatás elküldi. A management server közvetlenül továbbítja a MOBILE webszolgáltatás MOBILE adatokat, a program soha nem készült a OperationsManager vagy OperationsManagerDW adatbázishoz.  Ha egy kiszolgálóra megszakad a kapcsolat a MOBILE webszolgáltatással, azt gyorsítótárát az adatok helyileg addig, amíg a kapcsolati létesíteni MOBILE.  A management server tervezett karbantartás vagy tervezett üzemszünetek miatt offline állapotban, ha egy másik management server kezelése csoportjában található folytatódik MOBILE kapcsolatokkal.  

Az alábbi ábra szemlélteti a kezelési kiszolgálók és a System Center Operations Manager kezelése csoport és a MOBILE, az irány és a portokra ügynökök közötti kapcsolat.   

![Mobile-műveletek-kezelő-integráció-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

## <a name="system-requirements"></a>Rendszerkövetelmények
Mielőtt hozzákezdene, tekintse át a következő részletek ellenőrzéséhez előfeltétel teljesül.

- Csak akkor támogatja műveletek Manager 2012 SP1 UR6 MOBILE és a nagyobb, és a műveletek Manager 2012 R2 UR2 és nagyobb.  A proxykiszolgáló támogatási műveletek Manager 2012 SP1 UR7 és műveletek Manager 2012 R2 UR3 hozzá lett adva.
- Minden futó Operations Manager ügynökök megfelel a minimális támogatás követelményeknek. Győződjön meg arról, hogy ügynökök felfelé a minimális frissítés, egyéb esetben a Windows ügynök forgalom sikertelen lesz, sok hiba lehet, hogy töltse ki az Operations Manager eseménynaplójában talál.
- Egy MOBILE-előfizetést.  További tudnivalókért tekintse át a [napló Analytics – első lépések](log-analytics-get-started.md)elemre.

## <a name="connecting-operations-manager-to-oms"></a>Csatlakozás Operations Manager MOBILE
Hajtsa végre a következő lépéseket az Operations Manager kezelése csoport csatlakozhat egy MOBILE munkaterületek konfigurálása sorozata.

1. Jelölje ki a **felügyeleti** munkaterület a Operations Manager konzol.
2. Bontsa ki a tevékenységek kezelése csomagja csomópontot, és kattintson a **kapcsolat**elemre.
3. Kattintson a **Műveletek Management csomagja Register** hivatkozásra.
4. Kattintson a **Műveletek Management csomagja bevezetési varázsló: hitelesítési** oldalon adja meg az e-mail címét vagy telefonszámát, és a MOBILE előfizetése társított rendszergazdafiók jelszavát, és kattintson a **Bejelentkezés**gombra.
5. Akkor is sikeresen hitelesítését követően, a a **Műveletek Management csomagja bevezetési varázsló: Jelölje ki a munkaterület** lap, kéri, jelölje be a MOBILE munkaterület.  Ha egynél több munkaterület, jelölje ki a munkaterület kívánt regisztrálása az Operations Manager management group a legördülő listából, és kattintson a **Tovább**gombra.

    >[AZURE.NOTE] Operations Manager egyszerre csak egy MOBILE munkaterület használatát támogatja. A kapcsolatot, és a számítógépek, a korábbi munkaterületével MOBILE regisztráltak MOBILE törlődnek.

6. A a **Műveletek Management csomagja bevezetési varázsló: összefoglaló** oldalon beállításainak megerősítése és hagyják megfelelő, kattintson a **Létrehozás**gombra.
7. A a **Műveletek Management csomagja bevezetési varázsló: Befejezés** lapján kattintson a **Bezárás**gombra.

### <a name="add-agent-managed-computers"></a>Ügynök felügyelt számítógépek hozzáadása
Miután integráció a MOBILE-munkaterület, ezt csak a MOBILE kapcsolatot létesít, nem van gyűjtött adatokat a ügynökök jelentése a felügyeleti csoportba érkező. Ez nem fordulhat elő, amíg mely adott ügynök felügyelt számítógépek fog adatgyűjtés napló elemzéséhez konfigurálása után. Kiválaszthat vagy a számítógép-objektumok külön-külön, vagy választhatja ki egy csoportot, amely a Windows számítógép objektumokat tartalmaz. Nem választhat ki egy csoportot, amely egy másik osztály, például a logikai lemezen vagy SQL-adatbázisait példányát tartalmazza.

1. Nyissa meg az Operations Manager konzolt, és válassza a **felügyeleti** munkaterület.
2. Bontsa ki a tevékenységek kezelése csomagja csomópontot, és kattintson a **kapcsolat**elemre.
3. A **számítógép/csoport hozzáadása** hivatkozásra a Műveletek csoportban kattintson a jobb oldali ablaktábla címsor.
4. A **Számítógép** keresőmezőbe is kereshet számítógépek és a csoportok Operations Manager ellenőrizni. Jelölje ki a számítógépek és a csoportok, amelyeknek a MOBILE beépített, kattintson a **Hozzáadás**gombra, és kattintson **az OK**gombra.

Számítógépek és adatokat gyűjt a műveletek Management csomagja a műveletek konzol a **felügyeleti** munkaterületi felügyelt számítógépek csomópontját konfigurált csoportok megtekintése.  További lehetőségek hozzáadása vagy a számítógépek és csoportok eltávolítása szükség szerint.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>A műveletek konzol MOBILE proxy beállításainak konfigurálása
Ha egy belső proxykiszolgálója van, a kezelés csoport és a MOBILE webszolgáltatás között, végezze el az alábbi lépéseket.  Ezeket a beállításokat központilag felügyelt a felügyeleti csoportból és ügynök felügyelt rendszerek, az adatokat szeretne gyűjteni a MOBILE hatókörének szereplő elosztott.  Ez akkor hasznos, ha bizonyos megoldások a management server átlépése, és adatok küldése közvetlenül a MOBILE webszolgáltatás.

1. Nyissa meg az Operations Manager konzolt, és válassza a **felügyeleti** munkaterület.
2. Bontsa ki a műveletek Management csomagot, és kattintson a **kapcsolatok**gombra.
3. A MOBILE kapcsolatot nézetben kattintson a **Proxykiszolgáló beállítása**.
4. A **Műveletek Management csomagja varázsló: proxykiszolgáló** lap, válassza **a műveletek Management csomagja eléréséhez proxykiszolgáló használata**, majd írja be az URL-címe a portszámot, például a http://corpproxy:80, és kattintson a **Befejezés gombra**.

Ha a proxykiszolgáló hitelesítést igényel, a következő lépésekkel állítsa be a hitelesítő adatok és a beállításokat, amelyekkel felügyelt számítógépekre, amely a jelentést MOBILE tesz a kezelés csoport propagálása kell.

1. Nyissa meg az Operations Manager konzolt, és válassza a **felügyeleti** munkaterület.
2. **RunAs konfigurációs**válassza a **profilok**lehetőséget.
3. Nyissa meg a **System Center Advisor futtatása, profil Proxy** profilt.
4. A következőképpen profil varázslót kattintson a Hozzáadás Futtatás mint fiókkal gombra. Hozzon létre egy új [Futtatás mint fiókkal](https://technet.microsoft.com/library/hh321655.aspx) , vagy egy meglévő fiók használatával. Ehhez a fiókhoz át a proxykiszolgáló megfelelő engedélyekkel kell szerepelnie.
5. A fiók kezelése, válassza az **egy kijelölt osztály, csoport vagy objektum**beállításához kattintson a **Select...** és kattintson a **csoport...** a **Csoport** keresőmezőbe megnyitásához.
6. Keres, és válassza a **Microsoft rendszer központ Advisor figyelése Server csoport**.  Kattintson az **OK gombra** kattintva zárja be a **Csoport** keresőmezőbe a csoport kijelölése után.
7.  Kattintson az **OK gombra** a **Futtatás mint fiók hozzáadása** párbeszédpanel bezárásához.
8.  A varázsló, és mentse a módosításokat a **Mentés** gombra.

Miután a kapcsolat létrejött, és mely ügynökök lesz gyűjteni, és a MOBILE jelentésadatainak állítsa be, az alábbi beállításokkal kell alkalmazni, kattintson a kezelés csoport nem feltétlenül ebben a sorrendben:

- A Futtatás fiókot **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** jön létre.  A Futtatás profilként **Microsoft rendszer központ Advisor futtatása mint profil Blob** társított és a két osztály - **Gyűjtése kiszolgálót** és a **Műveletek Manager kezelése csoport**célját.
- Két összekötők jönnek létre.  Az első **Microsoft.SystemCenter.Advisor.DataConnector** neve, és automatikusan konfigurálja úgy továbbítja példányainak kezelése csoportjában található összes osztály a MOBILE napló Analytics által létrehozott minden riasztást előfizetéssel. A második rendelkezik **Advisor összekötő**, amely a felelős a MOBILE webszolgáltatás kommunikáció és -adatok megosztása.
- A **Microsoft rendszer központ Advisor figyelése Server csoport**ügynökök és csoportok kiválasztása után kattintson a kezelés csoport adatokat szeretne gyűjteni, hozzáadódik.

## <a name="management-pack-updates"></a>Felügyeleti csomag frissítések
Konfigurációs befejezése után az Operations Manager management group kapcsolatot hoz létre a MOBILE szolgáltatással.  A kulcskezelő kiszolgálótól a webszolgáltatás szinkronizálni, és a frissített konfigurációs adatok fogadására management csomagok engedélyezte a megoldások Operations Manager együttműködő formájában.   Ellenőrzi, hogy Operations Manager frissítéseit alábbi felügyeleti csomagok automatikusan töltse le és importálhatja őket, ha elérhetők.  Két szabályok vonatkoznak különösen ezt a viselkedésének szabályozásához, amely:

- **Microsoft.SystemCenter.Advisor.MPUpdate** - frissíti az alap MOBILE management csomagok. Tizenkét (12) óránként lefut alapértelmezés szerint.
- **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - frissítések megoldás management csomagok engedélyezve van a munkaterületen. Alapértelmezés szerint öt (5) percenként fut.

Két szabályok megakadályozhatja, hogy az automatikus letöltése letiltásával őket, vagy módosíthatja, milyen gyakran szinkronizálja a management server határozza meg, ha egy új felügyeleti csomag érhető el, és célszerű lehet letölteni MOBILE gyakoriságát bármikor felülbírálva.  Kövesse a lépéseket a **gyakoriság** paraméter értéket tartalmazó módosítása a szinkronizálási ütemezés másodpercben [felülbírálása egy szabályt, és a képernyő](https://technet.microsoft.com/library/hh212869.aspx) , vagy módosítsa a **engedélyezve** paramétert letiltása a szabályokat.  A felülírás műveletek Manager Management Group osztály összes objektumok Megcélozhat.

Ha szeretné folytatni követően a meglévő módosítása ellenőrzési folyamat biztonságkezelési felügyeleti csomag verziókban a termelési kezelése csoportjában, tiltsa le a szabályok, és engedélyezi őket, amikor frissítések engedélyezettek adott időben. Ha egy fejlesztés vagy a kérdések és válaszok kezelése csoport a környezetben, és rendelkezik internetkapcsolattal, beállíthatja a kezelés csoport a támogatási ebben az esetben egy MOBILE munkaterületével.  Ez lehetővé teszi, hogy tekintse át, és a MOBILE management csomagok közelítéses verzióiban felmérése felengedése őket a termelési felügyeleti csoportba előtt.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Váltás az Operations Manager csoport új MOBILE munkaterületre
1. Jelentkezzen be az MOBILE-előfizetése, és az új munkaterület létrehozása a [Microsoft műveletek Management Suite](http://oms.microsoft.com/).
2. Nyissa meg a Operations Manager konzolt olyan fiókkal, a műveletek kezelő rendszergazdák szerepkör tagja, és válassza a **felügyeleti** munkaterület.
3. Bontsa ki a műveletek Management csomagot, és jelölje be a **kapcsolatok**.
4. Jelölje be a középső ablaktábla mellett a **Művelet Management csomagja újra konfigurálása** hivatkozásra.
5. Hajtsa végre a **Műveleteket Management csomagja bevezetési varázsló** , és írja be az e-mail címét vagy telefonszámát, és az új MOBILE munkaterület társított rendszergazdai fiók jelszavát.

    > [AZURE.NOTE] A **Műveletek Management csomagja bevezetési varázsló: válassza ki a munkaterület** lap bemutatja a meglévő munkaterület használatban lévő.


## <a name="validate-operations-manager-integration-with-oms"></a>Műveletek Manager integráció a MOBILE ellenőrzése
Az alábbi néhány különböző módon ellenőrizheti, hogy az Operations Manager-integrációba a MOBILE sikeres.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Integráció a MOBILE portálról megerősítéséhez.

1.  A MOBILE portálon kattintson a **Beállítások** csempére
2.  Jelölje ki a **csatlakoztatott adatforrásból**.
3.  System Center Operations Manager csoportjában a táblázatban ekkor megjelennek a csoport nevét, a kezelés ügynökök és állapot számával együtt szerepel, adatok utolsó érkezésekor.

    ![Mobile-beállítások-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

4.  Megjegyzés: a **Munkaterület-azonosító** értéket a bal oldali a beállítások lap.  A Operations Manager kezelése csoportban az alábbi ellen ellenőrzi.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Integráció a műveletek konzolról megerősítéséhez.

1.  Nyissa meg az Operations Manager konzolt, és válassza a **felügyeleti** munkaterület.
2.  Válassza a **Kezelés csomagok** és a **keressen:** szöveg mezőbe írja be a **tanácsadó** vagy **üzletiintelligencia**.
3.  Attól függően, hogy engedélyezte a megoldásokkal látni fogja a megfelelő felügyeleti csomag a keresési eredmények között szerepel.  Például ha engedélyezte a riasztási projektmenedzsment megoldás, a felügyeleti csomag Microsoft rendszer központ Advisor riasztási Management lesz a listában.
4.  A **Figyelés** nézetből nyissa meg a **Műveletek felügyeleti Suite\Health állapot** megtekintése.  Jelölje ki a egy kiszolgálóra csoportban a **Kezelés kiszolgáló állapota** ablakban, és a **Részletek nézet** ablaktáblában erősítse meg a **hitelesítési szolgáltatás URI** tulajdonság értéke megegyezik a MOBILE munkaterület azonosítóját.

    ![OMS-opsmgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)


## <a name="remove-integration-with-oms"></a>Integráció a MOBILE eltávolítása
Ha már nincs szüksége a Operations Manager-kezelés csoport és a MOBILE munkaterület integrációja, akkor megfelelően el a kapcsolat beállítása a kezelés csoport több lépéseket. Az alábbi eljárás a MOBILE munkaterület frissítése az adatkezelési csoport lévő hivatkozás törlésével, törölje a MOBILE összekötők és törölje a MOBILE támogató management csomagok lesz.   

1.  Nyissa meg a műveletek Manager parancs rendszerhéj a műveletek kezelő rendszergazdák szerepkör-fiókból.

    >[AZURE.WARNING] Győződjön meg arról, egyetlen egyéni felügyeleti csomag a word Advisor vagy IntelligencePack a továbblépés előtt neve nem rendelkezik, egyéb esetben az alábbi lépésekkel törli őket a felügyeleti csoportból.

2.  Írja be a parancssor rendszerhéj`Get-SCOMManagementPack -name "*advisor*" | Remove-SCOMManagementPack`

3.  Következő típusa`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack`

4.  Nyissa meg a műveletek Manager műveletek konzol olyan fiókkal, a műveletek kezelő rendszergazdák szerepkör tagja.
5.  A **felügyeleti**lapon válassza a a **Kezelés csomagok** csomópontot és a **keres:** mezőbe írja be a **tanácsadó** , és ellenőrizze, hogy a kezelés csoport továbbra is importálásának módja a következő management csomagok:

    - A Microsoft System Center Advisor
    - A Microsoft System Center Advisor belső

6. A MOBILE portálon kattintson a **Beállítások** csempére.
7.  Jelölje ki a **csatlakoztatott adatforrásból**.
8.  System Center Operations Manager csoportjában a táblázatban ekkor megjelennek a csoport nevét, a kezelés el szeretné távolítani a munkaterületen.  Az oszlop **Utolsó adatot**csoportjában kattintson az **Eltávolítás**gombra.  

    >[AZURE.NOTE] Hivatkozás **törlése** nem lesz szabad 14 nap után, ha nincs tevékenység észlelt a csatlakoztatott felügyeleti csoportból.  
   
9.  Megjelenik egy ablak kéri, hogy erősítse meg, hogy szeretné-e az eltávolítás folytatásához.  Kattintson az **Igen gombra** a folytatáshoz. 

A két összekötők – törlése Microsoft.SystemCenter.Advisor.DataConnector és a tanácsadó összekötőre, az alábbi PowerShell-parancsprogramot mentése a számítógépre, és hajtsa végre, használja az alábbi példák.

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

>[AZURE.NOTE] A számítógép futtatja a parancsfájl, ha nem egy kiszolgálóra, a műveletek Manager 2012 SP1 vagy R2 parancs rendszerhéj, attól függően, hogy a kezelés csoport verziója telepítve kell rendelkeznie.

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

A jövőben Ha újracsatlakozás a kezelés csoport MOBILE-munkaterületre szóló, meg kell importálja újra az `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` felügyeleti csomag fájlból, a legutóbbi összesítő frissítés a felügyeleti csoportba alkalmazni.  Ehhez a fájlhoz megtalálhatja a `%ProgramFiles%\Microsoft System Center 2012` vagy a `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` mappát.

## <a name="next-steps"></a>Következő lépések

- [Log Analytics hozzáadása megoldások a megoldástárból](log-analytics-add-solutions.md) funkciókat és a szükséges adatok összegyűjtése.
- [A proxykiszolgáló és a tűzfal beállításainak napló Analytics](log-analytics-proxy-firewall.md) Ha szervezete használja a proxykiszolgáló vagy a tűzfalat, hogy ügynökök kommunikálni tudjanak a napló Analytics szolgáltatás.
