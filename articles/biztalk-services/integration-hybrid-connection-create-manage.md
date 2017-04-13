<properties 
    pageTitle="Hibrid adatkapcsolatok létrehozása és kezelése |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hibrid kapcsolatot létrehozni, a kapcsolat kezelése és telepítése a hibrid kapcsolatkezelő. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Hibrid adatkapcsolatok létrehozása és kezelése


## <a name="overview-of-the-steps"></a>A lépéseinek áttekintése
1. Hibrid kapcsolat létrehozása a magánhálózaton az **állomásnév** vagy **teljesen minősített tartománynév** a helyszíni erőforrás beírásával.
2. Hivatkozás az Azure web Apps alkalmazások vagy az Azure mobilalkalmazások a hibrid kapcsolatot.
3. A hibrid kapcsolatkezelő telepítése a helyszíni erőforrás, és csatlakozzon a hibrid kapcsolatfelvételi. Az Azure portál telepítése és csatlakozás a egyetlen kattintással funkcióit.
4. Hibrid kapcsolatok és kapcsolatot igényelhetnek kezelése.

Ez a témakör ezeket a lépéseket sorolja fel. 

> [AZURE.IMPORTANT] A hibrid kapcsolat végpontjának beállításához IP-címet is lehetőség. Ha egy IP-címet használja, előfordulhat, hogy, vagy előfordulhat, hogy nem éri a helyszíni erőforrás attól függően, hogy az ügyfelek. A hibrid kapcsolat attól függ, hogy az ügyfél, a DNS-keresés módon. A legtöbb esetben az __ügyfél__ az alkalmazás kódját. Ha az ügyfél ne hajtsa végre a DNS-keresés (ez nem próbálkozzon az IP-cím úgy, mintha egy tartománynevet (x.x.x.x)), majd a forgalom nem küldi el a hibrid kapcsolaton keresztül.
>
> Ha például (pseudocode) meghatározásával **10.4.5.6** a helyszíni szolgáltató:
> 
> **A következő esetben működik:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **A következő esetben nem működik:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Hibrid kapcsolat létrehozása

Az Azure-portálon Web Apps alkalmazások használata **vagy** BizTalk szolgáltatások használata hibrid kapcsolatot hozhat létre. 

**Web Apps alkalmazások használata a hibrid kapcsolatok létrehozása**, [Csatlakozás Azure Web Apps alkalmazások egy helyszíni erőforráshoz](../app-service-web/web-sites-hybrid-connection-get-started.md)látható. A hibrid kapcsolat Manager (HCM) az webalkalmazásból, amely a használni kívánt módot is telepítheti. 

**BizTalk szolgáltatások hibrid kapcsolások létrehozásához**:

1. Jelentkezzen be az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. A bal oldali navigációs ablaktáblában jelölje ki a **BizTalk szolgáltatások** , és válassza a a BizTalk szolgáltatásban. 

    Ha nem egy meglévő BizTalk szolgáltatást, akkor [létrehozása BizTalk szolgáltatás](biztalk-provision-services.md).
3. Jelölje ki a **Hibrid kapcsolatok** lapon:  
![Hibrid kapcsolatok lapon][HybridConnectionTab]

4. Válassza a **hibrid kapcsolat létrehozása** vagy a **hozzáadása** gomb a tevékenységsávon. Adja meg az alábbiakat:

    A tulajdonság | Leírás
--- | ---
név | A hibrid kapcsolatnév egyedinek kell lennie, és nem lehet a BizTalk szolgáltatás nevét. Adjon meg egy tetszőleges nevet, de a célt jellemző. Példák:<br/><br/>Bérszámfejtő*SQL Server*<br/>SupplyList*SharepointServer*<br/>Ügyfelek*OracleServer*
Állomásnév | A teljesen minősített állomásnév esetén írja be az állomásnév vagy a helyszíni erőforrás IPv4-címét. Példák:<br/><br/>mySQLServer<br/>*mySQLServer*. *Tartomány*. corp.*cegnev*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *cegnev*.com<br/>10.100.10.10<br/><br/>Ha a IPv4-címet használja, vegye figyelembe, hogy az ügyfél vagy egy alkalmazás kódot sem oldja meg a IP-cím. A fontos Megjegyzés: Ez a témakör tetején látható.
Port | Adja meg, hogy a helyszíni erőforrás portszámot. Ha például Web Apps alkalmazások használata, írja be 80 és 443-as port. Ha az SQL Server használata esetén írja be a port 1433.

5. Jelölje ki a jelölőnégyzet be van jelölve a beállítási folyamat befejezéséhez. 

#### <a name="additional"></a>További

- Több hibrid kapcsolatokat hozhat létre. Lásd: a [BizTalk szolgáltatások: kiadásai diagram](biztalk-editions-feature-chart.md) engedélyezett kapcsolatok száma. 
- Minden egyes hibrid kapcsolat jön létre a csatlakozási_karakterlánc két: alkalmazás kulcsok, a KÜLDÉS és a helyszíni kulcsok, amely meghallgatása. Minden egyes pár tartalmaz egy elsődleges és másodlagos kulcsot. 


## <a name="LinkWebSite"></a>Hivatkozás az Azure alkalmazás szolgáltatás webalkalmazás vagy mobilalkalmazásban

