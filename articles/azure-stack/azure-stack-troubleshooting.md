<properties
    pageTitle="Microsoft Azure Papírhalom elhárítása |} Microsoft Azure"
    description="Azure Papírhalom hibaelhárítás."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure Papírhalom – hibaelhárítás

A dokumentum Azure Papírhalom közös hibaelhárítási információkat tartalmaz.

Az optimális használat érdekében győződjön meg arról, hogy a telepítési környezet felel meg az összes [követelmények](azure-stack-deploy.md) és [előkészületet](azure-stack-run-powershell-script.md) telepítése előtt. 

Hibaelhárítási problémák vannak a jelen szakaszban ismertetett a javaslatok számos különböző forrásból származik, és előfordulhat, hogy, vagy nem szünteti meg az adott problémát. Ha nem dokumentált problémát tapasztal, akkor ellenőrizze, hogy az [Azure Papírhalom fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) további támogatási és további információt.

Kód példák találhatók, mint, és nem lehet garantálni a várt eredményeket. Ez a szakasz megegyezik érvényes gyakori szerkesztések és frissítések végrehajtják a termék javított.

  

## <a name="known-issues"></a>Ismert problémák

 - Az alábbi nem végződő hibák a telepítés során, amelyek befolyásolják a telepítési sikeres jelenhetnek meg:
     - "A"C:\WinRM\Start-Logging.ps1"kifejezés szolgáltatáscsomagja nem ismerhető fel"
     - "Meghívásához EceAction: nem tárgymutató-be egy üres tömböt" 
     - "InvokeEceAction: argumentum nem lehet kapcsolódni a paraméter"Üzenet", mivel az üres karakterlánc."
 - Sikertelen volt szolgáltatónál 60.61.93 hiba történt "Alkalmazás nem található URI azonosítójú." telepítési jelenhetnek meg Ez a jelenség oka az, hogy alkalmazásokat az Azure Active Directory regisztrált módját.  Ez a hibaüzenet jelenik meg, ha továbbra is [futtassa újra a telepítési parancsfájlt](azure-stack-rerun-deploy.md) lépésben 60.61.93 telepítési befejeződéséig.
 - Láthatja, hogy az erőforrás **Elérhetőségének beállítása** a piactér jelenik meg a **virtualMachine-ARM** kategóriában – ezt a megjelenítést csak kozmetikai probléma.
 - A portálon, az **alapvető** lépésben egy új virtuális gép létrehozásakor a tárolási lehetőség SSD alapértelmezés szerint.  Ezt a beállítást meg kell változtatni, vagy a **méret** lapon a virtuális telepítés merevlemez, nem jelenik meg virtuális méretű érhető el, jelölje ki, és a telepítés folytatásához. 
 - Ekkor megjelenik a m/m-CON01 virtuális alapértelmezés szerint nem telepített AzureRM PowerShell-modulok (a TP1 ez volt nevű ClientVM). Ez a jelenség szándékosan van így, mivel [ezeket a modul telepítése](azure-stack-connect-powershell.md)és csatlakozás egy másik módszert.  
 - Láthatja, hogy a **Microsoft.Insights** erőforrás-szolgáltató nem van automatikusan regisztrálva bérlői előfizetésekhez. Ha szeretné, hogy-adatok figyelése a bérlő telepítése virtuális megjelenítéséhez, futtassa a következő parancsot a PowerShell (után, [telepíthet, és csatlakoztathatja](azure-stack-connect-powershell.md) a bérlő): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Ekkor megjelenik az erőforrás-csoportok funkciót a portálon exportálni, azonban nem szöveg megjelenített és érhető el az exportálás.      
 - A telepítési nagyobb, mint a rendelkezésre álló kvóta tároló-erőforrások elindíthatja.  A telepítés nem sikerült, és a fiók erőforrások fel kell függeszteni.  Kétféleképpen lehet remediation érhető el:
     - Szolgáltatás-rendszergazda kvóta, növelje meg, hogy a módosításokat nem módosítása azonnal érvénybe lép, és nem gyakran foglalnak órává történő továbbításához.
     - Szolgáltatás-rendszergazda, amely a bérlő kattintson az előfizetés is adhat további kvóta bővítmény csomagjával hozhat létre.
 - A portál VMs létrehozni a Azure Papírhalom környezetekben a "Azure - Kína" identitással használatakor nem fogja látni a virtuális méretű érhető el, jelölje ki a virtuális telepítési **méret** lépésénél, és továbbra is a telepítő nem tudja.
 - Amikor a virtuális sikeresen ténylegesen rendszerbe az portálon telepítési hiba jelenhet meg.
 - Amikor töröl egy csomagra, ajánlatot, vagy -előfizetésre, VMs nem lehet törölni.
 - Ekkor megjelenik a virtuális bővítmények a a piactéren.
 - A virtuális egy mentett virtuális kép nem telepíthető.
 - Szolgáltatások, amelyek nem szerepelnek az előfizetése bérlők jelenhetnek meg.  Ha a bérlők próbálja meg telepíteni, ezek az erőforrások, hibaüzenetet kapnak.  Példa: A bérlői előfizetés csak tartalmaz tárolási erőforrásokat.  Bérlői más erőforrások, mint a VMs létrehozásra szolgáló parancs jelenik meg.  Ebben az esetben a bérlő egy virtuális, üzembe alkalommal, amikor egy megjelenő üzenet jelzi, hogy a virtuális nem hozható létre. 
 - TP2 telepítésekor a virtuális, feltéve, ha futtatja az Azure Papírhalom telepítési parancsfájlt, vagy jelentéshez Windows üzenetküldés hiba jelenhet meg az operációs rendszer hamarosan lejár az állomás nem aktiválnia kell.


