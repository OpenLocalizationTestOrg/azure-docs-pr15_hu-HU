<properties 
    pageTitle="Hozzon létre egy virtuális natív mód jelentés-kiszolgálóval a PowerShell használatával |} Microsoft Azure"
    description="Ez a témakör ismerteti, és bemutatja a telepítési és az SQL Server Reporting Services natív mód jelentéskészítő kiszolgálót az Azure virtuális gép konfigurálása. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Hozzon létre egy Azure virtuális natív mód jelentés-kiszolgálóval a PowerShell használatával

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Ez a témakör ismerteti, és bemutatja a telepítési és az SQL Server Reporting Services natív mód jelentéskészítő kiszolgálót az Azure virtuális gép konfigurálása. A lépéseket a dokumentumban a hozhat létre a virtuális gép és a Windows PowerShell-parancsprogramot Reporting Services konfigurálása a virtuális a manuális lépésekkel kombinációját használja. A konfiguráció parancsfájl is tartalmaz, a tűzfal port megnyitása a HTTP-és HTTPs.

>[AZURE.NOTE] Ha nem szükséges **HTTPS** **lépés: 2 ugorja át**, a jelentéskészítő kiszolgálót.
>
>Miután létrehozta a virtuális az 1, ugorjon a jelentéskészítő kiszolgálót és a HTTP konfigurálása parancsfájl használata. Futtatása után a parancsfájlt a jelentéskészítő kiszolgálót használatára.

## <a name="prerequisites-and-assumptions"></a>Előfeltételek és feltételezések

