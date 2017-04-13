<properties
    pageTitle="Azure API-kezelés – gyakori kérdések |} Microsoft Azure"
    description="További tudnivalók a gyakori kérdésekre, a mintázatok és a gyakorlati tanácsok az Azure API-kezelés adott válaszok."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure API-kezelés gyakori kérdések

Válaszok a gyakori kérdésekre, a mintázatok és a gyakorlati tanácsok az Azure API-kezelés.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

-   [Hogyan lehet lehet kérdés feltevése a Microsoft Azure API-kezelés csoport?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Mit jelent az, ha egy funkció előzetes verzióban?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Hogyan lehet biztonságos a kapcsolat az API az adatkezelési átjáró és a háttér-szolgáltatások között?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Hogyan az API Management szolgáltatás példányának másolása egy új példányát?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Lehet kezelni az API-kezelés példány programozás útján?](#can-i-manage-my-api-management-instance-programmatically)
-   [Hogyan vehetek fel felhasználókat a Rendszergazdák csoport?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Miért jelenik meg a házirendet, hogy hogyan lehet hozzáadni nem érhető el a csoportházirend-szerkesztőben?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Hogyan használhatom a verziószámozás API API-kezelés?](#how-do-i-use-api-versioning-in-api-management)
-   [Hogyan állíthatom be az egy API több környezete?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Használhatom-e SOAP API-kezelés?](#can-i-use-soap-with-api-management)
-   [Az API adatkezelési átjáró IP cím állandó? Van lehetőség, a tűzfal szabályok?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Is egy OAuth 2.0 engedélyezési kiszolgáló is konfigurálása az Active Directory összevonási szolgáltatások biztonsági?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Milyen útválasztási módszert használni a API kezelése a környezetekben több földrajzi helyekre?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Az erőforrás-kezelő Azure-sablon segítségével hozza létre az API Management szolgáltatás?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Használhatom-e önaláírt SSL-tanúsítványt egy vissza befejezési?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Miért kapok hitelesítési hiba jelenik meg egy mely Számjegy tárházba klónozhatja?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [API kezelése a készült Azure ExpressRoute működik?](#does-api-management-work-with-azure-expressroute)
-   [Áthelyezhetők-e egy API alkalmazáskezelési szolgáltatás egyik előfizetésről a másikra?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Hogyan lehet lehet kérdés feltevése a Microsoft Azure API-kezelés csoport?

Kapcsolatfelvétel az alábbi lehetőségek közül:

-   A kérdések feltevése: az [API-kezelés MSDN-fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Küldés e-mailben <apimgmt@microsoft.com>.
-   Küldjön egy szolgáltatás kérése az [Azure Visszajelzési fórum](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Mit jelent az, ha egy funkció előzetes verzióban?

Előnézeti funkció esetén az azt jelenti, hogy azt aktívan használja kérő véleményezhessék hogyan meg a szolgáltatás működik. Az előzetes verzió egyik funkciójáról funkcionális teljes, de lehetséges, hogy egy visszajelzései változása szakítószilárdságának fogja VÁLLALUNK. Azt javasoljuk, hogy Ön nem attól függenek, egy funkció, amely a üzemi környezetben előzetes verzióban. Ha esetleges visszajelzéseit a előzetes verzió szolgáltatások, tudassa velünk a kapcsolattartási beállítások közül – [hogyan is lehet a Microsoft Azure API-kezelés csoport kérdések feltevése?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Hogyan lehet biztonságos a kapcsolat az API az adatkezelési átjáró és a háttér-szolgáltatások között?

Az API az adatkezelési átjáró és a háttéradatbázist szolgáltatások közötti kapcsolat biztonságossá tétele több lehetőség közül választhat. képes vagy:

-   Alapszintű hitelesítés HTTP használja. További információért tekintse meg [konfigurálása API-beállításait](api-management-howto-create-apis.md#configure-api-settings).
- Használja a kölcsönös SSL-hitelesítés, [biztonságos háttéradatbázist szolgáltatások ügyfél Azure API-kezelés tanúsítvány-hitelesítés használatával hogyan](api-management-howto-mutual-certificates.md)ismertetett módon.
- IP-whitelisting használja a háttéradatbázist szolgáltatásban. Ha egy normál vagy prémium réteg API-kezelés-példányt, az átjáró IP-címe állandó marad. Beállíthatja, hogy a whitelist engedélyezi az IP-cím. Az IP-címet az API-kezelési-példányok elérheti az irányítópulton az Azure-portálon.
- Csatlakozás az API-kezelés példányt az Azure virtuális hálózat. További információért megtudhatja, [hogy miként állíthatja be a VPN-kapcsolatok Azure API-kezelés](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Hogyan az API Management szolgáltatás példányának másolása egy új példányát?

Ha szeretne egy új példányát az API-kezelés példány másolása több lehetőség közül választhat. képes vagy:

-   A biztonsági mentés és visszaállító API-kezelés függvény. További információért megtudhatja, [hogy miként vészhelyreállítás szolgáltatás biztonsági másolat segítségével megvalósítása és Azure API-kezelés visszaállítása](api-management-howto-disaster-recovery-backup-restore.md).
-   A saját biztonsági másolat létrehozása és szolgáltatás visszaállítása az [API Management REST API -t](https://msdn.microsoft.com/library/azure/dn776326.aspx). A REST API segítségével mentése és visszaállítása a személyek, amelyet a szolgáltatás példányból.
-   Töltse le a szolgáltatás beállításai mely számjegy használatával, és töltse fel egy új példányát. További információért megtudhatja, [hogy miként mentse, és állítsa be a API-kezelés konfigurációjának mely számjegy használatával](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Lehet kezelni az API-kezelés példány programozás útján?

Igen, kezelheti API-kezelés programozás útján használatával:

-   Az [API-kezelés REST API-t](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   A [Microsoft Azure ApiManagement szolgáltatás könyvtár SDK csomagjában talál](http://aka.ms/apimsdk).
-   A [szolgáltatás telepítési](https://msdn.microsoft.com/library/mt619282.aspx) és [szolgáltatás kezelése](https://msdn.microsoft.com/library/mt613507.aspx) a PowerShell-parancsmagok.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Hogyan vehetek fel felhasználókat a Rendszergazdák csoport?

Az alábbiakban hogyan fel a felhasználókat a rendszergazdák csoportba:

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Nyissa meg az erőforráscsoport, amelynek a frissíteni kívánt API-kezelés példányt.
3. A API-kezelés az **Api-kezelés munkatársi** szerepkörök hozzárendelése a felhasználóhoz.

Ezután az újonnan hozzáadott közös munka az Azure PowerShell- [parancsmagok](https://msdn.microsoft.com/library/mt613507.aspx)használhatja. Az alábbi módszerrel jelentkezzen be rendszergazdaként:

1. Használja a `Login-AzureRmAccount` parancsmag bejelentkezni.
2. A környezet beállítása az előfizetéshez, amelynek a szolgáltatás használatával `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Egyszeri bejelentkezés az URL-CÍMEK első használatával `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Az URL-címet használhatja a felügyeleti portálon eléréséhez.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Miért jelenik meg a házirendet, hogy hogyan lehet hozzáadni nem érhető el a csoportházirend-szerkesztőben?

Ha a házirendet, amely a hozzáadni kívánt halványan jelenik meg, vagy a csoportházirend-szerkesztőben árnyékolt, lehet, hogy helyes-e a házirend hatálya alá nem jelenik meg. Egyes házirendi szolgál, hogy az adott tartományok és a házirend szakaszok használata. A házirend szakaszokat és a keresési tartományok házirend című a házirend használata című szakaszában található [API adatkezelési házirendek](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Hogyan használhatom a verziószámozás API API-kezelés?

API a verziószámozás használata API-kezelés néhány lehetőség közül választhat:

-   API-kezelés különböző verzióival ábrázolásához API-khoz is beállíthatja. Előfordulhat például, két különböző API-hoz, MyAPIv1 és MyAPIv2. A Fejlesztőeszközök verziót használni szeretné a fejlesztői választhat.
-   Is beállíthatja az API egy szolgáltatás URL-címmel, amelyek nem tartalmaznak verzió szegmens, például https://my.api. Ezután konfigurálhatja az egyes műveletek [Átírásának URL-cím](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) sablon olyan verziójú szakasz. Például egy műveletet is /resource és /v1/Resource néven [URL-cím Átírásának](api-management-howto-add-operations.md#rewrite-url-template) sablon című [sablon URL-CÍMÉT](api-management-howto-add-operations.md#url-template) . Módosíthatja a verzió szakasz értéket külön-külön az egyes műveletek.
-   Ha szeretné tartani a "alapértelmezés" verzió szakaszában az API-szolgáltatás URL-CÍMÉT, kijelölt műveletek, állított be egy házirendet, amely a [kódmentes szolgáltatás beállítása](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) házirend használja a háttéradatbázist kérelem elérési út módosítása.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Hogyan állíthatom be az egy API több környezete?

Beállításához több környezetben, például egy tesztkörnyezetben és munkakörnyezetben, egy API, ha két lehetőség közül választhat. képes vagy:

-   A Host különböző API-hoz a ugyanahhoz a bérlőhöz.
-   A különböző bérlők azonos API-khoz állomásnév.

### <a name="can-i-use-soap-with-api-management"></a>Használhatom-e SOAP API-kezelés?

[SOAP átadó](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) támogatási most érhető el. A rendszergazdák importálhatja a WSDL azok SOAP szolgáltatás, és Azure API szolgáltatás egy SOAP előtér hoz létre. Developer portal dokumentáció, vizsgálatot konzol, házirendek és analytics érhetők el az összes SOAP szolgáltatásokhoz.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Az API adatkezelési átjáró IP cím állandó? Van lehetőség, a tűzfal szabályok?

A szokásos és prémium rétegek a nyilvános IP-címe (virtuális) az API-kezelés bérlői nem statikus a bérlői, néhány kivétellel élettartama. Az IP-cím változását, az alábbi esetekben:

-   A szolgáltatás törlődik, és ezután újra létre.
-   Az előfizetése felfüggesztett (például nonpayment), és ezután visszaállítása.
-   Hozzáadása vagy eltávolítása az Azure virtuális hálózati (használhat virtuális hálózati csak a prémium réteg).

Több elem régió telepítésekhez a regionális címet változik, ha a régió vacated, és kattintson az engedély (használhat több területre telepítési csak a prémium réteg).

Több területre telepítéshez konfigurált prémium réteg bérlők kapnak, hogy egy nyilvános IP-cím / régió.

Az IP-címe (vagy, több területre környezetben) elérheti a bérlői lapon az Azure-portálon.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Is egy OAuth 2.0 engedélyezési kiszolgáló is konfigurálása az Active Directory összevonási szolgáltatások biztonsági?

Egy OAuth 2.0 engedélyezési kiszolgáló konfigurálása az Active Directory összevonási szolgáltatások (AD FS) biztonsági című témakörben talál [Használatával ADFS API-kezelés](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Milyen útválasztási módszert használni a API kezelése a környezetekben több földrajzi helyekre?

API-kezelés telepítések több földrajzi helyekre a [Teljesítmény forgalom útválasztási módszert](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) használja. Bejövő forgalmat a legközelebbi API átjáró rendszer irányítja. Egy terület offline állapotba kerül, ha a rendszer automatikusan bejövő forgalom irányítja a következő legközelebb átjáró. További tudnivalók a [forgalom Manager](../traffic-manager/traffic-manager-routing-methods.md)költségszámítás módszerek útválasztási módszereket.

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Az erőforrás-kezelő Azure-sablon segítségével hozza létre az API Management szolgáltatás?

igen. Lásd: az [Azure API alkalmazáskezelési szolgáltatás](http://aka.ms/apimtemplate) quickstart útmutató sablonok.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Használhatom-e önaláírt SSL-tanúsítványt egy vissza befejezési?

igen. Íme egy vissza vége önaláírt tanúsítvány Secure Sockets Layer (SSL) használata:

1. Hozzon létre egy [Kódmentes](https://msdn.microsoft.com/library/azure/dn935030.aspx) entitás API-kezelés használatával.
2. A **skipCertificateChainValidation** tulajdonság **Igaz**.
3. Ha már engedélyezni szeretné önaláírt tanúsítványok, az Kódmentes entitás törlése vagy a **skipCertificateChainValidation** tulajdonság **értéke hamis**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Miért kapok hitelesítési hiba jelenik meg egy mely számjegy tárházba klónozhatja?

Mely számjegy hitelesítőadat-kezelő használatakor, vagy ha szeretné, hogy mely számjegy összegyűjti klónozhatja Visual Studio segítségével, mutatjuk be a Windows hitelesítő adatok párbeszédpanelen az ismert probléma. A párbeszédpanel korlátozza jelszóhossz 127 karaktereket, és azt levágja a Microsoft által létrehozott jelszót. Dolgozunk a térközszakaszok rövidítése a jelszót. Most használjon mely számjegy Bash a mely számjegy tárházba klónozhatja.

### <a name="does-api-management-work-with-azure-expressroute"></a>API kezelése a készült Azure ExpressRoute működik?

igen. API-kezelési algoritmusával a készült Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Áthelyezhetők-e egy API alkalmazáskezelési szolgáltatás egyik előfizetésről a másikra?

igen. Megtudhatja, hogyan, lásd: az [erőforrások új erőforráscsoport vagy-előfizetésre áthelyezése](../resource-group-move-resources.md).