## <a name="deployment"></a>Telepítési

### <a name="deployment-failure"></a>Telepítési hiba
Hibát tapasztal a telepítés során, az Azure Papírhalom telepítőt lehetővé teszi a [futtassa újra a telepítési lépéseket](azure-stack-rerun-deploy.md)követve telepítése sikertelen volt a folytatáshoz.

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Végén található a telepítést a PowerShell-munkamenetet a még nyitva, és nem jeleníti meg bármely kimeneti

Ez a probléma valószínűleg csak egy PowerShell-parancsablakban alapértelmezett viselkedésének eredményét, ha azt választotta-e. A ez példányban ténylegesen sikerült, de a parancsfájl szüneteltetve, az ablak kiválasztásakor. Ellenőrizheti, hogy ez a helyzet a Word megjeleníti "select" a parancssorablakban címsorában.  Nyomja le az ESC billentyű lenyomásával a kijelölés megszüntetése azt, és azt követően az üzenet jelenjenek meg.

## <a name="templates"></a>Sablonok

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure-sablon Azure jegyzettömbhöz nem terjesztése

Győződjön meg arról, hogy:

- A sablon a Microsoft Azure szolgáltatás már elérhető, és a képen Azure egymást fedő kell használnia.
- Az Azure Papírhalom helyi példányt által támogatott egy adott erőforrás használt API-khoz, és hogy céloz meg egy érvényes helyet ("helyi" az Azure Papírhalom technikai előzetes verzió (Eszközpanel) 2, és a "USA-beli kelet" vagy "Dél-indiai" Azure-ban).
- [Ez a cikk](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) arról, hogy a próba-AzureRmResourceGroupDeployment parancsmagok elfog Azure erőforrás-kezelő szintaxis kis különbségeinek áttekintése

