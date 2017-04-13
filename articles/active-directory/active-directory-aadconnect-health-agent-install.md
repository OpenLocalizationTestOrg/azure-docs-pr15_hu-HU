<properties
    pageTitle="Azure Active Directory-állapot ügynök csatlakozás telepítése |} Microsoft Azure"
    description="Ez a az Azure Active Directory csatlakozás állapot lap, amely leírja a ügynök telepítését, az Active Directory összevonási szolgáltatások és a szinkronizálási."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Connect állapot ügynök telepítése

A dokumentum megismerkedhet való telepítéséről és konfigurálásáról az Azure Active Directory csatlakozás állapot ügynökök keresztül. Az [alábbi](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent)letöltheti az ügynökök.

##  <a name="requirements"></a>Követelmények
Az alábbi táblázat az Azure Active Directory csatlakozás állapot használatának követelményei listáját.

| Követelmények | Leírás|
| ----------- | ---------- |
|Azure Active Directory-támogatás| Azure Active Directory csatlakozás állapota az Azure Active Directory prémium funkció, és csak az Azure Active Directory prémium verzióban. </br></br>További tudnivalókért lásd: [Ismerkedés az Azure Active Directory-támogatás](active-directory-get-started-premium.md) </br>Egy 30 napig ingyenes próbaverziója megkezdéséhez lásd: [Indítsa el a próbaverziót.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|Az első lépések az Azure Active Directory csatlakozás állapota az Azure AD a globális rendszergazdának kell lennie.|Alapértelmezés szerint csak a globális rendszergazdák telepítse és állítsa be az állapot ügynökök a kezdéshez, elérheti a portált, és Azure Active Directory csatlakozás állapot belül minden műveletek hajthatók végre. További tudnivalókért olvassa el a [az Azure Active directory felügyelete](active-directory-administer.md)című témakört. <br><br> Szerepkör alapú hozzáférés-vezérlés használata engedélyezheti, más felhasználók Azure Active Directory csatlakozás állapota az access a szervezetben. További tudnivalókért lásd: [szerepkör alapú hozzáférés-vezérlés az Azure Active Directory csatlakozás egészség.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Fontos:** A ügynökök telepítésekor használt fiókot a munkahelyi vagy iskolai fiókkal kell lennie. Nem lehet a Microsoft-fiókjával. További tudnivalókért lásd: a [Regisztrálás az Azure, mint egy szervezet](sign-up-organization.md)
|Az Azure Active Directory csatlakozás állapot ügynök telepítve van egyes célzott kiszolgálón| Azure Active Directory csatlakozás állapot szükséges adatokat jelenít meg a portálon megadására célzott kiszolgálókon ügynököt telepíteni. </br></br>Például az Active Directory összevonási szolgáltatások helyszíni infrastruktúra betöltése, a ügynök kell telepíteni az Active Directory összevonási szolgáltatások AD FS-Proxy és webes szolgáltatásalkalmazás-Proxy kiszolgálókon. Hasonlóképpen, adatok beolvasása a helyszíni Active Directory tartományi szolgáltatások infrastruktúra, a agent kell telepíteni kell a tartomány-vezérlőket. </br></br>**Fontos:** A ügynökök telepítésekor használt fiókot a munkahelyi vagy iskolai fiókkal kell lennie. Nem lehet a Microsoft-fiókjával. További tudnivalókért lásd: a [Regisztrálás az Azure, mint egy szervezet](sign-up-organization.md)|
|Azure service végpontjait kimenő kapcsolatok|Telepítési és runtime során a agent kiszolgálóhoz való csatlakozást igényelnek Azure Active Directory csatlakozás állapot szolgáltatási végpontok Ha a kimenő kapcsolódási zárolva van, győződjön meg arról, hogy a következő végpontok megjelennek az engedélyezett listájában: </br></br><li>& #42;. BLOB.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.servicebus.Windows.NET - Port: 5671 </li><li>https://Management.Azure.com </li><li>https://s1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.dc.ad.MSFT.NET/</li><li>https://Login.Windows.NET</li><li>https://Login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|A kiszolgálón futó a agent tűzfalportokat.| A agent kell ahhoz, hogy az Azure Active Directory állapot szolgáltatás végpontjait kommunikáció ügynök nyissa meg a következő tűzfalportokat szükséges.</br></br><li>TCP/UDP 443-as port</li><li>TCP/UDP-portok 5671</li>
|Az alábbi webhelyeket engedélyezése, ha engedélyezve van az Internet Explorer fokozott biztonság| Ha engedélyezve van az Internet Explorer fokozott biztonság, majd az alábbi webhelyeket engedélyezni kell a kiszolgálón, hogy telepítve a agent fog.</br></br><li>https://Login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://Login.Windows.NET</li><li>A szervezete megbízható Azure Active Directory összevonási kiszolgáló. Példa: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Az Azure Active Directory telepítése csatlakozás Active Directory összevonási szolgáltatások ügynök
A agent telepítés elindításához kattintson duplán az .exe fájl letöltött. Első képernyőjén kattintson a telepítés gombra.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health-requirements/install1.png)

A telepítés befejezése után kattintson a beállítása gombra.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health-requirements/install2.png)

