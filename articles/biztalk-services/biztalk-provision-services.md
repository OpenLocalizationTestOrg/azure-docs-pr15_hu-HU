<properties
    pageTitle="Azure BizTalk Services létrehozása az Azure-portálon |} Microsoft Azure"
    description="Megtudhatja, hogy miként kiépítése, vagy hozzon létre az Azure BizTalk szolgáltatást az Azure-portálon; MABS, WABS"
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
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Az Azure portálon BizTalk szolgáltatások létrehozása

Azure BizTalk Services létrehozása az Azure-portálon.

> [AZURE.TIP] Jelentkezzen be az Azure-portálra, szüksége van egy Azure-fiók és az Azure előfizetés. Ha nem rendelkeznek fiókkal, létrehozhat egy ingyenes próba-fiókkal néhány percen belül. Lásd: [Azure ingyenes próbaverziót](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Hozzon létre egy BizTalk szolgáltatás
Attól függően, hogy a választott Edition nem az összes BizTalk szolgáltatás beállításai nem érhető el.

1. Jelentkezzen be az [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Az alsó navigációs ablaktáblában válassza az **Új**:  
![Jelölje be az új gombon][NEWButton]

3. Válassza az **alkalmazás SERVICES** > **BIZTALK szolgáltatás** > **egyéni létrehozása**:  
![Jelölje be a BizTalk szolgáltatás, és válassza a Custom létrehozása][NewBizTalkService]

4. Írja be a BizTalk szolgáltatás beállításai:

    <table border="1">
    <tr>
    <td><strong>BizTalk szolgáltatás neve</strong></td>
    <td>Adjon meg egy tetszőleges nevet, de jellemző. Néhány példa:<br/><br/>
    <em>értéket</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>myapplication</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" automatikusan felkerül a név. Ezzel létrehoz egy URL-CÍMÉT, például a BizTalk szolgáltatás eléréséhez használt <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Ha tesztelése/fejlesztési szakaszában, válassza a <strong>fejlesztői</strong>. Ha a gyártás szakaszban, használja a <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk szolgáltatások: kiadásai diagram</a> annak megállapításához, hogy <strong>prémium</strong>, <strong>normál</strong>vagy <strong>egyszerű</strong> az üzleti igényektől helyes választás.
    </td>
    </tr>
    <tr>
    <td><strong>Régió</strong></td>
    <td>Jelölje ki a földrajzi régióban tárolni a BizTalk szolgáltatásban.</td>
    </tr>
    <tr>
    <td><strong>Tartományi URL-cím</strong></td>
    <td><strong>Nem kötelező</strong>. Alapértelmezés szerint ki a tartományi URL-cím <em>YourBizTalkServiceName</em>. biztalk.windows.net. Egyéni tartományt is meg lehet adni. Ha például <em>a contoso</em>a tartomány esetén is megadhat: <br/><br/>
    <em>Értéket</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Jelölje be a következő nyílra.

5. Írja be a tár és az adatbázis-beállítások:

    <table border="1">
    <tr>
    <td><strong>Figyelés és archiválás tárterület-fiók</strong></td>
    <td>Jelöljön ki egy meglévő tárterület-fiókot vagy hozzon létre egy új tárterület-fiókot. <br/><br/>Hoz létre egy új tárterület-fiókkal, ha adja meg a <strong>Tárterület-fiók nevére</strong>.</td>
    </tr>
    <tr>
    <td><strong>A nyomon követés adatbázis</strong></td>
    <td>Ha egy meglévő Azure SQL-adatbázissal, azt egy másik BizTalk szolgáltatás nem használhatók. A bejelentkezési név van szüksége, és a jelszó Azure SQL-adatbázis kiszolgálója készült, hogy mikor megadott.<br/><br/><strong>Tipp:</strong> A nyomon követés adatbázis és a figyelés és archiválás tárterület-fiók létrehozása a BizTalk szolgáltatásként ugyanabban a régióban.</td>
    </tr>
    </table>
Jelölje be a következő nyílra.

6. Adja meg az adatbázis-beállításokat:

    <table border="1">
    <tr>
    <td><strong>név</strong></td>
    <td>Érhető el, ha kijelölt <strong>egy új SQL-adatbázis-példány létrehozása</strong> az előző képernyőre.
    <br/><br/>
   Írja be a BizTalk szolgáltatás által használt SQL-adatbázis nevét.</td>
    </tr>
    <tr>
    <td><strong>Kiszolgáló</strong></td>
    <td>Érhető el, ha kijelölt <strong>egy új SQL-adatbázis-példány létrehozása</strong> az előző képernyőre.
    <br/><br/>
   Jelölje ki egy meglévő SQL-adatbázis kiszolgálója, vagy hozzon létre egy új SQL-adatbázis kiszolgálója.</td>
    </tr>
    <tr>
    <td><strong>Kiszolgáló a bejelentkezési neve</strong></td>
    <td>Írja be a bejelentkezési felhasználónév.</td>
    </tr>
    <tr>
    <td><strong>Adatbázisjelszó kiszolgáló</strong></td>
    <td>Írja be a bejelentkezési jelszavát.</td>
    </tr>
    <tr>
    <td><strong>Régió</strong></td>
    <td>Érhető el, ha az <strong>Új SQL-adatbázis-példány létrehozása</strong> ki van jelölve. Jelölje ki a földrajzi területhez tartozik, az SQL-adatbázis tárolni.</td>
    </tr>
    </table>

Jelölje ki a jelölőnégyzet be van jelölve a varázsló. A haladás ikon jelenik meg:  
![Folyamatban ikont jeleníti meg, ha teljes][ProgressComplete]

Amikor elkészült, az Azure BizTalk szolgáltatása létezik, és készen áll a az alkalmazások. Az alapértelmezett beállítások megfelelőek. Alapértelmezett beállításait módosítani szeretné, ha a bal oldali navigációs ablaktáblában válassza a **BIZTALK szolgáltatások** , és válassza ki a BizTalk szolgáltatásban. A további beállítások a [irányítópult, a Monitor, és a méret lapon](biztalk-dashboard-monitor-scale-tabs.md) , a képernyő tetején jelenik meg.

Attól függően, hogy a BizTalk szolgáltatás állapotát bizonyos műveletek, amelyek nem fejeződhet be vannak. Ezek a műveletek listáját lépjen [BizTalk szolgáltatások állapotát diagram](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Utáni kiépítési lépéseket

-  [A tanúsítvány telepítése a helyi számítógépre](#InstallCert)
-  [A gyártási kész-tanúsítvány felvétele](#AddCert)
-  [A hozzáférés-vezérlés névtér beszerzése](#ACS)

#### <a name="InstallCert"></a>A tanúsítvány telepítése a helyi számítógépre
Önaláírt tanúsítvány BizTalk szolgáltatás kiépítési részeként létrejön, és az BizTalk szolgáltatás előfizetéséhez társított. A tanúsítvány letöltése kell, és hol BizTalk alkalmazások terjesztése vagy üzeneteket küldeni egy BizTalk szolgáltatás végpontjának a számítógépen telepíteni.

1. Jelentkezzen be az [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Jelölje ki a **BIZTALK szolgáltatások** a bal oldali navigációs ablakban, és válassza a szolgáltatás BizTalk előfizetését.
3. Válassza az **Irányítópult** lapon.
4. Válassza **az SSL-tanúsítvány letöltése**:  
![Az SSL-tanúsítvány módosítása][QuickGlance]
5. Kattintson duplán a tanúsítványra, majd futtassa a tanúsítvány telepítése a varázsló lépéseit. Ellenőrizze, hogy telepítette a tanúsítvány csoportban a **Megbízható legfelső szintű hitelesítésszolgáltatók** áruházból.

#### <a name="AddCert"></a>A gyártási kész-tanúsítvány felvétele
Ha csak a fejlesztői környezetben BizTalk szolgáltatások létrehozásának íródott automatikusan létrehozott önaláírt tanúsítvány. Ha gyártási esetben cserélhet le egy gyártási kész tanúsítványt.

1. Az **Irányítópult** lapon válassza a **Frissítés SSL-tanúsítványt**.
2. Tallózással keresse meg a személyes SSL-tanúsítvány (*CertificateName*.pfx), amely tartalmazza a BizTalk szolgáltatás neve, írja be a jelszót, és kattintson a pipára.

#### <a name="ACS"></a>A hozzáférés-vezérlés névtér beszerzése

1. Jelentkezzen be az [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Jelölje ki a **BIZTALK szolgáltatások** a bal oldali navigációs ablakban, és válassza a a BizTalk szolgáltatásban.
3. A tálcájának jelölje ki a **Kapcsolat adatait**:  
![Jelölje ki a kapcsolat adatait][ACSConnectInfo]

4. Másolja a hozzáférés-vezérlés értékeket.

Ha telepíti a Visual Studio BizTalk szolgáltatási projekt, írja be a hozzáférés-vezérlés névtér. A hozzáférés-vezérlés névtér automatikusan létrejön a BizTalk szolgáltatásban.

A hozzáférés-vezérlés értékek bármely alkalmazásban használható. Azure BizTalk szolgáltatások létrehozásakor a hozzáférés-vezérlés névtér a a BizTalk szolgáltatás üzembe hitelesítéssel szabályozza. Ha szeretné módosítani az előfizetés vagy a névtér kezelése, válassza **Az ACTIVE DIRECTORY** a bal oldali navigációs ablakban, majd a névtér. A tálcán a lehetőségeket sorolja fel.

**Kezelés** gombra kattintva megnyílik az Access vezérlő adatkezelési portálon. Az Access vezérlő felügyeleti portálon a BizTalk szolgáltatás **szolgáltatás identitások**használja:  
![ACS szolgáltatás identitások elérése szabályozhatja az adatkezelési portálon][ACSServiceIdentities]

A hozzáférés-vezérlés szolgáltatás identitás hitelesítő adatait, amelyek lehetővé teszik az alkalmazások és a közvetlenül a hozzáférés-vezérlés hitelesítést végezni, és egy token fogadni.

> [AZURE.IMPORTANT]A BizTalk szolgáltatás **tulajdonosa** a szolgáltatás alapértelmezett identitás és a **jelszó** értéket használja. Ha a jelszó érték helyett szimmetrikus kulcs értékét használja, az alábbi hiba akkor fordulhat.<br/><br/>*Nem sikerült csatlakozni a Control Management szolgáltatást fiókot a megadott hitelesítő adatokkal*

[A ACS Namespace kezelése](https://msdn.microsoft.com/library/azure/hh674478.aspx) néhány útmutatások és javaslatok listája.

## <a name="requirements-explained"></a>Követelmények magyarázata

Ezekről a követelményekről az ingyenes Edition nem vonatkoznak.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Mire van szüksége</strong></td>
        <td><strong>Miért van szükség</strong></td>
</tr>
<tr>
<td>Azure előfizetés</td>
<td>Az előfizetés határozza meg, akik jelentkezhet be az Azure-portálra. A fióktulajdonos az előfizetést az <a HREF="https://account.windowsazure.com/Subscriptions">Azure előfizetések</a>hoz létre.
<br/><br/>
Az Azure-fiók is több előfizetéssel rendelkezik, és bárki, aki engedélyezett (permitted) kezelhető. Például az Azure fióktulajdonos <em>BizTalkServiceSubscription</em> nevű előfizetést hoz létre, és ad a BizTalk rendszergazdák, a vállalaton belül (például ContosoBTSAdmins@live.com) az előfizetés a hozzáférést. Ebben az esetben a BizTalk rendszergazdák jelentkezzen be az Azure-portálra és a szolgáltatott szolgáltatások teljes rendszergazdai jogosultságokat az előfizetés, beleértve az Azure BizTalk szolgáltatásokat. A BizTalk a rendszergazdák nem Azure-fiók tulajdonosainak és így nem férhetnek hozzá számlázási információkat.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Az előfizetések kezelése és a tárterület-fiókokat az Azure-portálon</a> nyújt további információt.
</td>
</tr>
<tr>
<td>Azure SQL-adatbázis</td>
<td>A táblák, nézetek és a BizTalk szolgáltatás, beleértve a nyomon követési adatok által használt tárolt eljárások tárolja.
<br/><br/>
Amikor létrehoz egy BizTalk szolgáltatás, egy meglévő Azure SQL Server Azure SQL-adatbázis használata, vagy automatikusan hozzon létre egy új kiszolgáló vagy egy adatbázis.
<br/><br/>
Az SQL-adatbázis skála automatikusan be van állítva. Az alapértelmezett méretarány általában egy BizTalk szolgáltatás elegendő. A skála módosítása árak hatással van. Lásd: a <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">fiók és számlázás az Azure SQL-adatbázis</a>
<br/><br/>
<strong>Jegyzetek</strong>
<br/>
<ul>
<li> Amikor létrehoz egy új adatbázist és Azure SQL Server, az Azure szolgáltatás automatikusan engedélyezve van. A BizTalk szolgáltatáshoz Azure Services engedélyezni.</li>
<li>Ha egy meglévő Azure SQL Server hoz létre egy új Azure SQL-adatbázissal, a tűzfal szabályokat, a kiszolgáló nem változnak. Emiatt lehetőség más Azure szolgáltatások nem használhatók a kiszolgáló-adatbázisokhoz való hozzáférés.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure hozzáférés-vezérlés névtere</td>
<td>Azure BizTalk szolgáltatások hitelesíti. Ha telepíti a Visual Studio BizTalk szolgáltatási projekt, írja be a hozzáférés-vezérlés névtér. Amikor létrehoz egy BizTalk szolgáltatás, a hozzáférés-vezérlés névtér automatikusan létrejön.</td>
</tr>

<tr>
<td>Azure tárterület-fiók</td>
<td>Táblázatok, BLOB és a következő mentése a BizTalk szolgáltatás által használt hozzáférést biztosít:

<ul>
<li>Az naplófájlok, hogy figyelheti a BizTalk szolgáltatást. A felügyeleti eredménye a **Figyelés** lapon az Azure-portálon is megjelenik.</li>
<li>Egy X12 vagy a partnerek közötti AS2 szerződés létrehozásakor engedélyezheti az Archiválás szolgáltatás üzenet tulajdonságai tárolásához. Ezeket az adatokat tároló fiók menti a program.</li>
</ul>
<br/>
Amikor létrehoz egy BizTalk szolgáltatás, egy meglévő tárterület-fiókot használ, vagy automatikusan hozzon létre egy új tárterület-fiókot.
<br/><br/>
Az alapértelmezett tároló beállítások megfelelőek BizTalk szolgáltatáshoz.
<br/><br/>
Amikor létrehoz egy tárterület-fiókkal, egy elsődleges kulcs és egy másodlagos kulcsa automatikusan létrejönnek. Billentyűk a tárterület-fiók elérésének szabályozása A BizTalk szolgáltatás automatikusan használja az elsődleges kulcs.
<br/><br/>
A <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">tárhely</a> című témakör további információt.
</td>
</tr>

<tr>
<td>A magánjellegű az SSL-tanúsítvány</td>
<td>
Az Azure BizTalk szolgáltatásainak létrehozásakor is létrejön egy HTTPS URL-címet, amely tartalmazza a BizTalk szolgáltatás neve. Ez az URL automatikusan csak fejlesztési önaláírt tanúsítvány használatára van beállítva. Gyártási szüksége van egy privát SSL-tanúsítványt.
<br/><br/>
<strong>Fontos az SSL-tanúsítvány adatait</strong>

<ul>
<li>A tanúsítvány lejárati idő kisebb, mint 5 év kell lennie.</li>
<li>Saját tanúsítványok jelszó szükséges. Ismeri a jelszót, és a legjobb módszer oszthat meg a jelszót a rendszergazdák.</li>
<li>A próba/fejlesztői környezet önaláírt tanúsítványok használatosak. Önaláírt tanúsítványok használata esetén a tanúsítvány importálása a személyes tanúsítvány áruház és a megbízható legfelső szintű hitelesítésszolgáltatók tanúsítvány áruházból.</li>
</ul>
<br/>A gyártási tanúsítvány kérést küld a hitelesítésszolgáltató, amikor adja meg a következő tanúsítvány tulajdonságok:
<br/>

<ul>
<li><strong>Továbbfejlesztett billentyű használatát</strong>: legalább az Azure BizTalk szolgáltatások a kiszolgáló hitelesítést igényel.</li>
<li><strong>Közös neve</strong>: Adja meg az Azure BizTalk szolgáltatás URL-címe teljes tartománynevét (FQDN). Ebben a cikkben <a HREF="#BizTalk">létrehozása BizTalk szolgáltatás</a> témakörben találhat.</li>
</ul>
<br/>
Egy új vagy eltérő tanúsítvány felvehetők a BizTalk szolgáltatás létrehozását követően.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hibrid kapcsolatok

Az Azure BizTalk szolgáltatásainak létrehozásakor a **Hibrid kapcsolatok** lapon érhető el:

![Hibrid kapcsolatok lapon][HybridConnectionTab]

Hibrid kapcsolatok erőforráshoz való kapcsolódáshoz az Azure webhelyén vagy a Azure mobilszolgáltatás bármely helyszíni, amely egy statikus portot használja, például SQL Server, MySQL, HTTP webes API-hoz, Mobile szolgáltatásokat és a legtöbb egyéni webszolgáltatásokhoz használják.  Hibrid kapcsolatok és a BizTalk kártya szolgáltatás eltérőek. A BizTalk kártya szolgáltatásával rendszerhez való csatlakozáshoz Azure BizTalk Services egy helyszíni üzleti vonal (üzleti).

 Lásd: [Hibrid kapcsolatok](integration-hybrid-connection-overview.md) további hozhat létre és kezelhet hibrid kapcsolatok együtt.


## <a name="next-steps"></a>Következő lépések

Most, hogy egy BizTalk szolgáltatás hoz létre, olvassa el a különböző [BizTalk szolgáltatások: irányítópult, a Monitor és a méret lapon](biztalk-dashboard-monitor-scale-tabs.md). A BizTalk szolgáltatás nem készen áll az alkalmazások számára. Indítsa el az alkalmazások létrehozása, keresse fel az [Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Lásd még:
- [BizTalk szolgáltatások: Kiadásai diagram](biztalk-editions-feature-chart.md)<br/>
- [BizTalk szolgáltatások: Állapot diagram](biztalk-service-state-chart.md)<br/>
- [BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](biztalk-backup-restore.md)<br/>
- [BizTalk szolgáltatások: szabályozása](biztalk-throttling-thresholds.md)<br/>
- [BizTalk szolgáltatások: Kibocsátó neve és a kibocsátó billentyűk](biztalk-issuer-name-issuer-key.md)<br/>
- [Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hibrid kapcsolatok](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