Az Azure Papírhalom sablonjainak már a [GitHub tárházba](http://aka.ms/AzureStackGitHub/) segítségével segítséget nyújt az első lépések.

## <a name="virtual-machines"></a>Virtuális gépeken futó

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>A Microsoft Azure Papírhalom ez host indítás után a bérlők VMs eltűnnek a Hyper-V kezelője, és térjen vissza automatikusan egy kicsit várakozás után?

Mivel a rendszer vissza megtalálható a tároló alrendszer és RPs szükséges konzisztencia megállapításához. A szükséges függ, hogy a hardver és specs-használatban, de lehet, hogy egy kis időt, a bérlő térjen vissza, és ismeri VMs fogadó újraindítása után.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Lehet néhány virtuális gépeken futó törölt, de továbbra is látni fogja a lemezen virtuális fájlokat. Ez a jelenség várható?

Igen, az elvárt működés. Mivel az ilyen módon készült:

- Amikor töröl egy virtuális, VHD nem törlődnek. Lemezen külön erőforrások erőforráscsoport.
- A tárhely fiók törlésekor a kap a törlés azonnal Azure erőforrás-kezelővel (portálon PowerShell) látható, de tartalmazhat lemezt továbbra is megmaradnak tárolóban lévő mindaddig, amíg fut szemétgyűjtő.

"Árvasorok" VHD jelenik meg, érdemes szem előtt tartani, ha a tárterület-fiókot, amelyet törölt mappa részét képezik. A tároló fiók törlése nem, érdemes normál vannak továbbra is megtalálhatók.

Erről további [tárolás fiókokat](azure-stack-manage-storage-accounts.md)az adatmegőrzési küszöb konfigurálásáról.

## <a name="installation-steps"></a>Telepítési lépései
Előfordulhat, hogy hasznos hibaelhárítási célból Azure Papírhalom telepítési lépéseket az alábbi adatokat:  

| Index | név | Leírás|
| ----- | ----- | -----|
|értékek a 0,11 | (DEP) Fizikai gépek ellenőrzése | A hardver- és a fizikai csomópontok OS konfigurációjának ellenőrzése. |
| 0.12 | (DEP) Ez a fizikai gépek hálózat konfigurálása | Virtuális hálózati kapcsolók és a kapcsolatok konfigurálása. |
| 0.14 | (DEP) Tartomány terjesztése | Telepítse az Active Directory virtuális gépen. |
| 0,15 | (DEP) A tartomány beállítása | Állítsa be a tartomány rekordjainak a biztonsági csoportokat stb. |
| 0,16 | (DEP) Fizikai gép beállítása | Állítsa be a hálózat, illesztés tartomány és a helyi rendszergazdák beállítása. |
| 0,18 | ÖSSZEGYŰJTÉS (L) Tárterület fürt konfigurálása | Tárterület fürt létrehozása, tároló készlet és a fájl kiszolgálót hozhat létre. |
| 0.19 | (KÖLTSÉGINDEX) Beállítás háló infrastruktúra | Állítsa be a Előfeltételek, telepítés háló. |
| 0.21 | (NETTÓ) BGP és a hálózati Címfordítást beállítása | A telepítést BGP és a hálózati Címfordítást - csak egy csomópont szükséges. |
| 0.22 | (NETTÓ) Hálózati Címfordítást és időkiszolgáló konfigurálása | Szinkronizálja a kiszolgálót, és beállítja a hálózati címfordító Eszközig bejegyzések. |
| 40.41 | (KÖLTSÉGINDEX) A vendégként való bekapcsolódáshoz VMs létrehozása | Hozzon létre VMs kezelését. |
| 40.42 | (FBI) Állítsa be a PowerShell JEA | Állítsa be a PowerShell JEA összes szerepkörének. |
| 40.43 | (FBI) Azure Papírhalom hitelesítésszolgáltató beállítása | Azure Papírhalom hitelesítésszolgáltató és a telepítést. |
| 40.44 | (FBI) Azure Papírhalom hitelesítésszolgáltató konfigurálása | Adja meg az Azure Papírhalom hitelesítésszolgáltató. |
| 40.45 | (NETTÓ) A VMs NC beállítása | A vendégként való bekapcsolódáshoz VMs NC telepítése |
| 40.46 | (NETTÓ) NC VMs konfigurálása | A vendégként való bekapcsolódáshoz VMs NC konfigurálása |
| 40.47 | (NETTÓ) A vendégként való bekapcsolódáshoz VMs konfigurálása | Állítsa be az adatkezelési VMs NC hozzáférés-vezérlési listák. |
| 60.61.81 | (FBI) Azure Papírhalom háló csengetés Services - FabricRing előfeltétel telepítése | VIP FabricRing-hoz |
| 60.61.82 | (FBI) Azure Papírhalom háló csengetés szolgáltatásainak üzembe - háló csengetés fürt terjesztése | Telepíti és Azure Papírhalom háló csengetés fürt beállítja. |
| 60.61.83 | (FBI) Erőforrás-szolgáltatók felügyeleti bővítmények üzembe helyezése | Erőforrás-szolgáltatók felügyeleti bővítmények telepítése |
| 60.61.84 | (ACS) Állítsa be az Azure egységes tároló csomópont szintjén. | Telepíti, és Azure egységes tároló konfigurálja a csomópont szintjén. |
| 60.61.85 | (ACS) Azure egységes tároló beállítása fürt szintjét. | Telepíti és Azure egységes tároló beállítja fürt szintjén. |
| 60.61.86 | (FBI) Azure Papírhalom háló csengetés vezérlő Services - előfeltétel telepítése | InfraServiceController előfeltételei |
| 60.61.87 | (FBI) Azure Papírhalom háló csengetés vezérlő Services - előfeltétel telepítése | Költségindex előfeltételei |
| 60.61.88 | (FBI) Azure Papírhalom háló csengetés vezérlő Services - előfeltétel telepítése | ASAppGateway előfeltételei |
| 60.61.89 | (FBI) Azure Papírhalom háló csengetés vezérlő Services - előfeltétel telepítése | Tárterület-vezérlő vonatkozó követelmények |
| 60.61.90 | (FBI) Azure Papírhalom háló csengetés vezérlő Services - előfeltétel telepítése | HealthMonitoring előfeltételei |
| 60.61.91 | (FBI) Azure Papírhalom háló csengetés vezérlő Services - előfeltétel telepítése | Dokumentumokat előfeltételei |
| 60.61.92 | (FBI) Azure Papírhalom háló csengetés vezérlő Services - előfeltétel telepítése | PMM előfeltételei |
| 60.61.93 | (Katal) AzureStack szolgáltatás rendszerbiztonsági létrehozása | Azure Graph-alkalmazások és a szolgáltatás rendszerbiztonsági AAD hozhat létre. |
| 60.61.94 | (NETTÓ) A telepítő GW VMs | A vendégként való bekapcsolódáshoz VMs GW telepíti. |
| 60.61.95 | (NETTÓ) GW VMs konfigurálása | A vendégként való bekapcsolódáshoz VMs GW állítja be. |
| 60.61.96 | (NETTÓ) A hosts IDN terjesztése | A hosts infrastruktúra IDN terjesztése |
| 60.61.97 | (NETTÓ) IDN konfigurálása | IDN szerepkör konfigurálása |
| 60.61.98 | (FBI) A telepítő WSUS VMs | A vendégként való bekapcsolódáshoz VMs WSUS server telepítése. |
| 60.61.99 | (FBI) WSUS VMs konfigurálása | A vendégként való bekapcsolódáshoz VMs WSUS-kiszolgáló konfigurálása |
| 60.61.100 | (FBI) Az SQL Azure-VMs beállítása | A vendégként való bekapcsolódáshoz VMs Azure SQL server telepítése |
| 60.61.101 | (Katal) A telepítő előfeltételei VMs volt. | A vendégként való bekapcsolódáshoz VMs a Microsoft Azure egymást fedő előfeltételei beállítja. |
| 60.61.102 | (Katal) A telepítő LETT VMs | A vendégként való bekapcsolódáshoz VMs Microsoft Azure Papírhalom telepíti. |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.121 | (FBI) Erőforrás-szolgáltatók és vezérlők terjesztése | Erőforrás-szolgáltatók és vezérlők telepítések |
| 60.120.122 | (FBI) Vezérlő beállításairól | Vezérlők konfigurálása |
| 60.120.123 | (Katal) Állítsa be a WAS VMs | A vendégként való bekapcsolódáshoz VMs állítja be a Microsoft Azure Papírhalom. |
| 60.120.124 | (Katal) Azure Papírhalom AAD konfiguráció. | Azure Papírhalom konfigurálja az Azure Active Directory. |
| 60.120.125 | (Katal) ADFS telepítése | Telepíti az Active Directory összevonási szolgáltatások (ADFS) |
| 60.120.126 | (Katal) ADFS/Graph telepítése | Azure Papírhalom Graph telepítések |
| 60.120.127 | (Katal) ADFS konfigurálása | Konfigurálja az Active Directory összevonási szolgáltatások (ADFS) |
| 60.140.141 | (FBI) Összegző konfigurálása | Beállítja a tárhelyet erőforrás-szolgáltató |
| 60.140.142 | (ACS) Azure egységes tároló konfigurálása. | Adja meg az Azure egységes tárolás. |
| 60.140.143 | (FBI) Tárterület-fiókok létrehozása | Az összes tároló fiókok létrehozása különböző szolgáltatók való használatra. |
| 60.140.144 | (FBI) Az összegző külső.FÜGGV használatát | Regisztráljon használatát tárolás szolgáltató. |
| 60.140.145 | (KÖLTSÉGINDEX) Költségindex létrehozott VMs, a Hosts és a fürt áttelepítése | A létrehozott VMs, Hosts és fürt objektumok áttelepíti a Költségindex |
| 60.140.146 | (FBI) A Windows Defender konfigurálása | A Windows Defender konfigurálása |
| 60.160.161 | (HÉTFŐ – VASÁRNAP) Figyelés ügynökök beállítása | Konfigurálja a ügynök figyelése |
| 60.160.162 | (FBI) NRP Előfeltételek | Telepíti az NRP vonatkozó követelmények |
| 60.160.163 | (FBI) NRP telepítési | NRP telepítések  |
| 60.160.164 | (FBI) NRP konfigurálása | NRP konfigurálása |
| 60.160.165 | (FBI) KSZT Előfeltételek | Telepíti az KSZT vonatkozó követelmények |
| 60.160.166 | (FBI) KSZT telepítési | KSZT telepítések |
| 60.160.167 | (FBI) KSZT konfigurálása | KSZT konfigurálása |
| 60.160.168 | (FBI) FRP Előfeltételek | Telepíti az FRP vonatkozó követelmények |
| 60.160.169 | (FBI) FRP telepítési | FRP telepítések  |
| 60.160.170 | (FBI) FRP konfigurálása | FRP konfigurálása |
| 60.160.174 | (FBI) Előzetesen szükséges URP | Telepíti az URP vonatkozó követelmények |
| 60.160.175 | (FBI) Telepítési URP | URP telepítések  |
| 60.160.176 | (FBI) Konfigurációs URP | Konfigurálja a URP |
| 60.160.171 | (FBI) HRP Előfeltételek | Telepíti az HRP vonatkozó követelmények |
| 60.160.172 | (FBI) HRP telepítési | HRP telepítések  |
| 60.160.173 | (FBI) HRP konfigurálása | HRP konfigurálása |
| 60.160.177 | (KV) KeyVault Előfeltételek | Telepíti KeyVault vonatkozó követelmények |
| 60.160.178 | (KV) KeyVault telepítési | KeyVault telepítések  |
| 60.160.179 | (KV) KeyVault konfigurálása | KeyVault konfigurálása | 
| 60.190.191 | (FBI) Tár beállítása | Tár beállítása |
| 60.190.192 | (FBI) Háló csengetés szolgáltatások beállítása: | Háló csengetés szolgáltatások beállítása: |
| 60.221 | (FBI) Konzol VMs beállítása | A vendégként való bekapcsolódáshoz VMs konzol server telepítése. |
| 60.222 | (FBI) Konzol VMs beállítása | DVM tartalom áthelyezése a virtuális konzolt. |
| 251 | Felkészülés a jövőbeli host újraindítása | Indítsa újra a rendszert szabály beállítása |


## <a name="next-steps"></a>Következő lépések

[Gyakori kérdések](azure-stack-FAQ.md)