A parancssorba indított néhány PowerShell, amely végrehajtja a Register-AzureADConnectHealthADFSAgent követ. Amikor a rendszer kéri, hogy jelentkezzen be az Azure, lépjen tovább, és jelentkezzen be.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health-requirements/install3.png)


Bejelentkezés után továbbra is PowerShell. Ha befejeződött, bezárhatja a PowerShell, és a konfigurálás befejeződött.

Ezen a ponton a szolgáltatások el kell indítani a agent figyelésére és a szükséges adatok összegyűjtése automatikusan engedélyezésének. Ha nem éri el a az előző szakaszokban ismertetett előtti követelmények, figyelmeztetések jelennek meg a PowerShell ablakban. Győződjön meg arról, a [követelmények](active-directory-aadconnect-health-agent-install.md#requirements) elvégzéséhez az ügynök telepítése előtt. Az alábbi képernyőképen látható-e hibák.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health-requirements/install4.png)

Ha ellenőrizni szeretné a ügynök telepítve van, keresse meg az alábbi szolgáltatások a kiszolgálón. Ha befejezte a konfigurációt, hogy már futnia kell. Egyéb esetben őket a rendszer leállítja mindaddig, amíg a konfigurálás befejeződött.

- Azure AD Connect állapot AD FS diagnosztika szolgáltatás
- Azure AD Connect állapot AD FS háttérismeretek szolgáltatás
- Azure AD Connect szolgáltatás felügyelete a állapot Active Directory összevonási szolgáltatások

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>A Windows Server 2008 R2-kiszolgálók ügynök telepítése

A lépéseket a Windows Server 2008 R2-kiszolgálók követelményei szerint:

1. Győződjön meg arról, hogy fut-e a kiszolgáló, Service Pack 1 vagy magasabb.
1. Kapcsolja ki a IE ESC agent telepítéshez:
1. Telepítse a Windows PowerShell 4.0 minden-kiszolgálót az Active Directory állapot ügynök telepítése előtt. A Windows PowerShell 4.0 telepítése:
 - Telepítse a [Microsoft .NET-keretrendszer 4.5](https://www.microsoft.com/download/details.aspx?id=40779) , a telepítőprogram letöltése a kapcsolat nélküli használata az alábbi hivatkozásra.
 - Telepítse a PowerShell ISE (a Windows-szolgáltatások)
 - Telepítse a [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Az Internet Explorer 10-es verzió telepítése vagy újabb a kiszolgálón. (Kötelező szolgáltatás állapota hitelesítést végezni a Azure rendszergazdai hitelesítő adataival.)
1. További információt a Windows PowerShell 4.0 telepítése a Windows Server 2008 R2, a Wikilapok cikke [Itt](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Active Directory összevonási szolgáltatások naplózásának engedélyezése

> [AZURE.NOTE] Ez a szakasz csak az Active Directory összevonási szolgáltatások összevonási kiszolgálóihoz vonatkozik.

Ahhoz, hogy a használati Analitikát funkció gyűjtése és adatok elemzése az Azure Active Directory csatlakozás állapot ügynök van szüksége az Active Directory összevonási szolgáltatások naplókat található információkat. Ezek a naplók alapértelmezés szerint nincs engedélyezve vannak. Az alábbi eljárással az AD FS naplózásának engedélyezéséhez, és keresse meg az AD FS naplókat az AD FS-kiszolgálókon.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Active Directory összevonási szolgáltatások 2.0-s naplózásának engedélyezése

1. Kattintson a **Start**gombra, mutasson a **program**, a **Felügyeleti eszközök**pontra, és válassza a **Helyi biztonsági házirend**.
2. Keresse meg a **Biztonsági ablakban házirend\Felhasználói Rights Management** mappát, és kattintson duplán a biztonsági naplózás létrehozása.
3. A **Helyi biztonsági beállítása** lapon ellenőrizze, hogy szerepel-e az Active Directory összevonási szolgáltatások 2.0-s szolgáltatásfiók. Ha nincs megadva, kattintson a **felhasználó vagy csoport hozzáadása** gombra, és vegye fel a listára, és kattintson **az OK**gombra.
4. A naplózás engedélyezése nyisson meg egy parancssort, emelt szintű jogosultsággal, és futtassa a következő parancsot:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zárja be a helyi biztonsági házirend, és nyissa meg a kezelés beépülő. Nyissa meg a kezelés beépülő modul, kattintson a **Start**gombra, mutasson a **program**, a **Felügyeleti eszközök**pontra, és válassza az Active Directory összevonási szolgáltatások 2.0-s kezelése.
6. Kattintson a Műveletek ablaktáblában összevonás-tulajdonságok szerkesztése.
7. **Összevonási szolgáltatás tulajdonságai** párbeszédpanelen kattintson az **esemény** fülre.
8. Jelölje ki a **sikeres** és **sikertelen eseményeket** jelölőnégyzetet.
9. Kattintson az **OK gombra**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>A Windows Server 2012 R2 rendszer AD FS naplózásának engedélyezése

1. Nyissa meg a **Helyi biztonsági házirend** **Kiszolgálókezelő** kezdőképernyőjén vagy a Kiszolgálókezelő nyissa meg a tálcán az asztalon, majd kattintson az **Eszközök/helyi biztonsági házirendje**.
2. Keresse meg a **Biztonsági ablakban házirend\Felhasználói jogok kiosztása** mappát, és kattintson duplán a **biztonsági naplózás létrehozása**.
3. A **Helyi biztonsági beállítása** lapon ellenőrizze, hogy szerepel-e az Active Directory összevonási szolgáltatások szolgáltatásfiók. Ha nincs megadva, kattintson a **felhasználó vagy csoport hozzáadása** gombra, és vegye fel a listára, és kattintson **az OK**gombra.
4. A naplózás engedélyezése nyisson meg egy parancssort, emelt szintű jogosultsággal, és futtassa a következő parancsot:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Zárja be a **Helyi biztonsági házirend**, és nyissa meg az **Active Directory összevonási szolgáltatások kezelése** beépülő (a Kiszolgálókezelő kattintson az eszközök, és válassza az Active Directory összevonási szolgáltatások kezelése).
6. Kattintson a Műveletek ablaktáblában **Összevonás-tulajdonságok szerkesztése**.
7. Összevonási szolgáltatás tulajdonságai párbeszédpanelen kattintson az **esemény** fülre.
8. A **sikeres és sikertelen naplózása** jelölőnégyzet bejelölése, és kattintson **az OK**gombra.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Keresse meg az AD FS-ellenőrzés napló


1. Nyissa meg az **Eseménynapló nevet**.
2. Nyissa meg a Windows-naplók pontra, és válassza a **Biztonság**.
3. A jobb oldalon kattintson a **Szűrő aktuális lehetőségre**.
4. A forrás válassza a **Active Directory összevonási szolgáltatások naplózás**elemre.

![Active Directory összevonási szolgáltatások naplók](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Ha létezik letiltani a naplózás az AD FS csoportházirend, majd az Azure Active Directory csatlakozás állapot ügynök nem tudja gyűjt információkat. Győződjön meg arról, hogy nem rendelkezik, előfordulhat, hogy lehet letiltani a naplózás csoportházirend.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Szinkronizálás az Azure Active Directory csatlakozás állapot ügynök telepítése
Az Azure Active Directory csatlakozás állapot agent a szinkronizálás automatikusan települ az Azure AD Connect legújabb verziójában. Szinkronizálási Azure AD Connect használatához szeretne a legújabb Azure AD Connect letöltheti és telepítheti azt. Letöltheti a legújabb verzió [Itt](http://www.microsoft.com/download/details.aspx?id=47594).

Ha ellenőrizni szeretné a ügynök telepítve van, keresse meg az alábbi szolgáltatások a kiszolgálón. Ha befejezte a konfigurációt, hogy már futnia kell. Egyéb esetben őket a rendszer leállítja mindaddig, amíg a konfigurálás befejeződött.

- Azure AD Connect állapota szinkronizálás háttérismeretek szolgáltatás
- Azure AD Connect szolgáltatás figyelése állapota szinkronizálás

![Azure AD Connect állapota szinkronizálás ellenőrzése](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Ne feledje, hogy Azure Active Directory csatlakozás állapot használatához Azure Active Directory prémium verzióban. Ha nem rendelkezik az Azure Active Directory prémium, nem tudja fejezze be a konfigurálását az Azure-portálon. További tudnivalókért lásd: a [követelmények lap](active-directory-aadconnect-health-agent-install.md#requirements).


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Kézi Azure Active Directory csatlakozás állapota szinkronizálás regisztráció
Ha a Azure Active Directory csatlakozás állapota szinkronizálás ügynök regisztráció nem sikerül Azure AD Connect sikeres telepítése után, az alábbi PowerShell-parancsot is használhatja manuálisan regisztrálhatja a ügynök.

>[AZURE.IMPORTANT] Csak a PowerShell paranccsal van szükség, ha a ügynök regisztráció Azure AD Connect telepítése után nem működik.

Az alábbi PowerShell parancs kötelező, csak ha az állapot ügynök regisztráció nem sikerül egy sikeres telepítés és Azure AD Connect konfigurálása után is. Az Azure Active Directory csatlakozás állapot szolgáltatások elkezdenek az ügynök sikeresen regisztráció után.

Manuálisan is regisztrálhatja az Azure Active Directory csatlakozás állapot agent a szinkronizálás használatával az alábbi PowerShell-parancsot:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

A parancs megnyitja a következő paraméterek:

- AttributeFiltering: (alapértelmezett) – Ha Azure AD Connect nem szinkronizálódó az alapértelmezett attribútum $true adja meg, és testre szabták szűrt attribútum használ. a $false különben a program.
- StagingMode: $false (alapértelmezett) – Ha az Azure AD Connect kiszolgáló nem szerepel a átmeneti tárolására $true módja, ha be van állítva a kiszolgáló átmeneti tárolására módot kell.

Amikor a rendszer kéri a hitelesítést ugyanazzal a globális rendszergazdai fiókkal kell használni (például: admin@domain.onmicrosoft.com) Azure AD Connect beállításához használt.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Az Active Directory tartományi szolgáltatások ügynök telepíti az Azure Active Directory csatlakoztatása
A agent telepítés elindításához kattintson duplán az .exe fájl letöltött. Első képernyőjén kattintson a telepítés gombra.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

A telepítés befejezése után kattintson a beállítása gombra.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

A parancssorba indított néhány PowerShell, amely végrehajtja a Register-AzureADConnectHealthADDSAgent követ. Amikor a rendszer kéri, hogy jelentkezzen be az Azure, lépjen tovább, és jelentkezzen be.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Bejelentkezés után továbbra is PowerShell. Ha befejeződött, bezárhatja a PowerShell, és a konfigurálás befejeződött.

Ezen a ponton a szolgáltatások el kell indítani a agent figyelésére és a szükséges adatok összegyűjtése automatikusan engedélyezésének. Ha nem éri el a az előző szakaszokban ismertetett előtti követelmények, figyelmeztetések jelennek meg a PowerShell ablakban. Győződjön meg arról, a [követelmények](active-directory-aadconnect-health-agent-install.md#requirements) elvégzéséhez az ügynök telepítése előtt. Az alábbi képernyőképen látható-e hibák.

![Azure AD Connect állapota az Active Directory tartományi szolgáltatások ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Ha ellenőrizni szeretné a ügynök telepítve van, keresse meg a tartományvezérlőnek a beállítás további szolgáltatásokat.

- Azure AD Connect állapot Active Directory tartományi szolgáltatások háttérismeretek szolgáltatás
- Azure AD Connect szolgáltatás felügyelete a rendszerállapot AD DS

Ha befejezte a konfigurációt, az alábbi szolgáltatások már futnia kell. Egyéb esetben őket a rendszer leállítja mindaddig, amíg a konfigurálás befejeződött.

![Azure AD Connect állapot ellenőrzése](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Az Active Directory tartományi szolgáltatások, a kiszolgáló Core ügynök telepíti az Azure Active Directory csatlakozás.
Miután telepítette az .exe fájl, hajtsa végre a regisztrációs folyamat az alábbi PowerShell-parancs használatával:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Azure Active Directory csatlakozás állapot ügynökök használni a HTTP-Proxy beállítása
Beállíthatja az Azure Active Directory csatlakozás állapot ügynökök dolgozhat a HTTP-proxy pontra.

>[AZURE.NOTE]
- "Netsh WinHttp beállítása ProxyServerAddress" használata nem támogatott a agent System.Net használ, hogy a Microsoft Windows HTTP-szolgáltatások helyett a webes kérelmek.
- A beállított Http-Proxy címet átadó titkosított Https-üzenetek szolgál.
- Hitelesített proxyk (használatával HTTPBasic) nem támogatottak.

### <a name="change-health-agent-proxy-configuration"></a>Módosítás állapot ügynök Proxy konfigurációja
Ha az alábbi beállítások konfigurálása az Azure Active Directory csatlakozás állapot ügynök használni a HTTP-proxy pontra.

>[AZURE.NOTE]Azure Active Directory csatlakozás állapot ügynök szolgáltatások újra kell indítani, annak érdekében, hogy frissíteni kell a proxybeállításokat. A következő parancsot:<br>
A restart-Service AdHealth *

#### <a name="import-existing-proxy-settings"></a>Meglévő proxy beállítások importálása

##### <a name="import-from-internet-explorer"></a>Importálás az Internet Explorerben
Az Internet Explorer HTTP-proxy beállításai lehet importálni, az Azure Active Directory csatlakozás állapot ügynökök való használatra. Minden egyes az állapot ügynök futtató kiszolgáló hajtsa végre az alábbi PowerShell-parancsot:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importálás a WinHTTP
Az Azure Active Directory csatlakozás állapot ügynökök való használatra importálhatók WinHTTP proxybeállításokat. Minden egyes az állapot ügynök futtató kiszolgáló hajtsa végre az alábbi PowerShell-parancsot:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>A proxykiszolgáló címek manuális megadása
A proxykiszolgáló manuálisan adhatja meg az egyes hajtja végre az alábbi PowerShell-parancsot futtató az állapot ügynök:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Példa: *Set-AzureAdConnectHealthProxySettings - HttpsProxyAddress myproxyserver: 443-as*

- "cím" DNS felbontási kiszolgálónév vagy IPv4-címet is lehet.
- "port" a nem hagyható. Ha hiányzik, majd a 443-as port alapértelmezett választja.

#### <a name="clear-existing-proxy-configuration"></a>Világos meglévő proxy konfigurációja
Törölheti a jelet a meglévő proxy konfigurációt a következő parancs futtatásával:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Olvassa el az aktuális proxybeállítások
A jelenleg beállított proxybeállítások erről a következő parancs futtatásával:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Azure Active Directory kapcsolat tesztelése csatlakoztatása a szolgáltatás állapota
Problémák felmerülhet az Azure Active Directory csatlakozás állapot ügynök az Azure Active Directory csatlakozás állapot szolgáltatás kapcsolatát elveszíti okozó lehetőség. Ide tartoznak a hálózati hibák, jogosultsági problémák vagy számos más oka lehet.

Ha a agent nem tudja adatok küldése a két óránál hosszabb ideig az Azure Active Directory csatlakozás állapot szolgáltatást, a portálon az alábbi értesítés jelzi: "a szolgáltatás állapota adat nem naprakész." Ha a szóban forgó Azure AD Connect állapot ügynök tölthet fel adatokat az Azure Active Directory csatlakozás állapot szolgáltatás az alábbi PowerShell parancs futtatásával győződhet meg:

    Test-AzureADConnectHealthConnectivity -Role ADFS

A szerepkör paraméter jelenleg hajtja végre a következő értékeket:

- ADFS
- Szinkronizálás
- HOZZÁADÁSA

A parancs a - ShowResults jelző segítségével részletes naplók megtekintése. Használja az alábbi példában:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]A kapcsolat eszközzel, a ügynök regisztráció először be kell fejezni. Ha nem sikerül a ügynök regisztráció befejezéséhez, győződjön meg arról, hogy az Azure Active Directory csatlakozás állapota a [követelmények](active-directory-aadconnect-health-agent-install.md#requirements) megfelel-e. Kapcsolat a végrehajtott próba alapértelmezés szerint ügynök regisztráció során.



## <a name="related-links"></a>Kapcsolódó hivatkozások

* [Azure AD Connect állapota](active-directory-aadconnect-health.md)
* [Azure AD Connect állapot műveletek](active-directory-aadconnect-health-operations.md)
* [Azure Active Directory használatával kapcsolatba állapot Active Directory összevonási szolgáltatások](active-directory-aadconnect-health-adfs.md)
* [Azure Active Directory csatlakozás állapota szinkronizálás használata](active-directory-aadconnect-health-sync.md)
* [Azure Active Directory segítségével kapcsolatba állapot Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect állapot – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect állapot korábbi verziók](active-directory-aadconnect-health-version-history.md)