Egy meglévő hibrid kapcsolat Web Appot vagy az Azure-App szolgáltatásban mobilalkalmazás mutató hivatkozás létrehozásához válassza **meglévő hibrid kapcsolat használata** a hibrid kapcsolatok lap. Lásd: a [hozzáférést a helyszíni hibrid kapcsolatok Azure alkalmazás szolgáltatás használata erőforrások](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>A hibrid kapcsolatkezelő helyszíni telepítéséhez

Hibrid kapcsolat létrehozását követően telepítse a hibrid kapcsolatkezelő a helyszíni erőforráson. Letölthető az Azure web Apps alkalmazások vagy a BizTalk szolgáltatásból. BizTalk szolgáltatások lépéseket: 

1. Jelentkezzen be az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. A bal oldali navigációs ablaktáblában jelölje ki a **BizTalk szolgáltatások** , és válassza a a BizTalk szolgáltatásban. 
3. Jelölje ki a **Hibrid kapcsolatok** lapon:  
![Hibrid kapcsolatok lapon][HybridConnectionTab]
4. Jelölje ki **a helyszíni telepítés**menüsávján:  
![A helyszíni telepítés][HCOnPremSetup]
5. Válassza a **telepítés és beállítás** futtatását, és töltse le a hibrid kapcsolatkezelő a helyszíni rendszeren. 
6. Jelölje ki a jelölőnégyzet be van jelölve a telepítés megkezdéséhez. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>További
- Hibrid kapcsolatkezelő telepíthető az alábbi operációs rendszereken:

    - A Windows Server 2008 R2 (a .NET-keretrendszer 4.5 + és a Windows Management Framework 4.0 + szükséges)
    - A Windows Server 2012 (Windows Management Framework 4.0 + szükséges)
    - A Windows Server 2012 R2


- Miután telepítette a hibrid kapcsolatkezelő, a következő történik: 

    - A hibrid kapcsolat Azure is automatikusan az elsődleges alkalmazás kapcsolati karakterlánc használatára van beállítva. 
    - A helyszíni erőforrás automatikusan a helyszíni elsődleges kapcsolattal használatára van beállítva.

- A hibrid kapcsolatkezelő érvényes helyszíni kapcsolati karakterlánc engedélyt kell használnia. Az Azure Web Apps alkalmazások vagy Mobile-alkalmazások engedélyezési érvényes alkalmazás kapcsolati karakterlánc kell használnia.
- Hibrid kapcsolatok méretezheti egy másik kiszolgálón egy másik példányában hibrid kapcsolatkezelő telepítésével. Állítsa be a helyszíni értesülnie használni az első helyszíni figyelő ugyanazt a címet. Ebben az esetben a forgalmat a helyszíni active hallgatók között a véletlenszerűen elosztott (ciklikus). 


## <a name="ManageHybridConnection"></a>Hibrid kapcsolatok kezelése
A hibrid kapcsolatokat kezeléséhez a következőkre van lehetősége:

- Használja az Azure-portálra, és nyissa meg a BizTalk szolgáltatásban. 
- Használja a [REST API-khoz](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>A hibrid a Csatlakozási_karakterlánc másolása és újragenerálása

1. Jelentkezzen be az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. A bal oldali navigációs ablaktáblában jelölje ki a **BizTalk szolgáltatások** , és válassza a a BizTalk szolgáltatásban. 
3. Jelölje ki a **Hibrid kapcsolatok** lapon:  
![Hibrid kapcsolatok lapon][HybridConnectionTab]
4. Jelölje ki a hibrid kapcsolatot. Jelölje ki a tevékenységsávon **Kapcsolat kezelése**:  
![Beállításainak kezelése][HCManageConnection]

    **Kapcsolat kezelése** sorolja fel, hogy az alkalmazás és a helyszíni kapcsolati karakterláncot. Másolja a vágólapra a kapcsolati karakterláncot, és a kapcsolati karakterláncban használt hívóbetű újragenerálása. 

    **Ha bejelöli a újragenerálása**, a megosztott hívóbetű belül a kapcsolati karakterlánc használt módosul. Tegye a következőket:
    - Az Azure klasszikus portálon válassza a **Szinkronizálás billentyűk** az Azure alkalmazásban.
    - Futtassa újra a **Helyszíni telepítés**. A helyszíni telepítés újra futtatásakor a helyszíni erőforrás automatikusan használatára van beállítva a frissített elsődleges kapcsolati karakterláncot.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Csoportházirend alkalmazása hibrid kapcsolat által használt helyszíni erőforrások kezelése

1. A [hibrid kapcsolatkezelő felügyeleti sablonok](http://www.microsoft.com/download/details.aspx?id=42963)letöltése.
2. Bontsa ki a fájlokat.
3. Azon a számítógépen, amely csoportházirend módosítja tegye a következőket:  

    - Másolás a. ADMX fájlokat a *%WINROOT%\PolicyDefinitions* mappába.
    - Másolás a. ADML fájlokat a *%WINROOT%\PolicyDefinitions\en-us* mappába.

Miután másolta, a csoportházirend-szerkesztőben a házirend módosítása is használhatja.




## <a name="next"></a>Következő

[Azure Web Apps alkalmazások csatlakoztatása egy helyszíni erőforrás](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Csatlakozás helyszíni SQL Server Azure webes alkalmazásokból](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Hibrid az adatkapcsolatok áttekintése](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Lásd még:

[REST API-t, a Microsoft Azure BizTalk szolgáltatásainak kezelése](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk szolgáltatások: Kiadásai diagram](biztalk-editions-feature-chart.md)  
[Hozzon létre egy BizTalk szolgáltatás Azure klasszikus portál használatával](biztalk-provision-services.md)  
[BizTalk szolgáltatások: Irányítópult, a Monitor és a méret lapon](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
