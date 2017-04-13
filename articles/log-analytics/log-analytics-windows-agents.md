<properties
    pageTitle="Csatlakozás Windows rendszerű számítógépeken napló Analytics |} Microsoft Azure"
    description="Ez a cikk bemutatja a lépéseket a Windows számítógépek, a helyszíni infrastruktúra közvetlenül MOBILE használatával csatlakozni egy testre szabott verzióját a Microsoft figyelése ügynök (MMA)."
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


# <a name="connect-windows-computers-to-log-analytics"></a>Csatlakozás Windows rendszerű számítógépeken napló Analytics

Ebből a cikkből megtudhatja, hogy milyen lépéseket kell csatlakoztatása a Windows számítógépek, a helyszíni infrastruktúra közvetlenül MOBILE munkaterületek verziójával testre szabott a Microsoft figyelése ügynök (MMA). Akkor telepítenie kell és az összes kívánt számítógépek ügynökök csatlakoztatása sorrendben őket az adatok küldése a MOBILE és a megjelenítheti és a MOBILE portálon adatokat MOBILE beépített. Minden egyes ügynök jelentést küldhet a több munkaterületek.

Telepítheti ügynökök telepítővel parancssori, vagy a kívánt állapot konfigurációs (DSC) az Azure automatizálás.  

>[AZURE.NOTE] A virtuális gépeken futó operációs rendszert futtató Azure-ban, egyszerűsítheti telepítési a [virtuális gép bővítmény](log-analytics-azure-vm-extension.md)használatával.

Internetkapcsolattal rendelkező számítógépen a agent fogja használni a csatlakozik az internethez adatok küldése a MOBILE. Nem rendelkezik internetkapcsolattal számítógépeknél proxy vagy a MOBILE napló Analytics továbbító is használhatja.

Csatlakozás a Windows számítógépek MOBILE nem túl bonyolult feladat használatával 3 lépést kell elvégeznie:

1. Töltse le a ügynök telepítőfájl
2. A kiválasztott módszerrel ügynököt
3. Állítsa be a agent vagy további munkaterületek, hozzáadása, szükség esetén

A következő ábrán a Windows rendszerű számítógépeken és a MOBILE közötti kapcsolat már telepítette és beállította a ügynökök.

