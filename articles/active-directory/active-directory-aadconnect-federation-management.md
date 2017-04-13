<properties
    pageTitle="Az Active Directory összevonási szolgáltatások kezelése és testreszabási az Azure AD Connect |} Microsoft Azure"
    description="Active Directory összevonási szolgáltatások kezelése Azure AD Connect és a felhasználó Active Directory összevonási szolgáltatások bejelentkezés módja az Azure AD Connect és PowerShell testreszabását."
    keywords="Active Directory összevonási szolgáltatások, ADFS, az AD FS-kezelés AAD csatlakozni, csatlakozás, bejelentkezés, az AD FS testreszabás, adatvédelmi, az Office 365-ös, összevonási, megbízó fél javítása"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory összevonási szolgáltatások szolgáltatás és testreszabási az Azure AD Connect

Ez a cikk részletes feladatok az Active Directory összevonási szolgáltatások (AD FS), amelyek segítségével a Microsoft Azure Active Directory csatlakozni lehet végrehajtani, valamint az egyéb gyakori AD FS-feladatok, amely a teljes konfiguráció AD FS-farm szükség lehet kapcsolódik.

| A témakör | Mire vonatkozik |
|:------|:-------------|
|**Active Directory összevonási szolgáltatások kezelése**|
|[Az adatvédelmi javítása](#repairthetrust)| Az Office 365-tel az összevonási megbízhatósági javítása |
|[Az AD FS-kiszolgáló hozzáadása](#addadfsserver) | Az Active Directory összevonási szolgáltatások farm további AD FS-kiszolgálóval kibontása|
|[Az Active Directory összevonási szolgáltatások webes alkalmazás proxykiszolgáló hozzáadása](#addwapserver) | Az Active Directory összevonási szolgáltatások farm egy további WAP kiszolgálón kibontása|
|[A szövetséges tartományban hozzáadása](#addfeddomain)| A szövetséges tartományban hozzáadása|
| **Active Directory összevonási szolgáltatások testreszabása**|
|[Adjon hozzá egy egyéni cégemblémával vagy ábra](#customlogo)| Az Active Directory összevonási szolgáltatások bejelentkezési lapja egy vállalati embléma és ábra testreszabása |
|[A bejelentkezési leírásának megadása](#addsignindescription) | Bejelentkezési lapja leírás hozzáadása |
|[Active Directory összevonási szolgáltatások állítást szabályok módosítása](#modclaims) | Active Directory összevonási szolgáltatások követelések különböző összevonási felhasználási területei módosítása |

## <a name="ad-fs-management"></a>Active Directory összevonási szolgáltatások kezelése

Azure AD Connect változatos műveleteket is elvégezheti az Azure AD Connect varázsló segítségével a minimális felhasználói beavatkozás Active Directory összevonási szolgáltatások kapcsolódó biztosít. Azure AD Connect telepítése a varázsló befejezése után futtatását is lehetővé teszi a varázsló ismételt további feladatokat.

### Az adatvédelmi javítása<a name=repairthetrust></a>

Azure AD Connect az Active Directory összevonási szolgáltatások és Azure Active Directory adatvédelmi aktuális állapotának ellenőrzése és a megfelelő műveletet a meghatalmazás kijavíthatja. Hajtsa végre az alábbi lépéseket követve javítsa ki az Azure Active Directory és az Active Directory összevonási szolgáltatások adatvédelmi.

1. Jelölje ki a **javítás AAD és ADFS megbízhatónak** a feladatok listájából.
![Javítás AAD és ADFS megbízható](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. A **Csatlakozás az Azure Active Directory** lapon adja meg a globális rendszergazdai hitelesítő adatait az Azure Active Directory, és kattintson a **Tovább**gombra.
![Azure Active Directory csatlakoztatása](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. **Távoli hozzáférés hitelesítő adatait** lapon adja meg a hitelesítő adatokat a tartományi rendszergazdája.
![Távoli hozzáférés hitelesítő adatok](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Gombra kattintás után **következő**Azure AD Connect tanúsítvány állapot ellenőrzése, és megjelenítése kapcsolatos problémák megoldásához.

    ![A tanúsítványok állam](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    A **kész konfigurálása** lapon megjelenik a kijavíthatja a meghatalmazás elvégzendő műveletek listájának.

    ![Készen áll a konfigurálása](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Kattintson a **telepítse** kijavíthatja a megbízható gombra.

>[AZURE.NOTE] Azure AD Connect lehet egyetlen helyreállítás vagy az önaláírt tanúsítványok törvény. Azure AD Connect-tanúsítványok nem tudja helyreállítani.

### Az AD FS-kiszolgáló hozzáadása<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD Connect hozzáadása az AD FS-kiszolgáló PFX biztonságitanúsítvány-fájl szükséges. Ezért a művelet végrehajtása, csak akkor, ha az AD FS-farm konfigurált Azure AD Connect használatával.

1. Jelölje ki a **Deploy további összevonási kiszolgáló** , és kattintson a **Tovább**gombra.
![További összevonási kiszolgáló](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. A **Csatlakozás az Azure Active Directory** lapon adja meg a globális rendszergazdai hitelesítő adatait az Azure Active Directory, és kattintson a **Tovább**gombra.
![Azure Active Directory csatlakoztatása](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Adja meg a tartomány rendszergazdai hitelesítő adatokat.
![Tartomány rendszergazdai hitelesítő adatok](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD Connect kéri a jelszót az új Active Directory összevonási szolgáltatások farm konfigurálása az Azure AD Connect közben megadott PFX fájl. **Írja be jelszavát** a jelszó megadása a PFX fájl gombjára.
![Tanúsítvány jelszava](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Adja meg az SSL-tanúsítvány](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. **Az AD FS-kiszolgálók** lapján adja meg a kiszolgáló neve megjelenjen az AD FS-farm az IP-címet.
![Active Directory összevonási szolgáltatások kiszolgálón](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Kattintson a **Tovább** gombra, és hajtsa végre **a végleges lap** . Azure AD Connect van hozzáadását követően a kiszolgálókat az AD FS-farmba, akkor kap ellenőrizheti a kapcsolatot.
![Készen áll a konfigurálása](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![A telepítés befejezése](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Az Active Directory összevonási szolgáltatások webes alkalmazás proxykiszolgáló hozzáadása<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD Connect hozzáadása a webes alkalmazás proxykiszolgáló PFX biztonságitanúsítvány-fájl szükséges. Ezért lesz a művelet végrehajtása, csak akkor, ha az AD FS-farm konfigurált Azure AD Connect használatával.

1. Válassza ki a rendelkezésre álló feladatlistából **Üzembe webes szolgáltatásalkalmazás-Proxy** .
![Webes szolgáltatásalkalmazás-proxy terjesztése](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Adja meg az Azure globális rendszergazdai hitelesítő adatait.
![Azure Active Directory csatlakoztatása](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Az **Adja meg az SSL-tanúsítvány** lapon adja meg a jelszót az Azure AD Connect konfigurálása az Active Directory összevonási szolgáltatások farm megadott PFX fájl.
![Tanúsítvány jelszava](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Adja meg az SSL-tanúsítvány](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Adja hozzá a kiszolgáló egy webes szolgáltatásalkalmazás-proxy hozzáadni. Előfordulhat, hogy a webes alkalmazás proxykiszolgáló nem csatlakozik a tartományt, mert a varázsló kéri a kiszolgáló felvételről rendszergazdai hitelesítő adatokkal.
![A kiszolgáló rendszergazdai hitelesítő adatai](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. A **Proxy adatvédelmi hitelesítő adatait** lapon adja meg a proxy meghatalmazás beállítása, és az elsődleges a farmban AD FS-kiszolgáló elérése rendszergazdai hitelesítő adatokkal.
![A proxykiszolgáló adatvédelmi hitelesítő adatok](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. **Készen áll a konfigurálása** lapon a varázsló elvégzendő művelet listáját jeleníti meg.
![Készen áll a konfigurálása](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Kattintson a **telepítse** a konfiguráció befejezéséhez. Miután a konfigurálás befejeződött, a varázslóban többféle ellenőrizheti a kiszolgálók kapcsolatot. Kattintson az **ellenőrzés** csatlakozási gombra.
![A telepítés befejezése](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### A szövetséges tartományban hozzáadása<a name=addfeddomain></a>

Nagyon egyszerűen Azure AD Connect használatával összevonni Azure AD a tartomány hozzáadása. Azure AD Connect hozzáadja az összevonáshoz a tartományt, és a kibocsátó megfelelően megfelelően, amikor több tartományt az Azure Active Directory összevont állítást szabályok módosítja.

1. A szövetséges tartományban hozzáadásához jelölje ki a tevékenységet **felvesz egy további Azure Active Directory tartományi**.
![További Azure Active Directory tartományi](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. A varázsló következő lapján az Azure Active Directory adja meg a globális rendszergazdai hitelesítő adatait.
![Azure Active Directory csatlakoztatása](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. A **távoli hozzáférés hitelesítő adatait** lapon adja meg a tartomány rendszergazdai hitelesítő adatokat.
![Távoli hozzáférés hitelesítő adatok](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. A következő lapon a varázsló, amellyel a helyszíni címtárában is kapcsolt Azure AD-tartományok listájának nyújt. Válassza ki a tartományt a listából.
![Azure Active Directory tartományi](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Miután úgy dönt, hogy a tartomány, a varázsló szolgáltatja kapcsolatban további műveleteket a varázsló lesz, és milyen következményekkel járnak a konfiguráció szükséges adatokat. Egyes esetekben, jelöljön ki egy tartományt még nem ellenőrizni Azure AD, ha a varázsló szolgáltatja segítséget nyújt a tartomány ellenőrzéséhez hivatkozásra. További információt talál [az Azure Active Directory egyéni tartománynév hozzáadása](active-directory-add-domain.md) .

5. Kattintson a **Tovább gombra**, és az **készen áll a konfigurálása** lapon megjelenik a lista műveleteket, amelyek az Azure AD Connect hajt végre. Kattintson a **telepítse** a konfiguráció befejezéséhez.
![Készen áll a konfigurálása](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>Active Directory összevonási szolgáltatások testreszabása

A következő szakaszokban a gyakori feladatokat, végezze el az Active Directory összevonási szolgáltatások bejelentkezési lapja testreszabásakor is olvashat.

### Adjon hozzá egy egyéni cégemblémával vagy ábra<a name=customlogo></a>

Az embléma a **bejelentkezési** lapon megjelenő cég beállításához használja a Windows PowerShell-parancsmagot és szintaxis.

> [AZURE.NOTE] A javasolt méretét az embléma 260 x 35 @ nem nagyobb 10 KB fájlméretű 96 dpi.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] A *Cél_neve* paraméter szükség. Az alapértelmezett téma az AD FS kiadott alapértelmezett neve.


### A bejelentkezési leírásának megadása<a name=addsignindescription></a>

A bejelentkezési lapja leírását a **bejelentkezési lapja**hozhatja létre a Windows PowerShell-parancsmagot és szintaxis.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### Active Directory összevonási szolgáltatások állítást szabályok módosítása<a name=modclaims></a>

Active Directory összevonási szolgáltatások egy egyéni állítást szabályok létrehozásához használható gazdag állítást nyelvet támogat. További tudnivalókért lásd: [A szerepkör a szabályhoz állítás nyelv](https://technet.microsoft.com/library/dd807118.aspx).

Az alábbi szakaszok ismertetik, hogyan írhat egyéni bizonyos esetekben Azure Active Directory vonatkozó szabályok és az Active Directory összevonási szolgáltatások összevonási.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Egy értéket a attribútumban címtárra feltételes megváltoztatható azonosító

Azure AD Connect egy adatforrás horgony helyőrzőként, amikor objektumok a rendszer szinkronizálja az Azure Active Directory-attribútum megadását teszi lehetővé. Ha az érték a egyéni attribútumból nem üres, érdemes lehet egy megváltoztatható azonosító állítás hiba. Előfordulhat, hogy például, jelölje be a forrás horgony a attribútum **ms-ds-consistencyguid** , és szeretné **ImmutableID** kibocsátás, **az ms-ds-consistencyguid** , abban az esetben, ha az attribútum szemben értéke. Ha az attribútum ellen nincs érték, **objectGuid** hiba, a megváltoztatható azonosítóját.  A következő szakaszban ismertetett módon igénylése egyéni szabálykészlet állíthat össze.

**Szabály 1: Lekérdezési attribútumok**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Ez a szabály lekérdezett **objectGuid** a felhasználó Active Directoryból és **az ms-ds-consistencyguid** értékeket. A tár nevére a AD FS-példányban-megfelelő tár nevének módosítása Módosíthatja, hogy követelések milyen az összevonáshoz **objectGuid** és **az ms-ds-consistencyguid**meghatározott megfelelő követelések típusát.

Is **hozzáadása** és a **probléma**, akkor ne vegyen fel egy kimenő problémát az entitás és a köztes értékek az értékek használata. A későbbi szabály kárigény adnak, melyik értéket kell használni a megváltoztatható azonosító létrehozása után

**Szabály 2: Ms-ds-consistencyguid a felhasználó létezésének ellenőrzése**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Ez a szabály **idflag** **useguid** értéke nincs **ms-ds-concistencyguid** töltve a felhasználó esetén úgynevezett ideiglenes jelző határozza meg. Ez mögött meghúzódó logika arra, hogy az Active Directory összevonási szolgáltatások nem engedélyezi a üres jogcímeken. Így hozzáadásakor jogcímeken http://contoso.com/ws/2016/02/identity/claims/objectguid és http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid szabály 1, akkor végződnek be egy **msdsconsistencyguid** formál csak ha az érték van töltve a felhasználó számára. Nem nem üres, ha az AD FS láthatja, hogy egy üres érték lesz, és beilleszti azt azonnal. Az összes objektum **objectGuid**, lesz, így az igény mindig lesznek szabály 1 végrehajtása után.

**Szabály 3: Megváltoztatható ID ki az ms-ds-consistencyguid, ha jelen**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Ez a egy implicit **található** jelölőnégyzetet. Ha létezik a állítást értékét, majd hiba, mint a megváltoztatható azonosítóját. Az előző példában **nameidentifier** igényt. Be kell a környezetben megváltoztatható azonosító módosítsa az a megfelelő állítást típusát.

**Szabály 4: Megváltoztatható ID ki objectGuid, ha nem szerepel a ms-ds-consistencyGuid**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

Ez a szabály az ideiglenes jelző **idflag**ellenőrzése egyszerűen vannak. Úgy dönt, hogy az az érték alapján állítást kiadását.

> [AZURE.NOTE] Fontos a szabályok sorrendjét.

#### <a name="sso-with-a-subdomain-upn"></a>Egyszeri bejelentkezés, és altartományt (UPN)

Azure AD Connect leírtak [szövetséges új tartomány felvétele](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain)segítségével az összevonáshoz egynél több tartományt is felvehet. Módosítania kell az egyszerű Felhasználónévi állítást, hogy a kibocsátó azonosító felel meg a legfelső szintű tartomány és nem a altartomány, mivel a szövetséges gyökértartomány is foglalkozik a gyermek.

Alapértelmezés szerint a kibocsátó azonosító állítást szabály van beállítva:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Alapértelmezett kibocsátó azonosító állítást](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Az alapértelmezett szabály egyszerűen UPN-utótag tart, és használja a kibocsátó azonosító kárigény. Például János sub.contoso.com a felhasználó, és az Azure Active Directory összevont contoso.com. Belép az adott john@sub.contoso.com bejelentkezés az Azure Active Directory, miközben a felhasználó nevét, és az alapértelmezett kibocsátó Azonosítóját formál szabályt az Active Directory összevonási szolgáltatások a következő módon kezeli.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Formál érték:** http://sub.contoso.com/adfs/services/trust/

Ahhoz, hogy csak a gyökértartomány a kibocsátó állítást értéket, úgy módosítja a állítást szabályt az alábbi megfelelően.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [felhasználói bejelentkezési beállításait](active-directory-aadconnect-user-signin.md).