- **Azure előfizetés**: Ellenőrizze az Azure-előfizetésben elérhető magmintákat számát. A javasolt virtuális méret **a3**hoz létre, **4** elérhető magmintákat van szükség. Ha egy virtuális méretét **az A2**, **2** elérhető magmintákat szüksége.
    
    - Ellenőrizze az alapvető határértékén előfizetését, az Azure klasszikus portálon kattintson a beállítások a bal oldali ablaktáblában, majd kattintson a HASZNÁLATÁT a felső menüben.
    
    - Ha növelni szeretné a core kvóta, a [Azure](https://azure.microsoft.com/support/options/)ügyfélszolgálatát. Virtuális méret információt című témakörben talál [Az Azure virtuális gép méretét](virtual-machines-linux-sizes.md).

- **A Windows PowerShell-parancsfájlokat**: A témakör azt feltételezi, hogy rendelkezik-e a munka alapismeretek a Windows PowerShell. A Windows PowerShell használatával kapcsolatos további tudnivalókért lásd: a következőket:

    - [A Windows PowerShell indítása a Windows Server rendszeren](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [A Windows PowerShell használatába](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Lépés: 1: Az Azure virtuális gép kiépítése

1. Keresse meg a Azure klasszikus portált.

1. A bal oldali ablaktáblában kattintson a **virtuális gépeken futó** .

    ![a Microsoft azure virtuális gépeken futó](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Kattintson az **Új**gombra.

    ![az új gomb](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Kattintson a **gyűjteményből**.

    ![új virtuális gyűjteményből](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Kattintson **Az SQL Server 2014-es RTM Standard – a Windows Server 2012 R2** , és kattintson a nyílra kattintva továbbra is.

    ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Ha a Reporting Services adatok előfizetések szolgáltatás alapú van szüksége, válassza az **SQL Server 2014-es RTM Enterprise – a Windows Server 2012 R2**. Az SQL Server kiadásainak és a támogatott funkciókról további tudnivalókért lásd: [Az SQL Server 2012 kiadásai által támogatott funkciók](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. A **virtuális gép beállítása** lapon az alábbi mezők szerkesztése:
                                    
    - Ha egynél több **verzióját kiadás dátuma**, jelölje be a legújabb verzióra.
    
    - **Virtuális számítógépnév**: A számítógépnek a neve a következő lapon konfigurációs üzenetként is az alapértelmezett felhőalapú szolgáltatás DNS nevet. A tartománynév egyedinek kell lennie az Azure szolgáltatás között. Fontolja meg, hogy a virtuális, amelyek ismerteti, hogy mire használható a virtuális számítógép neve. Ha például ssrsnativecloud.
    
    - **Réteg**: szabványos
    
    - **Méret: A3** az SQL Server-munkaterhelésekből ajánlott virtuális mérete. Ha egy virtuális csak egy jelentéskészítő kiszolgálót, mint egy virtuális A2 mérete, amely elegendő kivéve, ha a jelentéskészítő kiszolgálót egy nagy terhelés találkozik. A virtuális árinformációkat olvassa el a [Virtuális gépeken futó árak](https://azure.microsoft.com/pricing/details/virtual-machines/)című témakört.
    
    - **Új felhasználónév**: a nevet ad a virtuális rendszergazdaként jön létre.
    
    - **Új jelszót** , és **erősítse meg**. A jelszót az új rendszergazdai fiókot használja, és erős jelszavak használata ajánlott.
    
    - Kattintson a **Tovább**gombra. ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. A következő lapon az alábbi mezők szerkesztése:

    - **Felhőbeli szolgáltatástól**: jelölje be az **Új felhőalapú szolgáltatás létrehozása**.
    
    - **Felhőalapú szolgáltatás DNS neve**: a nyilvános DNS neve a társított a virtuális felhőalapú szolgáltatás. Az alapértelmezett neve a virtuális jelölőnégyzetét beírt nevét. A témakör későbbi lépéseket, ha hoz létre egy megbízható hitelesítésszolgáltató tanúsítványát, és kattintson a DNS-név értékét a "**tulajdonos**" a tanúsítvány használható.
    
    - **Régió/affinitás csoport/virtuális hálózati**: a végfelhasználók legközelebb régiójának.
    
    - **Tárterület-fiók**: automatikusan generált tárterület-fiókot használ.
    
    - **Elérhetőség beállítása**: nincs.
    
    - **VÉGPONTOK** A **Távoli asztali** és a **PowerShell** végpontok megőrzése, és adja hozzá a HTTP vagy HTTPS-végpont a környezettől függően.

        - **HTTP**: az alapértelmezett nyilvános és titkos legyen **80**. Megjegyzendő, hogy egy privát port eltérő 80, ha módosítani **$HTTPport = 80** a http parancsfájl.

        - **HTTPS**: az alapértelmezett nyilvános és titkos legyen **443-as**. Biztonsági okokból, hogy a személyes port módosítása, és állítsa be a tűzfalat, és a személyes portot használja a jelentéskészítő kiszolgálót. További információt a végpontok megtudhatja, [hogy miként állítsa be való kommunikáció virtuális géphez](virtual-machines-windows-classic-setup-endpoints.md). Megjegyzés: Ha eltérő 443-as port változó a paraméter **$HTTPsport 443-as =** a HTTPS parancsfájl.
    
    - Kattintson a Tovább gombra. ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. A varázsló utolsó lapján, **a virtuális ügynököt a** kijelölt alapértelmezett megtartani. Ez a témakör lépéseit nem használja a virtuális ügynök, de ha a virtuális tartani, a virtuális ügynök és bővítmények lehetővé teszi, hogy kényelmesebb ő CM.  További információt a virtuális Agent olvassa el a [virtuális ügynök és bővítmények – rész 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/)című témakört. Az alapértelmezett telepített bővítmények ad operációs rendszert futtató egyik a "BGINFO" bővítmény, amely a virtuális asztali, a rendszer-információkat, például a belső IP- és a szabad lemezterület jeleníti meg.

1. Kattintson a Kész gombra. ![oké](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. A virtuális **állapota** a rendelkezést során jeleníti meg **(létesítése) kezdődik** , és majd jeleníti meg **fut** , amikor a virtuális kiépített használatára.

## <a name="step-2-create-a-server-certificate"></a>Lépés: 2: Kiszolgálói tanúsítvány létrehozása

>[AZURE.NOTE] Ha HTTPS, a jelentéskészítő kiszolgálón nincs szüksége, akkor **lépés: 2 átugrása** , és válassza a szakasz **a jelentéskészítő kiszolgálót és a HTTP konfigurálása parancsfájl használata**. A HTTP parancsfájl használatával gyorsan konfigurálása a jelentéskészítő kiszolgálót, és a jelentéskészítő kiszolgálót használatára.

A virtuális HTTPS használatához egy megbízható hitelesítésszolgáltató tanúsítványát van szüksége. Az esete is használhatja az alábbi két módszer közül:

- Érvényes SSL-tanúsítvány kiadott által hitelesítésszolgáltató (CA), és a Microsoft által megbízhatóként. A legfelső szintű hitelesítésszolgáltató tanúsítványok szükség lehet a Microsoft Root Certificate Program kiosztott. Ezzel a programmal kapcsolatos további tudnivalókért olvassa el a [Windows és a Windows Phone 8 SSL Root Certificate Program (tag CA)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) és [a Microsoft Root Certificate Program – bevezetés](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx)című témakört.

- Önaláírt tanúsítvány. Önaláírt tanúsítványok gyártási környezetekben nem ajánlott.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>A tanúsítvány létrehozott egy megbízható hitelesítő szervezete (CA) használata

1. **A webhely hitelesítésszolgáltatótól kiszolgálói tanúsítvány kérése**. 

    A kiszolgálói tanúsítvány varázsló kérelem Tanúsítványfájl (Certreq.txt) hitelesítésszolgáltató küldendő létrehozásához vagy egy online hitelesítésszolgáltató kér létrehozásához használható. Ha például Microsoft tanúsítvány szolgáltatások Windows Server 2012. Attól függően, hogy a kiszolgálói tanúsítvány által kínált azonosító garancia szintjét érdemes több napig hitelesítésszolgáltató a kérelem jóváhagyása és a tanúsítvány fájl küldése több hónapra. 

    A kiszolgáló igénylése további információt talál az alábbi: 
    
    - [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx)használja.
    
    - Biztonsági eszközök felügyelete a Windows Server 2012.

    [A Windows Server 2012 felügyeletéhez biztonsági eszközök](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] A megbízható SSL-tanúsítvány **kiadott** területén kell az új virtuális használt **Felhőalapú szolgáltatás DNS neve** megegyezik.

1. **A kiszolgálói tanúsítvány, a webkiszolgáló telepítése**. Az érintett webkiszolgálóra ebben az esetben a virtuális, amelyen a jelentéskészítő kiszolgálót és a webhely későbbi lépéseiben jön létre, ha konfigurálja a Reporting Services. A kiszolgálói tanúsítvány telepítése az érintett webkiszolgálóra a tanúsítvány beépülő modult használatával kapcsolatos további tudnivalókért lásd: [kiszolgálói tanúsítvány telepítése](https://technet.microsoft.com/library/cc740068).
    
    Ha ez a témakör a beépített parancsfájl, konfigurálása a jelentéskészítő kiszolgálót, a tanúsítványok **ujjlenyomat** értéke a parancsprogram paraméterként szükséges. Nézze meg a következő szakaszban további információt az ujjlenyomatot a tanúsítvány beszerzése.

1. A kiszolgálói tanúsítvány rendelhet a jelentéskészítő kiszolgálót. A hozzárendelés befejeződött a következő szakaszban, a jelentéskészítő kiszolgálót beállításakor.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>A virtuális gépeken futó önaláírt tanúsítványt használja

Önaláírt tanúsítvány jött létre a virtuális, amikor a virtuális kiépített. A tanúsítvány neve megegyezik a virtuális tartománynév tartalmaz. Tanúsítvány hibák elkerülése érdekében, hogy szükség rá, hogy megbízható-e a virtuális magát a és a webhely összes felhasználója-e a tanúsítvány.

1. A tanúsítvány, kattintson a helyi virtuális a legfelső szintű hitelesítésszolgáltató megbízható, vegye fel a tanúsítvány a **Megbízható legfelső szintű hitelesítésszolgáltatók**. Az alábbiakban megtalálja a szükséges lépéseket összefoglalását. A lépések részletes útmutatást a megbízható hitelesítésszolgáltató olvassa el a [kiszolgálói tanúsítvány telepítése](https://technet.microsoft.com/library/cc740068)című témakört.

    1. Az Azure klasszikus portálról jelölje ki a virtuális, és kattintson a Csatlakozás gombra. A böngésző beállításaitól függően a kéri a virtuális csatlakozhat .rdp fájlt menteni.
    
        ![azure virtuális gép csatlakoztatása](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) A felhasználó virtuális nevét, a felhasználónév és a úgy állította be, amikor a virtuális létrehozott jelszót használja. 
    
        Ha például az alábbi képen a virtuális nevét **ssrsnativecloud** pedig a felhasználó nevét **tesztfelhasználó nevet**.
        
        ![bejelentkezési tartalmaz virtuális neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Futtassa a mmc.exe. További tudnivalókért lásd: [Útmutató: a beépülő modult a tanúsítványok megtekintése](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. Konzol alkalmazás **fájl** menüben adja hozzá a **tanúsítványok** beépülő modult, **Számítógépfiók** megjelenésekor válassza ki, és kattintson a **Tovább gombra**.
    
    1. Válassza a **Helyi számítógép** kezelése, és kattintson a **Befejezés gombra**.
    
    1. Kattintson az **OK gombra** , és bontsa ki a **a tanúsítványok – személyes** csomópontok majd kattintson a **tanúsítványok**gombra. A tanúsítvány neve után a virtuális DNS-nevét, és **cloudapp.net**végződik. Kattintson a jobb gombbal a tanúsítvány nevét, és kattintson a **Másolás**parancsra.
    
    1. Bontsa ki a **Megbízható legfelső szintű hitelesítésszolgáltatók** csomópontot, majd kattintson a jobb gombbal a **tanúsítványok** , és válassza a **Beillesztés parancsot**.
    
    1. Az érvényesítés, kattintson duplán a **Megbízható legfelső szintű hitelesítésszolgáltatók** csoportban a tanúsítvány nevét, és győződjön meg arról, hogy nincsenek hibák, és megjelenik a tanúsítvány. Ha szeretne a HTTPS-parancsprogramot, ez a témakör a beépített, konfigurálása a jelentéskészítő kiszolgálót, a tanúsítványok **ujjlenyomat** értéke parancsfájl paraméterként szükséges. **A ujjlenyomat érték**, adja meg a következőket. Mintát is a PowerShell [használata parancsfájl, a jelentéskészítő kiszolgálót és HTTPS konfigurálása](#use-script-to-configure-the-report-server-and-HTTPS)szakaszban az ujjlenyomatot beolvasásához.
        
        1. Kattintson duplán a tanúsítvány, például ssrsnativecloud.cloudapp.net nevére.
        
        1. Kattintson a **Részletek** fülre.
        
        1. Kattintson a **ujjlenyomat**. A Részletek mezőben, például a6 jelenik meg az ujjlenyomatot értékének 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2-c fb 2f.
        
        1. Másolja az ujjlenyomatot és az érték menthet későbbre, vagy a parancsfájl szerkesztése most választógombot.
        
        1. (*) A parancsprogram futtatása előtt távolítsa el az értékpárok közötti térközök. Például az ujjlenyomatot előtt jegyezni most lenne a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. A kiszolgálói tanúsítvány rendelhet a jelentéskészítő kiszolgálót. A hozzárendelés befejeződött a következő szakaszban, a jelentéskészítő kiszolgálót beállításakor.

Önaláírt SSL-tanúsítványt használja, ha a tanúsítvány neve már megegyezik a virtuális gép hostname (állomásnév). Ezért a DNS-Rekordokat a gép már bejegyezte globálisan, és bármelyik ügyfélprogramból is elérhető.

## <a name="step-3-configure-the-report-server"></a>3 lépés: A jelentéskészítő kiszolgálót konfigurálása

Ez a szakasz végigvezeti a virtuális beállítása a Reporting Services natív mód jelentéskészítő kiszolgálót. A jelentéskészítő kiszolgálót konfigurálása használhatja az alábbi lehetőségek közül:

- A jelentéskészítő kiszolgálót konfigurálása a parancsfájl használatával

- Konfigurációkezelő használatával állítsa be a jelentéskészítő kiszolgálót.

A részletes lépéseket című [a virtuális gép csatlakozzon, és indítsa el a Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Hitelesítési Megjegyzés:** Windows-hitelesítés az ajánlott hitelesítési módszer áll és az alapértelmezett Reporting Services-hitelesítést. Csak azok a felhasználók a virtuális konfigurált is hozzáférhetnek a Reporting Services, és a Reporting Services szerepkörökhöz rendelt.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Állítsa be a jelentéskészítő kiszolgálót és a HTTP parancsfájl használatával

A Windows PowerShell-parancsprogramot használatával állítsa be a jelentéskészítő kiszolgálót, hajtsa végre az alábbi lépéseket. A konfiguráció HTTP, nem HTTPS tartalmazza:

1. Az Azure klasszikus portálról jelölje ki a virtuális, és kattintson a Csatlakozás gombra. A böngésző beállításaitól függően a kéri a virtuális csatlakozhat .rdp fájlt menteni.

    ![azure virtuális gép csatlakoztatása](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) A felhasználó virtuális nevét, a felhasználónév és a úgy állította be, amikor a virtuális létrehozott jelszót használja. 

    Ha például az alábbi képen a virtuális nevét **ssrsnativecloud** pedig a felhasználó nevét **tesztfelhasználó nevet**.
    
    ![bejelentkezési tartalmaz virtuális neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. A virtuális nyissa meg **A Windows PowerShell ISE** rendszergazdai jogosultságokkal rendelkező. A PowerShell ISE alapértelmezés szerint a Windows server 2012 van telepítve. Használja a ISE egy szabványos a Windows PowerShell-ablak helyett, hogy a parancsprogram beillesztése a ISE, módosítsa a parancsfájlt, és futtassa a parancsfájl ajánlott.

1. A Windows PowerShell ISE kattintson a **Nézet** menü, és kattintson a **Parancsprogram-ablaktábla megjelenítése**gombra.

1. Másolja a következő parancsokat, és a parancsprogram beillesztése a Windows PowerShell ISE parancsfájl ablakban.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Ha a a virtuális egy HTTP-eltérő 80-as port hozta létre, módosítsa a paramétert $HTTPport = 80.

1. A Reporting Services a parancsfájl van beállítva. Ha szeretne futtassa a Reporting Services, módosítsa a elérési útját a "v11", a Get-WmiObject kimutatásban névtér verziója részét.

1. Futtassa a.

**Érvényesítés**: Ellenőrizze, hogy működik-e a egyszerű jelentés kiszolgáló működését, olvassa el a [konfigurációjának ellenőrzése](#verify-the-configuration) részben a témakör későbbi.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Állítsa be a jelentéskészítő kiszolgálót és HTTPS parancsfájl használatával

A Windows PowerShell használatával állítsa be a jelentéskészítő kiszolgálót, hajtsa végre az alábbi lépéseket. A konfiguráció HTTPS, nem HTTP tartalmazza.

1. Az Azure klasszikus portálról jelölje ki a virtuális, és kattintson a Csatlakozás gombra. A böngésző beállításaitól függően a kéri a virtuális csatlakozhat .rdp fájlt menteni.

    ![azure virtuális gép csatlakoztatása](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) A felhasználó virtuális nevét, a felhasználónév és a úgy állította be, amikor a virtuális létrehozott jelszót használja. 

    Ha például az alábbi képen a virtuális nevét **ssrsnativecloud** pedig a felhasználó nevét **tesztfelhasználó nevet**.

    ![bejelentkezési tartalmaz virtuális neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. A virtuális nyissa meg **A Windows PowerShell ISE** rendszergazdai jogosultságokkal rendelkező. A PowerShell ISE alapértelmezés szerint a Windows server 2012 van telepítve. Használja a ISE egy szabványos a Windows PowerShell-ablak helyett, hogy a parancsprogram beillesztése a ISE, módosítsa a parancsfájlt, és futtassa a parancsfájl ajánlott.

1. Ahhoz, hogy a parancsfájlok futtatását, futtassa az alábbi Windows PowerShell-parancsot:

        Set-ExecutionPolicy RemoteSigned

    Futtassa az alábbi módon a házirend ellenőrzése:

        Get-ExecutionPolicy

1. A **Windows PowerShell ISE**kattintson a **Nézet** menü, és kattintson a **Parancsprogram-ablaktábla megjelenítése**gombra.

1. Másolja a következő parancsokat, és illessze be a Windows PowerShell ISE parancsfájl ablakban.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Módosítsa a **$certificatehash** paramétert a parancsfájl:

    - Ez a paraméter **szükséges** . Ha a fenti lépésekkel nem mentette a tanúsítvány értéket, használja az alábbi lehetőségek közül kivonat értékének másolása a tanúsítványok ujjlenyomat.:

        A virtuális, nyissa meg a Windows PowerShell ISE, és futtassa az alábbi parancsot:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        A kimenet az alábbihoz hasonlóan fog kinézni. Ha a parancsfájl ad vissza egy üres sort, a virtuális nem van beállítva, például tanúsítvány, [virtuális gépeken futó önaláírt tanúsítványt használja,](#to-use-the-virtual-machines-self-signed-certificate)olvassa el.
    
    VAGY
    
    - A virtuális mmc.exe futtatható, és adja hozzá a **tanúsítványok** beépülő modult.
    
    - A **Megbízható legfelső szintű hitelesítésszolgáltatók** csomópont csoportban kattintson duplán a tanúsítvány neve. A virtuális gép az önaláírt tanúsítványt használja, ha a tanúsítvány munkafüzetéről a virtuális DNS nevét, és **cloudapp.net**végződik.
    
    - Kattintson a **Részletek** fülre.
    
    - Kattintson a **ujjlenyomat**. A Részletek mezőben, például af 11 60 b6 4 28 8 d 89 0a jelenik meg az ujjlenyomatot értékének 82 12 bb 6b a9 c3 66 4f 31 90 48
    
    - **A parancsprogram futtatása előtt**, távolítsa el az értékpárok közötti térközök. Ha például af1160b64b288d890a8212ff6ba9c3664f319048

1. Módosítsa a **$httpsport** paraméter: 

    - Ha korábban 443-as port a HTTPS-végpont, majd szükségtelen ezt a paramétert a parancsfájl frissítéséhez. A port értéket, a személyes HTTPS-végpont a virtuális konfigurált kijelölt egyébként használja.

1. Módosítsa a **$DNSName** paraméter: 

    - A parancsprogram be van állítva egy helyettesítő karaktert tanúsítvány $DNSName az = "+". Ha nem tesz helyettesítő tanúsítvány kötést beállítása, Megjegyzés $DNSName ki nem kívánt ="+"és a következő sort, a teljes $DNSNAme hivatkozás, ## $DNSName="$server.cloudapp.net engedélyezése".

        Ha nem szeretné, hogy a virtuális gép DNS nevet szeretne adni Reporting Services, módosítsa a $DNSName értékét. Paraméter használatával, ha a tanúsítvány is használni kell ezt a nevet, és regisztrálását globálisan a DNS-kiszolgáló neve.

1. A Reporting Services a parancsfájl van beállítva. Ha szeretne futtassa a Reporting Services, módosítsa a elérési útját a "v11", a Get-WmiObject kimutatásban névtér verziója részét.

1. Futtassa a.

**Érvényesítés**: Ellenőrizze, hogy működik-e a egyszerű jelentés kiszolgáló működését, olvassa el a [konfigurációjának ellenőrzése](#verify-the-connection) részben a témakör későbbi. A tanúsítvány ellenőrzéséhez kötés nyisson meg egy parancssorablakot rendszergazdai jogosultságokkal rendelkező, és futtassa az alábbi parancsot:

    netsh http show sslcert

Az eredmény lesz az alábbiakra terjed ki:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Állítsa be a jelentéskészítő kiszolgálót Configuration Manager használatával

Ha nem szeretné, hogy futtatni a PowerShell-parancsprogramot konfigurálása a jelentéskészítő kiszolgálót, kövesse az ebben a szakaszban a Reporting Services natív mód Konfigurációkezelő használatával állítsa be a jelentéskészítő kiszolgálót.

1. Az Azure klasszikus portálról jelölje ki a virtuális, és kattintson a Csatlakozás gombra. A felhasználónév és a úgy állította be, amikor a virtuális létrehozott jelszót használja.

    ![azure virtuális gép csatlakoztatása](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. A Windows update futtatása és a frissítések telepítése a virtuális. Ha újra kell indítani a virtuális szükség, indítsa újra a virtuális, és az Azure klasszikus portálról a virtuális újracsatlakozáshoz.

1. A virtuális Start menüjében írja be a **Reporting Services** , és nyissa meg a **Reporting Services Configuration Manager**.

1. Az alapértelmezett értékeket hagyja a **Kiszolgáló neve** és a **Jelentés kiszolgálói példány**. Kattintson a **Csatlakozás**gombra.

1. A bal oldali ablaktáblában kattintson a **Webhely szolgáltatás URL-CÍMÉT**.

1. RS alapértelmezés szerint be van állítva az IP-"Az összes hozzárendelt" 80-as HTTP port. HTTPS hozzáadása:

    1. Az **SSL-tanúsítvány**: válassza ki a tanúsítványt, például [a virtuális neve] használni kívánt. cloudapp.net. A tanúsítványok nem szerepel a listán, ha a szakaszban olvashat **lépés 2: kiszolgálói tanúsítvány létrehozása** miként telepítheti és a virtuális lévő tanúsítvány megbízható információkat.
    
    1. Az **SSL-Port**: válassza ki a 443-as. Ha úgy állította be a magánjellegű HTTPS-végpont a különböző magánjellegű portokkal a virtuális, használja az alábbi ezt az értéket.
    
    1. Kattintson az **Alkalmaz** gombra, és várja meg a művelet befejeződik.

1. A bal oldali ablaktáblában kattintson az **adatbázisból**.

    1. Kattintson a **Módosítás Databas**-e.
    
    1. Kattintson a **jelentés kiszolgálói új adatbázis létrehozása** , és kattintson a **Tovább**gombra.
    
    1. Kilépés az alapértelmezett **Kiszolgáló neve**: a virtuális megegyezik a név mezőt pedig hagyja az alapértelmezett **Hitelesítés típusa** **Aktuális felhasználó** – **Integrált adatvédelem**. Kattintson a **Tovább**gombra.
    
    1. Hagyja az alapértelmezett **jelentéskészítő** **Adatbázis nevét** , és kattintson a **Tovább**gombra.
    
    1. Hagyja az alapértelmezett **Hitelesítés típusa** **Szolgáltatás hitelesítő adatait** , és kattintson a **Tovább**gombra.
    
    1. Az **Összegzés** lapon kattintson a **Tovább gombra** .
    
    1. A konfiguráció elkészült, kattintson a **Befejezés gombra**.

1. A bal oldali ablaktáblában kattintson a **Jelentéskezelő URL-CÍMÉT**. Az alapértelmezett **Virtuális könyvtár** **jelentésként** hagyja, és kattintson az **Alkalmaz**gombra.

1. Kattintson a **Kilépés** a Reporting Services Configuration Manager bezárásához.

## <a name="step-4-open-windows-firewall-port"></a>4 lépésben: Megnyitás Windows tűzfal Port

>[AZURE.NOTE] Ha korábban a parancsfájlok konfigurálása a jelentéskészítő kiszolgálót, kihagyhatja ebben a szakaszban. A parancsprogram kattintva nyissa meg a tűzfal port lépés tartalmazza. Az alapértelmezett HTTP 80-as és 443-as port HTTPS-volt.

A virtuális távolról csatlakozni Jelentéskezelő vagy a virtuális gépen a jelentéskészítő kiszolgálót, a TCP-végpont van szükség. Szükség van a virtuális tűzfal ugyanazt a portot megnyitásához. A virtuális kiépített a végpont hozta létre.

Ez a témakör az alapvető információk nyissa meg a tűzfal port olvashat. További tudnivalókért olvassa el a [tűzfalat a jelentés kiszolgálói hozzáférés konfigurálása](https://technet.microsoft.com/library/bb934283.aspx) című témakört.

>[AZURE.NOTE] Ha korábban a parancsfájl konfigurálása a jelentéskészítő kiszolgálót, kihagyhatja ebben a szakaszban. A parancsprogram kattintva nyissa meg a tűzfal port lépés tartalmazza.

Ha úgy állította be a magánjellegű port HTTPS-eltérő 443-as, módosítsa a következő parancsfájl megfelelően. Nyissa meg a Windows tűzfal be a **443-as** port, hajtsa végre a következőket:

1. Rendszergazdai jogosultságokkal a Windows PowerShell-ablak megnyitása

1. Ha eltérő 443-as port, a HTTPS-végpont a virtuális konfigurálásakor használt, a következő parancsot a portot frissítése, és parancsot:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. A parancs befejezésekor, **Ok** jelenik meg a parancssort.

Annak ellenőrzéséhez, hogy a port nyitja meg, nyissa meg a Windows PowerShell-ablakot, és futtassa az alábbi parancsot:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>A konfigurációjának ellenőrzése

Ha ellenőrizni szeretné, hogy most már működik-e az egyszerű jelentés kiszolgálói funkciókat, a böngészőben nyissa meg a rendszergazdai jogosultságokkal rendelkező, és majd böngészéssel keresse meg a következő jelentés ad jelentés Kiszolgálókezelő URL-CÍMEK:

- A virtuális tallózással keresse meg a jelentés kiszolgáló URL-címe:

        http://localhost/reportserver

- A virtuális tallózással keresse meg a jelentés kezelő URL-címe:

        http://localhost/Reports

- A helyi számítógépen nyissa meg a **távoli** jelentés Manager a virtuális. Frissítse a DNS-név szükség szerint a következő példában. Amikor a rendszer kéri a jelszót, akkor jön létre, ha a virtuális kiépített rendszergazdai hitelesítő adatok használata A felhasználónév tartományban van a []\[felhasználónév] formátumban, ahol a tartomány-e a virtuális számítógép nevét, például ssrsnativecloud\testuser. Ha nem használ HTTP-**S**, távolítsa el az **s** URL-címét. Információk a következő szakaszban nézze meg a többi felhasználó virtuális hoz létre.

        https://ssrsnativecloud.cloudapp.net/Reports

- A helyi számítógépen nyissa meg a távoli jelentés kiszolgáló URL-címe. Frissítse a DNS-név szükség szerint a következő példában. Ha nem használ HTTPS, távolítsa el az s az URL-cím.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Felhasználók létrehozása és szerepkörök kiosztása

A jelentéskészítő kiszolgálót ellenőrzéséhez és konfigurálása után a leggyakoribb felügyeleti feladatokat, hogy hozzon létre egy vagy több felhasználó felhasználók hozzárendelése a Reporting Services szerepkörök. További tudnivalókért lásd: a következőket:

- [Helyi felhasználói fiók létrehozása](https://technet.microsoft.com/library/cc770642.aspx)

- [Engedélyek biztosítása felhasználó hozzáférését a jelentés-kiszolgáló (Jelentéskezelő)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Létrehozhatja és kezelheti a szerepkör-hozzárendelés](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Hozhat létre, és tegye közzé a jelentéseket a Azure virtuális géphez

Az alábbi táblázat összefoglalja, hogy a meglévő kimutatások egy helyszíni számítógépről, a jelentéskészítő kiszolgálón tárolt a a Microsoft Azure virtuális gép közzététele elérhető beállítások:

- **RS.exe parancsfájl**: jelentés-elemek és a meglévő jelentéskészítő kiszolgálót másolni szeretné a Microsoft Azure virtuális gép használata RS.exe parancsfájl. További információ című "Natív lehetőség – a Microsoft Azure virtuális gép natív mód" a [minta Reporting Services rs.exe parancsfájlt, hogy a tartalom áttelepítése jelentés kiszolgálók között](https://msdn.microsoft.com/library/dn531017.aspx).

- **A jelentéskészítő**: A virtuális gép is tartalmaz, kattintson a-egyszer a Microsoft SQL Server jelentéskészítő használatával létrehozott verzióját. Jelentés builder az első alkalommal indítása a virtuális gépen:

    1. Indítsa el a böngésző rendszergazdai jogosultságokkal rendelkező.
    
    1. Keresse meg a virtuális gépen Jelentéskezelő, és kattintson a menüszalagon a **Jelentéskészítő használatával létrehozott** .

    További tudnivalókért lásd: [telepítése, eltávolítása, és a jelentéskészítő támogatására](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: virtuális**: Ha a virtuális az SQL Server 2012 létrehozott, majd az SQL Server Data Tools a virtuális gépen telepítve van, és a **Jelentés kiszolgálói projektek** és a virtuális gépen jelentések létrehozásához használható. Az SQL Server Data Tools közzéteheti a jelentések, a jelentéskészítő kiszolgálón a virtuális gépen.
    
    Ha a virtuális létrehozott SQL Server-2014-es, telepítheti az SQL Server adatainak eszközök - BI for visual Studio. További tudnivalókért lásd: a következőket:

    - [A Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013-ban](https://www.microsoft.com/download/details.aspx?id=42313)

    - [A Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [Az SQL Server Data Tools és az SQL Server üzleti intelligenciával kapcsolatos funkciók (SSDT-BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: távoli**: a helyi számítógépen Reporting Services projekt létrehozása az SQL Server Data Tools Reporting Services-jelentéseket tartalmazó. Állítsa be a projekt való csatlakozáshoz a webes szolgáltatás URL-címe.

    ![projekt tulajdonságainak SSDT SSRS projekt](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Parancsfájl használata**: jelentés kiszolgálói tartalom másolása parancsfájl használatával. További tudnivalókért lásd: [minta Reporting Services rs.exe parancsfájlt, hogy a tartalom áttelepítése jelentés kiszolgálók között](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Ha nem használ a virtuális a költség kisméretűvé alakítása

>[AZURE.NOTE] Költségek esetében az Azure virtuális gépeken futó használaton minimalizálásához leállítása a virtuális az Azure klasszikus portálról. Ha a Windows power beállítások belül egy virtuális használatával állítsa le a virtuális, továbbra is az előfizetést terhelő ugyanakkora a virtuális. A díjak csökkentéséhez kell állítsa le a virtuális az Azure klasszikus portálon. Ha már nincs szüksége a virtuális, ne felejtse el a virtuális és a kapcsolódó .vhd fájlt tároló költségek elkerülésére törlése. További tudnivalókért lásd: a [Virtuális gépeken futó árak részletek](https://azure.microsoft.com/pricing/details/virtual-machines/)– gyakori kérdések szakaszban.

## <a name="more-information"></a>További információ

### <a name="resources"></a>Erőforrások

- Az SQL Server üzleti intelligenciával kapcsolatos funkciók és a SharePoint 2013-ban egy egyetlen server deployment kapcsolatos hasonló tartalmat lásd: [A Windows PowerShell használatával hozzon létre egy Azure virtuális az SQL Server üzleti Intelligencia és a SharePoint 2013-ban](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Hasonló tartalmak, az SQL Server üzleti intelligenciával kapcsolatos funkciók és a SharePoint 2013-ban egy több server deployment kapcsolatban olvassa el a [Telepíteni az SQL Server üzleti intelligenciával kapcsolatos funkciók az Azure virtuális gépeken futó](https://msdn.microsoft.com/library/dn321998.aspx)című témakört.

- Az SQL Server üzleti intelligenciával kapcsolatos funkciók az Azure virtuális gépeken futó telepítéseknél kapcsolatos általános tudnivalókért lásd: az [SQL Server üzleti intelligenciával kapcsolatos funkciók az Azure virtuális gépeken futó](virtual-machines-windows-classic-ps-sql-bi.md).

- További információt a költség az Azure számítja ki a költségek című témakör [árak Számológép Azure](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines)virtuális gépeken futó lapja.

### <a name="community-content"></a>Közösségi tartalom

- Lépésenkénti útmutatást a Reporting Services natív mód jelentéskészítő kiszolgálót létrehozása nélkül parancsfájl, akkor olvassa el a [Szolgáltatója SQL Reporting Service Azure virtuális gépen](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Az SQL Server Azure VMs az egyéb forrásokra mutató hivatkozások

[SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)
