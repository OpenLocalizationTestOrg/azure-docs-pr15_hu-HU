<properties 
    pageTitle="Az SQL Server védelme az SQL Server vészhelyreállítás és Azure webhely helyreállítási |} Microsoft Azure" 
    description="Ez a cikk ismerteti, hogyan való replikáció SQL Server Azure webhely helyreállítási SQL Server-katasztrófa lehetőségeivel." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>Az SQL Server védelme az SQL Server vészhelyreállítás és Azure webhely helyreállítás 


A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md).

 Ez a cikk ismerteti az SQL Server BCDR technológiák és Azure webhely helyreállítási kombinációját alkalmazás vissza az SQL Server végére védelmét. Rendelkeznie kell az SQL Server katasztrófa helyreállítási lehetőségeket jól érthető (feladatátvétel használható, AlwaysOn rendelkezésre állási csoportok, az adatbázis tükrözés, jelentkezzen be a szállítási) és Azure webhely helyreállítási, mielőtt beállítaná a jelen cikkben ismertetett forgatókönyvek.



## <a name="overview"></a>– Áttekintés

Sok munkaterhelésekből használja az SQL Server egy foundation. Alkalmazások, például a SharePoint, a Dynamics és az SAP SQL Server használatával adatszolgáltatások végrehajtása.  Alkalmazások telepítése az SQL Server számos különböző módon:

- **Különálló SQL Server**: SQL Server és a minden adatbázisban tárolt egyetlen számítógépen (tényleges vagy egy virtuális). Virtualizálva, amikor fürtözés host helyi magas elérhetősége használható. Nincs vendég szintű magas elérhetőség áll rendelkezésre.
- **Az SQL Server feladatátvevő fürtözés-példányok (mindig a FCI)**: az SQL server-példány a megosztott lemez két vagy több csomópontok úgy van konfigurálva, a Windows Feladatátvevőfürt. Ha bármelyik fürt példányok lefelé, a fürt is átadni az SQL Server egy másik példányában. Ez a beállítás elsődleges webhelyen HA általában szolgál. Azt nem vírusvédelmet hiba vagy a megosztott tárolási rétegben üzemszünetek. A megosztott lemez ISCSI használata esetén optikai csatorna vagy megosztott VHDx kell végrehajtani.
- **SQL mindig a elérhetősége csoportok**: ezekkel a lépésekkel két csomópontokat is a megosztott semmi fürt az SQL Server adatbázisokkal konfigurált szinkron replikációs és automatikusan átveszi az elérhetőség csoportban a telepítés.

Vállalati kiadásában SQL Server is biztosít a natív katasztrófa helyreállítási technológiák visszaállítása az adatbázis egy távoli webhelyre. Ez a cikk azt fogja kihasználhatja és e natív SQL katasztrófa helyreállítási technológiák integrálása: 

- SQL mindig a elérhetősége csoportok a következőhöz: az SQL Server 2012 és 2014-es vállalati kiadásaihoz vészhelyreállítás.
- SQL-adatbázis tükrözés a magas biztonsági módot az SQL Server Standard edition (bármely verzió), illetve az SQL Server 2008 R2.


Webhely helyreállítási SQL Server védheti, mint a táblázatban.

 |**A helyszíni helyszíni** | **A helyszíni az Azure** 
---|---|---
**A Hyper-V** | igen | igen
**VMware** | igen | igen 
**Fizikai kiszolgáló** | igen | igen


## <a name="support-and-integration"></a>Támogatási és integrációja

Az SQL Server verzióit az jelenik meg ez a cikk a által támogatott:


- Az SQL Server 2014-es Enterprise és az szokásos
- Az SQL Server 2012 vállalati és a szokásos
- Az SQL Server 2008 R2 Enterprise és az szokásos


Webhely helyreállítási is integrálódik a natív SQL Server BCDR technológiák katasztrófa helyreállítási megoldást nyújtanak az alábbi táblázat foglal össze.