![Mobile-közvetlen-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Rendszerkövetelmények és a szükséges konfigurációs
Mielőtt telepítése vagy ügynökök üzembe, tekintse át az ahhoz szükséges követelményeknek, az alábbiakat.

- A MOBILE MMA csak telepítheti Windows Server 2008 SP 1 vagy újabb rendszerű számítógépeket és Windows 7 SP1 vagy újabb verziójában.
- Szüksége van egy MOBILE-előfizetést.  További információkért lásd a [napló Analytics – első lépések](log-analytics-get-started.md).
- Minden egyes Windows rendszerű csatlakozhat az internethez, a HTTPS kell lennie. Lehet, hogy a kapcsolat közvetlen, a proxy-e, és a MOBILE napló Analytics továbbító.
- A MOBILE MMA telepíthető különálló számítógépek, a kiszolgáló és a virtuális gépeken futó. Ha MOBILE szolgáltatott Azure virtuális gépeken futó csatlakozni szeretne, olvassa el a [Csatlakozás Azure virtuális gépeken futó a napló Analytics](log-analytics-azure-vm-extension.md).
- TCP 443-as port használható különböző erőforrások az ügynök igényeknek megfelelően. További tudnivalókért lásd: a [proxy és tűzfal beállításainak napló Analytics](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Töltse le a ügynök telepítőfájl MOBILE
1. A MOBILE portál az **Áttekintés** oldalon kattintson a **Beállítások** csempére.  Kattintson a **Kapcsolódó források** fülre a képernyő tetején.  
    ![Kapcsolódó források fülre.](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. **Közvetlenül a számítógépek csatolni**kattintson a számítógép processzor típusát a beállítási fájl letöltéséhez alkalmazandó **Töltse le a Windows-ügynök** .
3. Jobb oldalán lévő **Munkaterületi azonosítója**kattintson a Másolás ikonra, és illessze be a Jegyzettömbbe azonosítója.
4. Az **Elsődleges kulcs**a jobb oldalon kattintson a Másolás ikonra, és illessze be a Jegyzettömbbe a billentyűt.     
    ![másolja a munkaterület-Azonosítóját és az elsődleges kulcs](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>Ügynököt a beállítás használatával
1. Futtassa a ügynököt a használni kívánt számítógépen telepítőt.
2. A Kezdőlap lapon kattintson a **Tovább**gombra.
3. A licencfeltételek lapon olvassa el a licenc, és válassza az **elfogadom**.
4. Célmappát lapon módosítsa vagy megőrzése a alapértelmezett telepítési mappájában, és kattintson a **Tovább gombra**.
5. Ügynök beállítások lapján megadhatja, hogy az Azure napló Analytics (MOBILE), Operations Manager, a agent csatlakozni, vagy üresen is hagyhat a választási lehetőségek, ha be szeretné állítani a a agent később. Kattintson a **Tovább**gombra.   
    - Ha azt választotta, csatlakoztassa az Azure napló Analytics (MOBILE), illessze be a **Munkaterület Azonosítóját** és az előző eljárás a Jegyzettömbbe másolt **Munkaterület kulcs (elsődleges)** , és kattintson a **Tovább gombra**.  
        ![illessze be a munkaterület-Azonosítóját és az elsődleges kulcs](./media/log-analytics-windows-agents/connect-workspace.png)
    - Ha azt választotta, Operations Manager csatlakozni, írja be a **Kezelés csoport nevét**, a **Kezelés kiszolgáló** neve és a **Management Server Port**, és kattintson a **Tovább gombra**. Ügynök művelet fiókja lapján válassza a helyi rendszer fiók vagy a helyi tartományi fiók, és kattintson a **Tovább**gombra.  
        ![felügyeleti csoport konfigurációs](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![ügynök művelet fiók](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. A telepítés lap felkészült tekintse át a választási lehetőségek, és kattintson a **telepítés**.
7. A konfiguráció sikeresen befejeződött lapon, kattintson a **Befejezés gombra**.
8. Amikor elkészült, a **Microsoft figyelése Agent** jelenik meg a **Vezérlőpulton**. Tekintse át a konfigurációban van, és ellenőrizze, hogy a ügynök csatlakoztatva van-e a működési háttérismeretek (MOBILE). Amikor MOBILE, a agent jeleníti meg, egy üzenet arról: **a Microsoft műveletek Management Suite szolgáltatás sikeresen csatlakozott a Microsoft figyelése Agent.**

## <a name="install-the-agent-using-the-command-line"></a>A parancssorból ügynököt
- Módosítsa, majd az alábbi példa a parancssorból ügynököt.

    >[AZURE.NOTE] Ügynökszoftvert frissíteni szeretné, ha a napló Analytics API parancsfájlok használni szeretne. Ügynökszoftvert frissíteni a következő szakaszban olvashat.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>A agent frissítése, és adja hozzá a munkaterület parancsfájl használatával
Ügynökszoftvert frissítése, és adja hozzá a napló Analytics API parancsfájlok, az alábbi PowerShell példa használatával munkaterület.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Ha már használta a parancssor vagy parancsfájlt korábban telepítése és konfigurálása a ügynök, `EnableAzureOperationalInsights` lett cserélve `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Telepítse az Azure automatizálási DSC használata ügynök

>[AZURE.NOTE] Az eljárás és parancsfájl példában nem frissíti a meglévő ügynökszoftvert.

1. A xPSDesiredStateConfiguration DSC modul importálhat [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure automatizálási.  
2.  Azure automatizálási változó eszközök *OPSINSIGHTS_WS_ID* és *OPSINSIGHTS_WS_KEY*hozhat létre. *OPSINSIGHTS_WS_ID* beállítása a MOBILE napló Analytics munkaterület azonosító, és állítsa *OPSINSIGHTS_WS_KEY* az elsődleges kulcs a munkaterületet.
3.  Használja az alábbi parancsprogramot, és mentse MMAgent.ps1
4.  A következő példa használatával telepítse az Azure automatizálási DSC használata ügynök és módosítása. Azure automatizálási MMAgent.ps1 importálása az Azure automatizálási felület vagy a parancsmag használatával.
5.  Rendelje hozzá a csomópontot a konfiguráción. 15 percen belül a csomópont konfigurációját ellenőrzi, és a MMA fog tolni, a csomópontra.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Kézi konfigurálás ügynökkel, vagy további munkaterületek hozzáadása
Ha telepítette az ügynökök, de nem konfigurálta őket vagy ha azt szeretné, hogy a agent több munkaterületek el, az alábbi információk segítségével engedélyezése ügynökkel, és állítsa be újra, azt. A agent konfigurálása után a ügynök szolgáltatással regisztrálja, és fog kapni a szükséges konfigurációs adatok és a megoldás adatokat tartalmazó management csomagok.

1. Miután telepítette a Microsoft figyelése Agent, nyissa meg a **Vezérlőpultot**.
2. Nyissa meg **A Microsoft figyelése Agent** , és kattintson a az **Azure napló Analytics (MOBILE)** fülre.   
3. Kattintson a **Hozzáadás gombra** kattintva nyissa meg a **napló Analytics munkaterület hozzáadása** mezőbe.
4. Illessze be a **Munkaterület-azonosító** és a **Munkaterület kulcs (elsődleges)** kimásolt Jegyzettömb alkalmazásba az előző eljárás a munkaterület, amelyet szeretne hozzáadni, kattintson **az OK**gombra.  
    ![Műveleti háttérismeretek konfigurálása](./media/log-analytics-windows-agents/add-workspace.png)

Után adatgyűjtés számítógépekről a agent felügyeli, a MOBILE felügyeli számítógépek száma megjelennek a MOBILE portálon, a **Kapcsolódó források** lapon a **Beállítások** , **Kiszolgálókhoz csatlakozik**.


## <a name="to-disable-an-agent"></a>Ügynökszoftvert letiltása
1. Miután telepítette a agent, nyissa meg a **Vezérlőpultot**.
2. Nyissa meg a Microsoft figyelése Agent, és kattintson a az **Azure napló Analytics (MOBILE)** fülre.
3. Jelölje ki a munkaterület, és kattintson az **Eltávolítás**. Ismételje meg ezt a lépést az összes többi munkaterület.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Tetszés szerint a el egy Operations Manager-kezelés csoport ügynökök beállítása

Az informatikai infrastruktúrát Operations Manager használ, ha Ön is használhatja az MMA ügynök Operations Manager ügynökeként.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>El egy Operations Manager-kezelés csoport MMA ügynökök beállítása
1.  Azon a számítógépen, amelyen a ügynök telepítve van nyissa meg a **Vezérlőpultot**.
2.  Nyissa meg **A Microsoft figyelése Agent** , és válassza a a **Operations Manager** fülre.
    ![Microsoft figyelése ügynök Operations Manager lap](./media/log-analytics-windows-agents/om-mg01.png)
3.  Ha az Operations Manager-kiszolgálókon integrálása az Active Directory, kattintson az **automatikus frissítése az Active Directory tartományi szolgáltatásokból kezelése csoport-hozzárendelések**.
4.  Kattintson a **Hozzáadás** a **kezelés csoport hozzáadása** párbeszédpanel megnyitásához.  
    ![A Microsoft figyelése Agent kezelése csoport hozzáadása](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  **Felügyeleti csoport neve** mezőbe írja be a kezelés csoport nevét.
6.  **Elsődleges management server** mezőbe írja be az elsődleges management Server a számítógép nevét.
7.  **Management server port** mezőbe írja be a TCP-portszámot.
8.  **Ügynök művelet fiók**csoportjában válassza a helyi rendszerfiók vagy egy helyi tartományi fiók elemre.
9.  A **kezelés csoport hozzáadása** párbeszédpanel bezárásához, és kattintson az **OK gombra** kattintva zárja be a **Microsoft figyelése ügynök tulajdonságai** párbeszédpanelen az **OK** gombra.

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Tetszés szerint állítsa be a MOBILE napló Analytics továbbító a használni kívánt

Kiszolgálók és ügyfelek, amelyeken nincs olyan internetes kapcsolat esetén továbbra is lehet őket MOBILE adatok küldése a MOBILE napló Analytics továbbító használatával.  A továbbító használatakor ügynökök adatok küldi el az Internet-hozzáféréssel rendelkező egyetlen kiszolgálón keresztül. A továbbító át adatokat az ügynökök a MOBILE nélkül, közvetlenül bármelyik átvitt adatok elemzése.

Lásd: a [MOBILE napló Analytics továbbító](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) , ha többet szeretne tudni a továbbító, beleértve a beállítás és konfiguráció.

A proxykiszolgáló, amely ebben az esetben a MOBILE továbbító, használata ügynökök konfigurálásáról című témakörből [napló Analytics-proxy és tűzfal beállításainak konfigurálása](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Ha szükséges a proxy és a tűzfal beállításainak konfigurálása
Ha proxykiszolgáló vagy a tűzfalak a környezetben, amely korlátozhatja a hozzáférést az interneten tekintse meg a [proxy és tűzfal beállításainak napló Analytics](log-analytics-proxy-firewall.md) ahhoz, hogy a közös kommunikáció a MOBILE szolgáltatás ügynökök.

## <a name="next-steps"></a>Következő lépések

- [Log Analytics hozzáadása megoldások a megoldástárból](log-analytics-add-solutions.md) funkciókat és a szükséges adatok összegyűjtése.
- [A proxykiszolgáló és a tűzfal beállításainak napló Analytics](log-analytics-proxy-firewall.md) Ha szervezete használja a proxykiszolgáló vagy a tűzfalat, hogy ügynökök kommunikálni tudjanak a napló Analytics szolgáltatás.
