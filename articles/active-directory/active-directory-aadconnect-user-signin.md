<properties
    pageTitle="Azure AD Connect: Felhasználónak bejelentkezés |} Microsoft Azure"
    description="Azure AD Connect felhasználónak bejelentkezés az egyéni beállításokat."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure Active Directory csatlakozás bejelentkezési beállításai

Azure AD Connect lehetővé teszi a felhasználóknak, hogy jelentkezzen be felhő és a helyszíni erőforrásoknak ugyanazt a jelszót. Ez a cikk ismerteti a alapfogalmak minden identitás modell segít azonosítója, a bejelentkezési kísérleteket Azure Active Directory használni szeretne.

Ha már régóta Azure AD-identitás modell, és szeretné, ha többet szeretne tudni az egy adott módszer, kattintson alább a megfelelő témakört.

* [Jelszó-szinkronizálás](#password-synchronization)
* [Szövetségi SSO (ADFS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>A felhasználó bejelentkezési módszer kiválasztása
A legtöbb szervezet számára, akik csak a felhasználó engedélyezni szeretné jelentkezzen be az Office 365-ben, szoftver-alkalmazások és más Azure AD-alapú erőforrásokat, az alapértelmezett jelszó-szinkronizálás beállítás ajánlott.
Egyes szervezetek, azonban van egy összevont bejelentkezési használata lehetőséget, például az Active Directory összevonási szolgáltatások adott okait.  Ezek a következők:

- Szervezetének már van-e az AD FS, illetve egy rendszerbe összevonási szolgáltató fél 3rd
- A Yammer biztonsági házirendje megakadályozza, hogy a jelszó tiltva a felhőbe szinkronizálása
- Hogy a felhasználók (nélkül kér, további jelszó) tapasztalható zökkenőmentes egyszeri bejelentkezés esetén szükséges a vállalati hálózathoz gépek elérése a felhőben erőforrások tartomány az illesztés
- Néhány adott lehetőségeket AD FS van szüksége
    - A helyszíni többtényezős hitelesítés külső szolgáltatóról vagy az intelligens kártya (További tudnivalók a harmadik féltől származó MFA szolgáltatók Windows Server 2012 R2 rendszer AD FS)
    - Az Active Directory integrációs szolgáltatások például finom fiók zárolása vagy az AD-jelszó és a munkavégzés óra házirend
    - Mindkét feltételes hozzáférést a helyszíni és az eszköz regisztrációs, Azure AD-csatlakozás vagy Intune MDM házirendek erőforrások cloud

### <a name="password-synchronization"></a>A jelszó-szinkronizálás
A jelszó-szinkronizálás felhasználói jelszavak tiltva a helyszíni Active Directoryból az Azure ad meg vannak szinkronizálva.  Ha megváltozott vagy helyszíni alaphelyzetbe jelszavakat, az új jelszót a rendszer szinkronizálja közvetlenül az Azure Active Directory, hogy a felhasználók mindig használhatja ugyanazt a jelszót felhő erőforrások mint a helyszíni.  A jelszavak soha nem küldött Azure Active Directory sem Azure Active Directory egyszerű szövegként tárolt.
A jelszó-szinkronizálás engedélyezése az önkiszolgáló szolgáltatás jelszó alaphelyzetbe állítása az Azure Active Directory jelszó írási-back együtt használható.

<center>![Felhőalapú](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[További információt a jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Egy új vagy meglévő Active Directory összevonási szolgáltatások használata a Windows Server 2012 R2 farm összevonás
A szövetséges jellel a felhasználók jelentkezhet Azure AD-alapú szolgáltatások helyszíni jelszavukat, és kattintson a vállalati hálózathoz, anélkül, hogy adja meg újra a jelszavukat.  Az Active Directory összevonási szolgáltatások összevonási beállítás lehetővé teszi üzembe egy új, vagy adjon meg egy meglévő Active Directory összevonási szolgáltatások Windows Server 2012 R2 farmban.  Ha úgy dönt, hogy egy már létező farm adja meg, Azure AD Connect konfigurálása a meghatalmazás között a farm és Azure Active Directory, így a felhasználók jelentkezhetnek.

<center>![Felhőalapú](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>A Windows Server 2012 R2 rendszer AD FS összevonási üzembe helyezéséhez lesz az alábbiakra van szükség

Ha új farm telepít:

- A Windows Server 2012 R2 kiszolgálója az összevonási kiszolgáló.
- A webes alkalmazás proxy Windows Server 2012 R2 kiszolgáló.
- .pfx fájl az SSL-tanúsítvány a tervezett összevonási szolgáltatás neve, például fs.contoso.com.

Ha telepíti az új farm vagy egy már létező farm használata:

- Az összevonási kiszolgálókon rendszergazdai hitelesítő adataival.
- Rendszergazdai hitelesítő adataival bármely munkacsoport (tartományon kívüli csatlakozott) kiszolgálók, amelyen létre szeretné a webes alkalmazás Proxy szerepkör telepíteni.
- A számítógépen, amelyen a varázsló végrehajtása tud csatlakozni a bármely más gépeken futó AD FS vagy a webes alkalmazás proxykiszolgáló Windows távfelügyelet keresztül telepíteni szeretné kell lennie.

[Egyszeri bejelentkezés konfigurálása az Active Directory összevonási szolgáltatások](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Jelentkezzen az AD FS régebbi verzióját használja, vagy egy harmadik fél megoldás

Ha van már konfigurált felhő bejelentkezési Active Directory összevonási szolgáltatások (például az AD FS 2.0) vagy egy külső összevonási szolgáltató régebbi verzióját használja, válassza a ugorja át a felhasználónak bejelentkezés konfigurációs Azure AD Connect keresztül.  Ezzel biztosíthatja, hogy a legújabb szinkronizálás és egyéb funkciók: Azure AD Connect első miközben használják a meglévő megoldás bejelentkezési a.

[Azure Active Directory összevonási külső kompatibilitási listája](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Felhasználói bejelentkezési és egyszerű felhasználónév (UDN)

### <a name="understanding-user-principal-name"></a>Tudnivalók az egyszerű felhasználónév

Az Active Directory az alapértelmezett UPN-utótag, amelyben felhasználói fiók létrehozása a tartomány DNS-nevét. A legtöbb esetben ez az a tartomány nevét, az interneten a vállalati tartomány azonosítóként regisztrált. További UPN-utótag használja az Active Directory – tartományok és megbízhatósági kapcsolatok elemet is hozzáadhat.

A felhasználó (UPN) van, a formátum username@domai. Például az active directory contoso.com felhasználói János lehet UPN john@contoso.com. A felhasználó (UPN) RFC 822 alapul. Bár UPN és e-mail megosztani ugyanazt a formátumot, a felhasználó egyszerű Felhasználónévi értékének előfordulhat, hogy, vagy nem lehet egyenlő annak a felhasználónak az e-mail címét.

### <a name="user-principal-name-in-azure-ad"></a>Egyszerű felhasználónév az Azure Active Directory

Azure AD Connect varázsló használata a userPrincipalName attribútum vagy megadhatja a attribútum (az egyéni telepítés) helyőrzőként a helyszíni egyszerű felhasználónév az Azure Active Directory lesz. Ez az érték bejelentkezni az Azure Active Directory lesz. Ha a felhasználó egyszerű felhasználónév attribútum értékének igazolt tartományt az Azure Active Directory nem felel meg, majd Azure Active Directory kicserélheti egy alapértelmezett. onmicrosoft.com értéket.

A beépített tartománynevet, amellyel az űrlap contoso.onmicrosoft.com megtalálható minden olyan könyvtárban az Azure Active Directory Azure vagy más Microsoft-szolgáltatások használatának megkezdéséhez. Javíthatja a és leegyszerűsíti a bejelentkezés módja egyéni tartományokat használ. Bővebben az egyéni tartomány nevét az Azure Active Directory és a ellenőrzése a tartományt, olvassa el [az Azure Active Directory egyéni tartománynév hozzáadása](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure Active Directory bejelentkezési konfiguráció

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure Active Directory bejelentkezési konfigurációban Azure AD Connect
Azure Active Directory bejelentkezés módja függ, hogy képes a szinkronizálás alatt áll, az egyik ellenőrizni az Azure Active directory-ban az egyéni tartományok felhasználó UPN-utótag megfelelően van-e Azure AD. Azure AD Connect biztosít a súgót, amikor az Azure Active Directory bejelentkezési beállításokkal kapcsolatosan, hogy a felhasználónak bejelentkezés módja felhőben hasonlóan a helyszíni. Azure AD Connect listája az a tartomány megadott egyszerű Felhasználónévi utótag és egyeztet őket az egyéni tartománynevet az Azure Active Directory, és a megfelelő műveletet kell tenni, hogy a nyújt segítséget.
Az Azure Active Directory bejelentkezési lapja sorolja fel, az egyszerű Felhasználónévi utótag definiált a helyszíni Active Directory és jeleníti meg a megfelelő állapot minden utótag szemben. Az állapot értékek egyike lehet az alábbi:

| Állam | Leírás | TEENDŐ |
|:----|:----|:----|
|Ellenőrzése| Azure AD Connect található egyező igazolta a tartományát az Azure Active Directory. A tartomány összes felhasználó jelentkezhetnek be a helyszíni hitelesítő adataival.| Nincs teendő |
|Nincs ellenőrizve| Azure AD Connect találta egyező egyéni tartományt az Azure Active Directory, de nem ellenőrzött lesz. A felhasználók a tartomány UPN-utótag változik az alapértelmezett. onmicrosoft.com utótag után szinkronizálási, ha a tartomány nem ellenőrzött lesz. | Az egyéni tartomány ellenőrzése az Azure Active Directory. [tudj meg többet](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Nem hozzáadva| Azure AD Connect nem találja az UPN-utótag megfelelő egyéni tartományt. A tartomány felhasználóit UPN-utótag változik az alapértelmezett. onmicrosoft.com, ha nem adja hozzá a tartományt, és verifeid Azure-ban.| Hozzáadásához és ellenőrzésé [További tudnivalók](./active-directory-add-domain.md) az UPN-utótag megfelelő egyéni tartomány|

Azure Active Directory bejelentkezési lapja a UPN suffix(s) az aktuális állapotú igazolás Azure AD a helyszíni Active Directory és a megfelelő egyéni tartományt meghatározott sorolja fel. Egyéni telepítési a felhasználónév az **Azure Active Directory bejelentkezési** lapja a attribútum ekkor választhatja.

![Azure Active Directory bejelentkezési lapja](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Kattintson a frissítés gombra kattintva újra az egyéni tartományok legújabb állapotának beolvasni Azure Active Directory kattinthat.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Egyszerű felhasználónév attribútum kiválasztása az Azure Active Directory

A UserPrincipalName - az attribútum userPrincipalName a felhasználók használja az azok bejelentkezés az Azure Active Directory attribútum és az Office 365-ben. A tartományok használja, más néven az UPN-utótag – az Azure AD a rendszer szinkronizálja a felhasználók előtt kell ellenőrizni. Az alapértelmezett attribútum userPrincipalName megtartása erősen ajánlott. Ha az attribútum nem átirányítható, és nem lehet ellenőrizni, akkor lehetséges, jelöljön ki egy másik attribútumot, például e-mailben, mint az attribútum, eközben tartsa a bejelentkezési azonosítóját. Ez az úgynevezett eltérő azonosítót. A másodlagos azonosító attribútumérték követniük kell a RFC822 szabvány. Másodlagos azonosító jelszó egyszeri bejelentkezés (SSO) és az összevonási Egyszeri bejelentkezési megoldásként is használható.

>[AZURE.NOTE] Egy másik azonosítójával szolgáltatás nem kompatibilis az Office 365-munkaterhelésekből. További tudnivalókért olvassa el [Alternatív bejelentkezési azonosító beállítása](https://technet.microsoft.com/library/dn659436.aspx).

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Másik egyéni tartományt államok és hatással Azure bejelentkezés módja
Nagyon fontos megértenie az egyéni tartomány államok az Azure Active directory és az egyszerű Felhasználónévi utótag definiált helyszíni. Ha használja az Azure Active Directory összekötése sycnhronization beállításakor tudassa velünk hajtsa végre a különböző lehetséges Azure bejelentkezési változat.

Az alábbi információk, tudassa velünk feltételezik, hogy azt az egyszerű Felhasználónévi utótag contoso.com használt a helyszíni címtárban részeként UPN, például az érintett user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Express-beállítások és a jelszó-szinkronizálás
| Állam         | Azure bejelentkezés felhasználói felület gyakorolt hatás |
|:-------------:|:----------------------------------------|
| Nem hozzáadva | Ebben az esetben a contoso.com nincs egyéni tartomány kerül az Azure Active directory. Egyszerű Felhasználónévi helyszíni utótaggal rendelkező felhasználók @contoso.com, nem tudja a helyszíni egyszerű felhasználónév használatával jelentkezzen be az-Azure. Ehelyett rendelkeznek használni egy új UPN őket által megadott Azure AD az alapértelmezett Azure Active directory-utótag hozzáadásával. Tegyük fel például, hogy vannak szinkronizál a felhasználóknak, hogy az Azure Active directory azurecontoso.onmicrosoft.com majd a helyszíni felhasználói user@contoso.com kapnak az egyszerű Felhasználónéviuser@azurecontoso.onmicrosoft.com|
| Nincs ellenőrizve | Ebben az esetben felkínálunk egy egyéni tartományt a contoso.com hozzáadott az Azure Active directory, de nem még ellenőrizni. Ha a felhasználók szinkronizálását a tartomány ellenőrzése nélkül a következő lép, majd a felhasználók lesz egy új UPN ugyanúgy, mint azt jelent a "nem hozzáadott" helyzetben Azure AD.|
| Ellenőrzése | Ebben az esetben van még egy egyéni tartományt a contoso.com már hozzáadott és az Azure AD-UPN-utótag ellenőrizve. Felhasználók a helyszíni egyszerű felhasználónév, például használni tudnak user@contoso.com, való bejelentkezéshez az Azure őket a rendszer szinkronizálja az Azure Active Directory után|

###### <a name="ad-fs-federation"></a>Active Directory összevonási szolgáltatások összevonási
Nem hozható létre egy összevonási alapértelmezés szerint az. onmicrosoft.com tartomány az Azure Active Directory vagy az Azure Active Directory-nem ellenőrzött egyéni tartományt. Ha az Azure AD Connect varázsló futtat, ha kijelöl egy nem ellenőrzött tartományt szeretne létrehozni egy majd Azure AD Connect az összevonási kérni fogja a szükséges rekordok létrehozásához hol helyezkedik el a tartomány a DNS. További információt talál [az alábbi](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Ha "Az Active Directory összevonási szolgáltatások összevonási" felhasználónak bejelentkezés lehetőséget választotta, akkor egy összevonási létrehozása az Azure Active Directory folytatásához egyéni tartományt kell rendelkeznie. Saját vitafórum Ez azt jelenti, hogy kell-e azt egy egyéni tartományt a contoso.com az Azure Active directory hozzáadott.

| Állam         | Azure bejelentkezés felhasználói felület gyakorolt hatás |
|:-------------:|:----------------------------------------|
| Nem hozzáadva | Ebben az esetben Azure AD Connect nem találta egyező egyéni tartományt az egyszerű Felhasználónévi utótag contoso.com az Azure Active directory számára. Be kell állítania egy egyéni tartományt a contoso.com, ha bejelentkezés Active Directory összevonási szolgáltatások használata a helyszíni egyszerű felhasználónév, például a felhasználók user@contoso.com.|
| Nincs ellenőrizve | Ebben az esetben Azure AD Connect kérni fogja ellenőrizheti a tartomány későbbi szakaszában kattintson a megfelelő részleteivel|
| Ellenőrzése | Ebben az esetben láthatja el a következő további műveletek nélkül beállításokkal|  

## <a name="changing-user-sign-in-method"></a>Felhasználói bejelentkezési módszer módosítása

Módosíthatja a felhasználó bejelentkezési módszer az összevonási jelszó-szinkronizálás használata a tevékenységek avaialble Azure AD Connect Azure AD Connect a varázslóval a kezdeti konfigurálása után. Futtassa újra az Azure AD Connect varázslót, és akkor be kell mutatni elvégezhető feladatok listáját. A feladatok listájából válassza a **Módosítás felhasználónak bejelentkezés** . 

![Módosítsa a felhasználónak bejelentkezés"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

A következő lapon a rendszer kéri a hitelesítő adatokat kér Azure AD.

![Azure Active Directory csatlakoztatása](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Válassza ki a **felhasználó bejelentkezési** lapján, a **Jelszó-szinkronizálás**. Ez változik a könyvtár egy felügyelt egy összevont.

![Azure Active Directory csatlakoztatása](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Ha csak ideiglenesen kapcsoló-jelszó-szinkronizálás, majd jelölje be **nem alakítja át a felhasználói fiókok**. A funkciót nem ellenőrzése vezet minden felhasználó alakítása összevont, és több óráig is eltarthat.
  
## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitások Azure Active Directoryval való integrálásának](active-directory-aadconnect.md).

További tudnivalók a [Azure AD Connect: tervezze meg fogalmat is](active-directory-aadconnect-design-concepts.md)