**A szolgáltatás** |**Részletek** | **Az SQL Server-verzióra** 
---|---|---
**AlwaysOn rendelkezésre állási csoport** | Az SQL Server különálló példányai egy Feladatátvevőfürt több csomópontok tartalmazó futnak.<br/><br/>Adatbázisok csoportosítható másolhatók (tükrözött) a SQL Server-példányok, hogy szükség van a nem megosztott tárolási feladatátvevő csoportokba.<br/><br/>Itt vészhelyreállítás elsődleges webhely és egy vagy több másodlagos webhelyek között. Két csomópontok beállíthatja a megosztott semmi sem az SQL Server-adatbázisokkal fürt konfigurált szinkron replikációs és automatikusan átveszi az elérhetőség csoportban. | Az SQL Server 2014-es és 2012 Enterprise edition
**Feladatátvétel használható (AlwaysOn FCI)** | Az SQL Server használja a Windows feladatátvétel a helyszíni SQL Server-munkaterhelésekből magas elérhetősége használható.<br/><br/>Az SQL Server példánya fut a megosztott lemez csomópontok egy Feladatátvevőfürt vannak beállítva. Ha egy példány nem működik a fürt nem sikerül fölé másikat.<br/><br/>A fürt hiba vagy a megosztott tárolási kimaradások vírusvédelmet nem. A megosztott iSCSI optikai csatornát, a végrehajtható vagy megosztott VHDXs. | Az SQL Server Enterprise kiadások<br/><br/>SQL Server Standard edition (csak két csomópontok korlátozott)
**Adatbázis-tükrözési (magas biztonsági mód)** | Egyetlen egy másodlagos példányt az adatbázis védi. Mindkét magas biztonsági (szinkron) a rendelkezésre álló és a nagy teljesítményű (aszinkron) replikációs módok. Egy Feladatátvevőfürt használatához nincs szükség. | Az SQL Server 2008 R2 rendszerben<br/><br/>Az SQL Server Enterprise kiadásainak
**Különálló SQL Server** | Az SQL Server és a adatbázist egy kiszolgálón (fizikai vagy virtuális) van közzétéve. Ha a kiszolgáló virtuális fürtözés Host magas elérhetősége használatos. Nincs vendég szintű magas elérhető. | Vállalati vagy Standard edition

## <a name="deployment-recommendations"></a>Telepítési javaslatok


Az alábbi táblázat összefoglalja az SQL Server BCDR technológiák integrálása webhely helyreállítási javaslatok.

**Verzió** |**Edition** | **Telepítési** | **A helyszíni helyszíni** | **Az Azure helyszíni** 
---|---|---|---|---
Az SQL Server 2014-es vagy 2012-ben | Vállalati | Feladatátvevő fürt példány | AlwaysOn rendelkezésre állási csoportok | AlwaysOn rendelkezésre állási csoportok
 | Vállalati | Az elérhetőség magas AlwaysOn rendelkezésre állási csoportok | AlwaysOn rendelkezésre állási csoportok | AlwaysOn rendelkezésre állási csoportok
 | Normál | Feladatátvevő fürt példány (FCI) | Webhely helyreállítási replikáció helyi tükrözése | Webhely helyreállítási replikáció helyi tükrözése
 | Vállalati vagy szabvány | Különálló | Webhely helyreállítási replikációs | Webhely helyreállítási replikációs
Az SQL Server 2008 R2 rendszerben | Vállalati vagy szabvány | Feladatátvevő fürt példány (FCI) | Webhely helyreállítási replikáció helyi tükrözése | Webhely helyreállítási replikáció helyi tükrözése
 | Vállalati vagy szabvány | Különálló | Webhely helyreállítási replikációs | Webhely helyreállítási replikációs
Az SQL Server (bármely verzió) | Vállalati vagy szabvány | Feladatátvevő fürt példány - DTC alkalmazás | Webhely helyreállítási replikációs | Nem támogatott


## <a name="deployment-prerequisites"></a>Telepítési előfeltételek

Mire van szükség, mielőtt indítása:

- Egy helyszíni SQL Server deployment támogatott SQL Server-verziót. Az SQL Server általában az Active Directory is szüksége.
- Telepíteni szeretné az alkalmazási példát előfeltételei. Előfeltételek minden telepítési cikkben találhatók. Ezek mutató hivatkozások találhatók, a [Webhely-helyreállítás](site-recovery-overview.md).
- Ha szeretné az Azure helyreállítás beállítása, kell az SQL Server virtuális gépeken futó győződjön meg róla, hogy kompatibilis Azure és webhely-helyreállítás [Azure virtuális gép készenléti értékelési](http://www.microsoft.com/download/details.aspx?id=40898) futtathatja.


## <a name="set-up-active-directory"></a>Az Active Directory beállítása

Az Active Directory, a másodlagos helyreállítási webhelyen az SQL Server működéshez kell. Néhány lehetőségek közül:

- **Kis vállalati**– Ha az alkalmazások és a helyszíni webhely tartományvezérlőnek kisszámú van, és azt szeretné, hogy a teljes webhely átadni, azt javasoljuk, használható webhely helyreállítási repication való replikáció a tartományvezérlőnek, a másodlagos adatközponthoz vagy Azure.

- **Közepes és nagy vállalati**– Ha alkalmazás sok van, az Active Directory erdő futtatja, és kívánt alkalmazást vagy a terhelést alapoktól sikertelen, azt javasoljuk, egy további tartományvezérlőnek a másodlagos adatközponthoz vagy Azure beállítása. Figyelje meg, hogy használata AlwaysOn rendelkezésre állási csoportok távoli webhelyre való azt javasoljuk, állítsa be a másodlagos webhelyen vagy a Azure, egy másik további tartományvezérlőnek helyreállított SQL Server-példányt az.

A megjelenő utasításokat a dokumentumban feltételezik, hogy a tartományvezérlőnek érhető el a másodlagos helyen. [További információ:](site-recovery-active-directory.md) az Active Directory, a webhely helyreállítási védelme.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>Védelem integrálása az SQL Server Always-On (helyszíni az Azure)


Webhely helyreállítási natív módon támogatja az SQL-AlwaysOn. Ha létrehozott egy SQL rendelkezésre állási csoport az Azure virtuális gép beállítása, a "Másodlagos", majd a webhely helyreállítási segítségével kezelheti az elérhetőség csoportok feladatátvételének együtt. 

>[AZURE.NOTE] Ezzel a funkcióval jelenleg előzetes verzióban, és elérhető a Hyper-V kiszolgálók az elsődleges adatközpontban kezelhetők a VMM felhőket, illetve VMware beállítási kezelik [Konfigurációs kiszolgálója](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Ez a lehetőség nem érhető el az új Azure portálon most jobbra.

#### <a name="prerequisites"></a>Előfeltételek

Az alábbiakban szükséges SQL AlwaysOn integrálása webhely helyreállítás:

- Egy helyszíni SQL Server, (önálló kiszolgáló vagy egy Feladatátvevőfürt).
- Egy vagy több Azure virtuális gépeken futó SQL Server telepítése
- Egy SQL rendelkezésre állási csoport beállítása között egy helyszíni SQL Server és az SQL Server Azure-ban futó
- A PowerShell távelérési engedélyezni kell a helyszíni SQL Server-gépen. A VMM kiszolgáló vagy a fiókkonfigurációs kiszolgáló láthatja, hogy az SQL Server távoli PowerShell-hívások.
- Egy felhasználói fiókot is hozzá kell a helyszíni SQL Server az SQL-felhasználói csoportok legalább ezt engedélyekkel:
    - ALTER elérhetősége: csoportengedélyek [Itt](https://msdn.microsoft.com/library/hh231018.aspx), és [Itt](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER adatbázis - engedélyek[alábbi](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- Egy RunAs fiókot VMM kiszolgálón kell létrehozni vagy -fiókot kell létrehozni a CSPSConfigtool.exe használata a felhasználó az előző lépésben említett konfigurációs-kiszolgálón 
- Az SQL-PS modul telepítése után, és Azure virtuális gépeken futó helyszíni SQL-kiszolgálókon
- A virtuális Agent telepített virtuális gépeken futó Azure futó kell lennie.
- NTAUTHORITY\System van, hogy az Azure virtuális gépeken futó SQL Server vonatkozó engedélyek követően:
    - ALTER rendelkezésre ÁLLÁSI csoport - engedélyek [Itt](https://msdn.microsoft.com/library/hh231018.aspx), és [Itt](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER adatbázis - engedélyek [alábbi](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Lépés: 1: Adja hozzá az SQL Server


1. Kattintson a **SQL hozzáadása** egy új SQL Server hozzáadása gombra. 

    ![SQL hozzáadása](./media/site-recovery-sql/add-sql.png)

2. **SQL-beállítások megadása**a > **nevét** adja meg egy rövid nevet, olvassa el az SQL Server.
3. **Az SQL Server (FQDN)** adja meg a forrást, amely a hozzáadni kívánt SQL Server teljes Tartománynevét. Abban az esetben, ha az SQL Server egy Feladatátvevőfürt van telepítve, majd adja meg a fürt, nem pedig bármelyik fürt csomópontok FQDN.  
4. Az **SQL Server-példányt** válassza az alapértelmezett példány, vagy adja meg az egyéni példányának nevét.
5. A **Management Server** VMM kiszolgáló kiválasztása vagy konfigurációs kiszolgálója regisztrált a webhely helyreállítási tárolóból elemre. Webhely helyreállítási kommunikálni az SQL Server e Management server használja.
6. **Fiókként fusson** nyújtanak egy RunAs fiókot, amely a megadott VMM kiszolgálón vagy a fiókot, amely a Configuraaaon-kiszolgáló neve. Ehhez a fiókhoz szeretne elérni az SQL Server használja, és olvasási és feladatátvevő engedéllyel kell rendelkezésre állási csoportok, az SQL Server-gépen.

    ![SQL párbeszédpanel hozzáadása](./media/site-recovery-sql/add-sql-dialog.png)

Az SQL Server felvétele után meg kíván jeleníteni a **SQL-kiszolgálók** fülre. 

![Az SQL Server-lista](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Lépés: 2: SQL elérhetősége csoport hozzáadása

1. Az SQL Server gépi hozzáadása után a következő lépésben az elérhetőség csoportok hozzáadása webhely helyreállítási. Megtennie, az előző lépésben hozzáadott SQL Server belül Lehatolás, majd kattintson az SQL-elérhetősége csoport hozzáadása. 

    ![SQL-AG hozzáadása](./media/site-recovery-sql/add-sqlag.png)

2. SQL-rendelkezésre állási csoport is lehet esetében, amelyek egy vagy több virtuális gépeken futó Azure-ban. Amikor hozzáadása az sql rendelkezésre állási csoport, meg kell adnia a nevét, és az előfizetés az Azure virtuális gép, amelyre át kell venni a webhely helyreállítási által az elérhetőség csoport.

    ![SQL-AG párbeszédpanel hozzáadása](./media/site-recovery-sql/add-sqlag-dialog.png)

3. A fenti példában elérhetősége csoport D1-AG válna elsődleges virtuális gépen SQLAGVM2 futó feladatátvevő előfizetés DevTesting2 belül. 

>[AZURE.NOTE] Csak az elsődleges, a fenti lépésben hozzáadott SQL Server vannak elérhetősége csoportokat a felvenni kívánt hely helyreállítási érhetők el. Ha az SQL Server-elérhetősége csoport elsődleges saját, vagy ha hozzáadta további rendelkezésre állási csoportok az SQL Server Miután felvette, a frissítés beállítás használatával az SQL Server frissítése.

#### <a name="step-3-create-a-recovery-plan"></a>3 lépés: A helyreállítási terv létrehozása

A következő lépésként virtuális gépeken futó és a csoportok elérhetősége helyreállítási terv létrehozása. Jelölje ki a azonos VMM kiszolgáló vagy fiókkonfigurációs kiszolgáló, mellyel az 1 forrás és a Microsoft Azure célként.

![Helyreállítási terv létrehozása](./media/site-recovery-sql/create-rp1.png)

![Helyreállítási terv létrehozása](./media/site-recovery-sql/create-rp2.png)

A példában a 3 virtuális gépeken futó SQL elérhetősége csoport használata a kódmentes, amelyek a Sharepoint-alkalmazás áll. A helyreállítási tervet, azt is mindkét az elérhetőség csoport kijelölése, valamint a virtuális gép, amely jelent az alkalmazást. 

További testre szabható a helyreállítási terv virtuális gépeken futó áthelyezése másik feladatátvevő csoportok feladatátvevő sorrendjének sorozat. Rendelkezésre állási csoport mindig sikertelen első fölé, mint bármely alkalmazás egy kódmentes volna használt. 

![Helyreállítási terv testreszabása](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Lépés: 4: Átveszi

Különböző feladatátvevő lehetőségek állnak rendelkezésre, miután egy rendelkezésre állási csoport lett hozzáadva helyreállítási terv.

Feladatátvevő | Részletek
--- | ---
**Tervezett feladatátvevő** | Tervezett feladatátvevő azt jelenti, hogy nincs adatok elvesztése feladatátvevő. SQL rendelkezésre állási csoport elérhetősége mód először szinkron értékre van állítva, és ezután megjelenítheti a virtuális gép közben az elérhetőség csoport hozzáadása webhely helyreállítási megadott elérhetősége csoport elsődleges feladatátvevő induljanak elérése. A feladatátvételi elkészülte elérhetősége mód értékre van állítva a azonos előtti a tervezett feladatátvételt indított volt.
**Tervezett feladatátvevő** | Tervezett feladatátvevő okozhat, az adatok elvesztését. Tervezett feladatátvevő kiváltó közben az elérhetőség mód az elérhetőség csoport nem változik, és az informatikai elsődleges megjelenítheti a virtuális gép közben az elérhetőség csoport hozzáadása webhely helyreállítási megadott lett. Miután nem tervezett feladatátvételre befejeződött, és a helyszíni SQL Servert futtató áll rendelkezésre a kiszolgáló újra, fordított replikációs az elérhetőség csoportban kezdeményezése tartalmaz. Ne feledje, hogy ez a művelet nem érhető el a helyreállítási tervhez SQL rendelkezésre állási csoport SQL-kiszolgálók lapján kell venni
**Teszt feladatátvevő** | SQL rendelkezésre állási csoport próba feladatátvétele nem támogatott. Próba feladatátvételét helyreállítási terv tartalmazó SQL rendelkezésre állási csoport elindítása meg, ha feladatátvevő volna kihagyja rendelkezésre állási csoport.


Fontolja meg feladatátvevő beállításokkal.

A beállítás | Részletek
--- | ---
**A beállítás 1** | 1. végezze el az alkalmazás és az előtér-meghatározási próba feladatátvevő.<br/><br/>2. a alkalmazás réteg írásvédett módban a replika másolatot eléréséhez frissítése, és végezze el az alkalmazás csak olvasható vizsgálata.
**A beállítás 2** | 1. a replikált SQL Server virtuális gép példány (vagy használja az VMM adatfeliratsor-webhelyre való Azure biztonsági másolat) másolatának létrehozása, és jelenítse meg a hálózat tesztelése<br/><br/> 2. végezze el a próba feladatátvételi a helyreállítási csomagot használ.

Lépés az 5: Visszavétele

Ha azt szeretné, hogy az elérhetőség csoport újra elsődleges a helyszíni SQL Server majd ezt teheti a helyreállítási terv tervezett feladatátvevő el, és válassza a irányának a Microsoft Azure helyszíni VMM kiszolgálóra.

>[AZURE.NOTE] Utána feladatátvételhez fordított replikációs rendelkezik, az elérhetőség csoportban koppintva folytathatja a replikáció kezdeményezése. A replikáció marad felfüggesztett, amíg ez történik.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Egy VMM Server vagy konfigurációs nélkül gépek védelme

A környezetben, amely nem egy VMM Server vagy konfigurációs felügyeli a Azure automatizálási Runbooks SQL elérhetősége csoportok parancsfájl feladatátvevő konfigurálása is használható. Állítsa be, hogy a lépései a következők:

1.  Hozzon létre egy rendelkezésre állási csoport átadni a parancsfájl helyi fájl. A mintaparancsfájl elérési útja az elérhetőség csoportban az Azure replikakészlettagon, és szeretné, hogy replikált példány átvétele. Ezt fogja a parancsfájlt az SQL Server replika virtuális gép megkerülhetők van az egyéni parancsfájl kiterjesztéssel.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Töltse fel a parancsfájl blob Azure tárterület-fiókjában. Ez a példa használja:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Hozzon létre egy Azure automatizálási runbook meghívni a parancsfájlok Azure SQL Server replika virtuális gépen. Ehhez használja a mintaparancsfájl. [Tudjon meg többet](site-recovery-runbook-automation.md) a helyreállítási csomagok automatizálási runbooks használatáról. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Létrehozásakor a helyreállítási terv az alkalmazáshoz "előre csoport 1 indítási" parancsfájl lépés hozzáadása, amely elindítja az automatizálási runbook rendelkezésre állási csoportok átadni.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>Védelem integrálása SQL AlwaysOn (helyszíni a helyszíni)

Ha az SQL Server az elérhetőség csoportok magas elérhetőség, vagy egy feladatátvevő fürt példány, azt javasoljuk, elérhetősége csoportokon keresztül, valamint a helyreállítási webhelyen. Ne feledje, hogy az útmutató elosztott tranzakciók nem használó alkalmazások számára.

1. [Konfigurálása adatbázisok](https://msdn.microsoft.com/library/hh213078.aspx) elérhetősége csoportokba.
2. Hozzon létre egy új virtuális hálózati másodlagos webhelyen.
3. Állítsa be az új virtuális hálózati és az elsődleges hely között a webhely VPN.
4. Hozzon létre egy virtuális gép a helyreállítási webhelyen, és az SQL Server telepítése.
5. Az új SQL Server virtuális géphez meglévő AlwaysOn rendelkezésre állási csoportok bővítése. Állítsa be a SQL Server-példányt egy aszinkron replika másolatként.
6. Az elérhetőség csoport figyelő létrehozása és frissítése a meglévő figyelő, ha meg szeretné jeleníteni a aszinkron replika virtuális gép.
7. Győződjön meg róla, hogy az alkalmazás farm beállítása a figyelő használatával. Ha a telepítő használatával az adatbázis-kiszolgáló neve, frissítse a figyelő használatához, így nem kell újrakonfigurálni az a feladatátvétel után.

Az elosztott tranzakciók használó alkalmazások azt ajánlási [Webhely helyreállítási SAN ismétlésekkel](site-recovery-vmm-san.md) vagy [VMWare/fizikai kiszolgáló webhelyre való replikáció](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Helyreállítási terv kapcsolatos szempontok

1. A mintaparancsfájl felvétele az elsődleges és másodlagos webhelyek VMM tárban.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Létrehozásakor a helyreállítási terv az alkalmazáshoz "előre csoport 1 indítási" parancsfájl lépés hozzáadása, amely elindítja az elérhetőség csoportok átadni a parancsfájl.



## <a name="protect-a-standalone-sql-server"></a>Az SQL Server önálló védelme

Ebben a konfigurációban javasoljuk, hogy a webhely helyreállítási replikációs használatával az SQL Server számítógép védelme. A pontos eljárás attól fogja, hogy az SQL Server be van állítva egy virtuális gép vagy a fizikai kiszolgálóhoz, és e Azure való replikáció kívánt vagy egy másodlagos helyszíni webhely. A [Webhely helyreállítás – áttekintés](site-recovery-overview.md)megnyithatja összes telepítési forgatókönyvek utasításokat.


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Az SQL Server fürt (normál vagy 2008 R2) védelme

SQL Server Standard edition vagy SQL Server 2008 R2 fürt ajánlott védelme az SQL Server webhely helyreállítási replikáció használata.

### <a name="on-premises-to-on-premises"></a>A helyszíni helyszíni

- Ha az alkalmazás elosztott tranzakciók javasoljuk [Webhely helyreállítási SAN ismétlésekkel](site-recovery-vmm-san.md) a Hyper-V környezet és a [VMware VMware/fizikai kiszolgáló](site-recovery-vmware-to-vmware.md) VMware környezetben üzembe.

- Nem DTC alkalmazások a fenti megközelítést szeretné helyreállítani a fürt önálló kiszolgáló által egy helyi magas biztonsági DB Tükörmargók vizsgál kihasználhatja.

### <a name="on-premises-to-azure"></a>A helyszíni az Azure

Webhely helyreállítás nem támogatja a vendégként való bekapcsolódáshoz fürt támogatási, amikor esetében, amelyek Azure. Az SQL Server is nem nyújt alacsony költség katasztrófa helyreállítási megoldást Standard edition. Javasoljuk, hogy a helyszíni SQL Server fürt az SQL Server önálló védelme és a visszanyerésében Azure-ban.


1. Állítsa be a további önálló SQL Server-példány a helyszíni webhelyen.
2. Állítsa be a tükrözése az adatbázisok védelmet igénylő szolgáló példány. Állítsa be a magas biztonsági módban tükrözése.
3.  Állítsa be a webhely helyreállítási a helyszíni webhelyen alapján a környezet ([A Hyper-V](site-recovery-hyper-v-site-to-azure.md) vagy [VMware/fizikai kiszolgáló](site-recovery-vmware-to-azure-classic.md).
4.  Webhely helyreállítási replikációs segítségével Azure való replikáció a az új SQL Server-példányt. Tükörmargók magas biztonsági másolatot, és így akkor fogja szinkronizálja az az elsődleges fürt, de a webhely helyreállítási replikációs szolgáltatásával Azure fogja replikált.

Az alábbi ábra bemutatja ezt a beállítást.

![Szabványos fürthöz](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Visszaállás kapcsolatos szempontok

SQL szabványos fürtre, vonatkozóan visszaállás feladatátvételhez után SQL biztonsági megkövetelése, és az eredeti fürt és újbóli létrehozásával a tükrözése a Tükörmargók példányból visszaállítása.

## <a name="next-steps"></a>Következő lépések
[Tudjon meg többet](site-recovery-best-practices.md) is megtudhat a Felkészülés a webhely helyreállítási üzembe.










 