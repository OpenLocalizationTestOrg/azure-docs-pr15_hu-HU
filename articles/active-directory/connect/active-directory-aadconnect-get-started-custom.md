<properties
    pageTitle="Azure AD Connect: Egyéni telepítési |} Microsoft Azure"
    description="A dokumentum adatai az egyéni telepítési beállítások az Azure AD Connect. Active Directory-Azure AD Connect telepítéséhez használja ezeket az utasításokat."
    services="active-directory"
    keywords="Mi az Azure AD Connect, telepítse az Active Directory, az Azure Active Directory szükséges összetevők"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Egyéni telepítési az Azure AD Connect
Azure AD Connect **egyéni beállítások** használatos, ha azt szeretné, hogy több a telepítés lehetőséget. Több erdők esetén, vagy ha be szeretné állítani a választható funkció nem vonatkozik a sürgős telepítési szolgál. Minden olyan esetben, ha a [**sürgős telepítési**](active-directory-aadconnect-get-started-express.md) beállítás nem felel meg a telepítő vagy a topológia használatban van.

Azure AD Connect telepítés megkezdése előtt győződjön meg arról, hogy [Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) töltheti le, és a teljes a előzetesen szükséges lépések leírását [Azure AD Connect: hardver- és előfeltételek](../active-directory-aadconnect-prerequisites.md). Győződjön meg arról is szükséges a fiókok [és Azure AD Connect fiókok és engedélyek](active-directory-aadconnect-accounts-permissions.md)ismertetett módon érhető el.

Ha a testre szabott beállítások nem egyezik meg a topológia, például a DirSync frissítése, akkor olvassa el a [dokumentáció kapcsolatos](#related-documentation) egyéb felhasználási területei.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Egyéni beállítások telepítésének Azure AD Connect

### <a name="express-settings"></a>Express-beállítások
Ezen a lapon kattintson a **Testreszabás** egy testre szabott beállításait a telepítés megkezdéséhez.

### <a name="install-required-components"></a>Szükséges összetevők telepítése
A szinkronizálási szolgáltatások telepítésekor hagyhatja, a választható konfigurációs szakasz nem jelöli be, és Azure AD Connect beállítja a szolgáltatás automatikusan. Egy SQL Server 2012 Express LocalDB példány létrehozza a megfelelő csoportok létrehozása és engedélyeket. Ha kívánja, módosíthatja az alapértelmezett beállításokat, az alábbi táblázat segítségével elérhető választható beállítási lehetőségekről.

![A szükséges összetevők](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Választható konfiguráció  | Leírás
------------- | -------------
Egy meglévő SQL Server használata | Lehetővé teszi az SQL Server és a példány nevét adja meg. Akkor válassza ezt a lehetőséget, ha már van egy adatbázis-kiszolgálóval, amelyeket használni szeretne. Írja be a **Példány nevét** vesszővel és port számot követ, ha az SQL Server nincs engedélyezve böngészési példány nevét.
Meglévő fiók szolgáltatás használata | Alapértelmezés szerint az Azure AD Connect létrehoz egy helyi szolgáltatásfiók a szinkronizálási szolgáltatások használatára. A jelszó nem az automatikusan generált és az ismeretlen annak a személynek, Azure AD Connect telepítése. Akkor használja a távoli SQL server vagy a proxy-hitelesítést igényel, ha szüksége van-e szolgáltatás a tartomány fiókra, és ismeri a jelszót. Ezekben az esetekben adja meg a szolgáltatás fiók használatához. Győződjön meg róla a felhasználó a telepítés futó SQL-Társítást, egy bejelentkezési az szolgáltatás fiókjához tartozó hozhatók létre. [Azure AD Connect fiókok](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation) és engedélyek
Adja meg a szinkronizálási egyéni csoportokat | Alapértelmezés szerint Azure AD Connect hoz négy csoportok a kiszolgálóra helyi szinkronizálási szolgáltatások telepítésekor. Ezek a csoportok: Rendszergazdák csoport, operátorok, Tallózás csoport, és a jelszó alaphelyzetbe állítása csoportnak. A saját csoportok Itt adhatja meg. A csoportok helyi kiszolgálói kell lennie, és a tartomány nem található.

### <a name="user-sign-in"></a>Felhasználónak bejelentkezés
Miután telepítette a szükséges összetevők, rendszer kéri, hogy a felhasználók egyszeri bejelentkezéses mód kiválasztása. A következő táblázat a rendelkezésre álló beállítások rövid leírását. A bejelentkezési módszerek teljes körű leírását lásd: a [felhasználónak bejelentkezés](../active-directory-aadconnect-user-signin.md).

![Felhasználónak bejelentkezés](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Egyszeri bejelentkezés beállítás | Leírás
------------- | -------------
Jelszó-szinkronizálás | Felhasználók is tudják jelentkezzen be Microsoft-felhőszolgáltatások, például az Office 365-ben, a helyszíni hálózaton használnak ugyanazt a jelszót használja. A felhasználók jelszavát, a jelszó kivonat Azure AD a rendszer szinkronizálja, és a hitelesítés történik a felhőben. [A jelszó-szinkronizálás](../active-directory-aadconnectsync-implement-password-synchronization.md) talál további információt.
Az Active Directory összevonási szolgáltatások összevonási | Felhasználók is tudják jelentkezzen be Microsoft-felhőszolgáltatások, például az Office 365-ben, a helyszíni hálózaton használnak ugyanazt a jelszót használja.  A rendszer átirányítja a felhasználókat azok a helyszíni Active Directory összevonási szolgáltatások a bejelentkezés az példány és hitelesítés helyszíni történik.
Nem konfigurálása | Sem funkció telepítve és beállítva. Akkor válassza ezt a lehetőséget, ha már van 3 fél összevonási kiszolgáló vagy egy másik már létező megoldást helyen.

### <a name="connect-to-azure-ad"></a>Azure Active Directory csatlakoztatása
A képernyőre Azure AD Connect írja be egy globális rendszergazdai fiókkal és a jelszót. Ha az előző oldal **az Active Directory összevonási szolgáltatások összevonási** lehetőséget választotta, nem jelentkezzen be a tartományban fiókkal szeretné engedélyezése az összevonási. Ajánlást, hogy az alapértelmezett **onmicrosoft.com** tartományban, amely megtalálható az Azure Active directory-fiókot használ.

Ehhez a fiókhoz van csak a szolgáltatási fiók létrehozása az Azure Active Directory és a varázsló befejezése után nem használható.  
![Felhasználónak bejelentkezés](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Ha a globális rendszergazdai fiókkal MFA engedélyezve van, majd meg kell adja meg a jelszót újra a bejelentkezési tulajdonságra, és végezze el a MFA kérdés. A kérdés lehet egy kezeléséről a ellenőrzőkódot vagy hívni.  
![MFA felhasználónak bejelentkezés](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

A globális rendszergazdai fiókkal is [Yammerhez Identitáskezelés](../active-directory-privileged-identity-management-getting-started.md) engedélyezve van.

Ha hiba jelenik meg, és a csatlakozási problémák lépnek fel, majd megtekintése [kapcsolódási problémák elhárítása](../active-directory-aadconnect-troubleshoot-connectivity.md)

## <a name="pages-under-the-section-sync"></a>Lapok szinkronizálás csoportban

### <a name="connect-your-directories"></a>A könyvtárak csatlakoztatása
Az Active Directory tartományi szolgáltatások csatlakozhat Azure AD Connect van szüksége a megfelelő engedélyekkel rendelkező fiók hitelesítő adatait. A tartomány rész NetBios vagy a teljes Tartománynevet formátumban, azaz FABRIKAM\syncuser vagy fabrikam.com\syncuser adhat meg. Ehhez a fiókhoz lehet normál felhasználói fiók, mivel az alapértelmezett olvasási engedélyeket csak szüksége van. Az esete előfordulhat további engedélyeket. További tudnivalókért lásd: az [Azure Active Directory csatlakozás fiókok és engedélyek](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Kapcsolódás könyvtárban](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure Active Directory bejelentkezési konfiguráció
Ezen az oldalon teszi lehetővé, tekintse át az UPN tartományok a helyszíni Active Directory tartományi szolgáltatások, amelyek az Azure Active Directory van ellenőrizve. Ezen az oldalon is lehetővé teszi a használandó a userPrincipalName attribútum konfigurálása.

![A tartományok nem ellenőrzött](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Ellenőrizze minden tartomány **Nem adja hozzá** , és **Nincs ellenőrizve**. Ellenőrizze, hogy használja ezeket a tartományokat az Azure Active Directory már ellenőrizte. Kattintson az frissítési a tartomány ellenőrzését. További tudnivalókért lásd: [hozzáadása és a tartomány tulajdonjogának igazolása](../active-directory-add-domain.md)

**UserPrincipalName** – az attribútum userPrincipalName az attribútum felhasználók használja, ha az azok jelentkezzen be az Azure Active Directory és az Office 365-ben. A tartományok használja, más néven az UPN-utótag – az Azure AD a rendszer szinkronizálja a felhasználók előtt kell ellenőrizni. Az alapértelmezett attribútum userPrincipalName tartani a Microsoft javasolja. Ha az attribútum nem átirányítható, és nem tudja ellenőrizni, majd jelöljön ki egy másik attribútumot lehetséges. Példa kijelölhet e-mailek, az attribútum, eközben tartsa a bejelentkezési azonosítóját. Egy másik, mint a userPrincipalName attribútum használata **Alternatív**ID neve. A másodlagos azonosító attribútumérték követniük kell a RFC822 szabvány. Másodlagos azonosító jelszó-szinkronizálás és összevonási is használható.

>[AZURE.WARNING]
Egy másik azonosítójával szolgáltatás nem kompatibilis az Office 365-munkaterhelésekből. További tudnivalókért olvassa el az [Alternatív bejelentkezési azonosító beállítása](https://technet.microsoft.com/library/dn659436.aspx).

### <a name="domain-and-ou-filtering"></a>Tartomány és a szervezeti egységre szűrése
Alapértelmezés szerint a tartományok és a szervezeti vannak szinkronizálva. Ha egyes tartományok vagy szervezeti szinkronizál az Azure Active Directory nem szeretné, megszüntetheti a tartományok és a szervezeti.  
![Szűrés DomainOU](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) a lapon, a varázsló tartományalapú szűrés konfigurálja. További tudnivalókért olvassa el a [tartományalapú szűrése](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering)című témakört.

Lehetőség arra is, hogy bizonyos tartományok nem érhetők el a tűzfal korlátozásai miatt. Ezeket a tartományokat alapértelmezés szerint nincs bekapcsolva, és hogy figyelmeztetés.  
![A tartományok nem érhető el](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Ha ez a figyelmeztetés jelenik meg, ellenőrizze, hogy valóban érhetők el ezeket a tartományokat, és a figyelmeztetés várhatóan.

### <a name="uniquely-identifying-your-users"></a>A felhasználók egyedileg azonosító
Az egyező keresztül erdők szolgáltatás lehetővé teszi, hogy határozhatja meg, hogyan jelennek meg a felhasználók az Active Directory tartományi szolgáltatások erdőkből Azure AD. A felhasználó előfordulhat, hogy vagy képviselteti csak egyszer összes erdők vagy van engedélyezett és a letiltott partnerek kombinációi. Partner néhány erdőkben is előfordulhat, hogy a felhasználó ábrázolva.

![Egyedi](./media/active-directory-aadconnect-get-started-custom/unique.png)

A beállítás | Leírás
------------- | -------------
[Felhasználók csak jelző egyszer összes erdők](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Minden felhasználónak Azure Active Directory egyes objektumként jönnek létre. Az objektumok nem a metaverse a tartományhoz.
[Levelezési attribútum](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Ezt a lehetőséget, ha a levelezési attribútum azonos értékkel rendelkezik, különböző erdőkben felhasználók és a partnerek csatlakozik. Akkor használja ezt a beállítást, ha a partnerek GALSync létrehozott.
[ObjectSID és msExchangeMasterAccountSID / msRTCSIP-OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Ez a beállítás egy engedélyezett felhasználói fiók erdőben erőforráserdők az egy letiltott felhasználóval fűz össze. Az Exchange ebben a konfigurációban a csatolt postaláda nevezik. Ez a beállítás is használható, ha csak a Lync használata és Exchange nem szerepel a Erőforráserdő.
sAMAccountName, mind a mailnickname-beli | Ez a beállítás attribútumait, ha várhatóan megtalálható a felhasználó bejelentkezési azonosító fűz össze.
Egy adott attribútum | Ezzel a beállítással, jelölje be a saját attribútum. **Korlátozása:** Győződjön meg arról, hogy már megtalálható a metaverse attribútum válassza. Ha egy egyéni attribútum (nem szerepel a metaverse), a varázsló nem sikerül elvégezni.

**Forrás horgony** – az attribútum sourceAnchor egy attribútum, amely a user objektumban élettartama során megváltoztatható. Az elsődleges kulcs a helyszíni rendelkező felhasználó összekapcsolása az Azure Active Directory. Az attribútum nem módosíthatók, mivel a meg kell terveznie egy jó attribútum használni. A helyes jelölt objectGUID. Az attribútum nem változik, kivéve, ha a felhasználói fiók erdők/tartományok közötti kerül. Több erdőre környezetben hol erdők között partnerek áthelyezése egy másik attribútum kell használni, például az Alkalmazottkód attribútum. Kerülje a tulajdonságait szeretné módosítani, ha egy személy házasságot köt vagy hozzárendelések módosítása. Az attribútumok nem használhat egy @-sign, , a levelezés és a userPrincipalName nem használhatók. Az attribútum egyben a kis-és nagybetűket áthelyezésekor objektum erdők között, ügyeljen, hogy a felső vagy alsó eset megőrzése érdekében. Bináris attribútumok base64 kódolású, de más attribútum típusú kódolatlan állapotában marad. Az összevonási esetek és bizonyos Azure AD-kapcsolatok Ez az attribútum is nevezik immutableID. A forrás horgony további információt a [Tervező fogalmak](../active-directory-aadconnect-design-concepts.md#sourceAnchor)találhatók.

### <a name="sync-filtering-based-on-groups"></a>Csoportok szinkronizálási szűrés alapján
A szűrés a csoportok funkció lehetővé teszi, hogy csak egy kisebb részét szinkronizálta a próbaüzem. Ez a szolgáltatás használatához a csoport létrehozása a helyszíni Active Directoryban erre a célra. Azure Active Directory közvetlen tagként majd hozzá felhasználók és csoportok kell szinkronizálnia. Később hozzáadása és eltávolítása a felhasználókat a csoportba megőrzéséhez kell lennie az Azure Active Directory-objektumok listáját. Szinkronizálni kívánt összes objektumát közvetlenül a csoport tagjának kell lennie. Felhasználók, csoportok, névjegyek és számítógépeken és eszközökön összes tagjának kell lennie közvetlen. Beágyazott csoporttagság továbbra is fennáll. Hozzáadásakor egy csoportot, csak a saját csoport tag hozzáadása és tagjai nem.

![Szinkronizálási szűrése](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Ez a funkció csak a támogatási kísérleti telepítés szolgál. Egy full-blown éles üzemi ne használja azt.

Az egy full-blown éles üzemi környezetben, fog nehezen és szinkronizál a egyetlen csoport kezelése. Ehelyett kell használni a lehetőségek közül választhat a [szűrés konfigurálása](../active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Választható lehetőségek
A képernyőn jelölje ki a választható szolgáltatások számára a különböző forgatókönyvekben teszi lehetővé.

![Választható lehetőségek](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Ha jelenleg DirSync vagy aktív Azure AD-szinkronizálás, nem aktiválta az Azure AD Connect visszaírást szolgáltatások.

Választható lehetőségek | Leírás
------------------- | -------------
Hibrid Exchange-telepítés | A hibrid Exchange-telepítés szolgáltatás lehetővé teszi, hogy az Exchange-postaládák közös megléte mindkét a helyszíni és az Office 365-ben. Azure AD Connect helyszíni címtárában az Azure Active Directory [attribútumok](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) meghatározott szinkronizál.
Azure Active Directory-alkalmazás és a attribútum szűrése | Azure AD-alkalmazás és -szűrés attribútum engedélyezésével az szinkronizált attribútumokat testreszabható. Ez a beállítás további konfigurációs oldalból ad a varázsló. További tudnivalókért lásd: [Azure AD-alkalmazás és -szűrés attribútum](#azure-ad-app-and-attribute-filtering).
A jelszó-szinkronizálás | Ha a bejelentkezési megoldásként összevonási lehetőséget választotta, majd engedélyezheti ezt a beállítást. A jelszó-szinkronizálás használható biztonsági lehetőségként. További tudnivalókért olvassa el a [Jelszó-szinkronizálás](../active-directory-aadconnectsync-implement-password-synchronization.md)című témakört.
Jelszó visszaírást | Jelszó visszaírást engedélyezésével származnak az Azure Active Directory jelszó-módosítások írása vissza a helyszíni címtárában. További tudnivalókért olvassa el a [-ös jelszavát az első lépések](../active-directory-passwords-getting-started.md)című témakört.
Csoport visszaírást | Ha az **Office 365-csoportok** szolgáltatást használja, majd lehet a helyszíni Active Directory ábrázolt ezekhez a csoportokhoz. Ez a beállítás csak akkor érhető el, ha az Exchange jelen a helyszíni Active Directory. További tudnivalókért olvassa el a [csoport visszaírást](../active-directory-aadconnect-feature-preview.md#group-writeback)című témakört.
Eszköz visszaírást | Azure Active Directory a helyszíni Active Directory-feltételes hozzáférési környezetek kialakításához lehetővé teszi az visszaírást eszköz objektumokat. További tudnivalókért olvassa el a [engedélyezése eszköz visszaírást az Azure AD Connect](../active-directory-aadconnect-feature-device-writeback.md)című témakört.
Bővítmény-attribútum címtár-szinkronizálás | Címtár-bővítmények attribútum szinkronizálás engedélyezésével a megadott attribútumok Azure AD a rendszer szinkronizálja. További tudnivalókért olvassa el a [címtár-bővítmények](../active-directory-aadconnectsync-feature-directory-extensions.md)című témakört.

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure Active Directory-alkalmazás és a attribútum szűrése
Ha szeretné korlátozni, mely attribútumok és Azure Active Directory szinkronizálása, indítsa el esetén szolgáltatások kiválasztásával. Ha Ön konfigurációs ezen a lapon, új szolgáltatás ki kell választani kifejezetten az telepítővarázslóban újrafuttatásával.

![Választható funkció alkalmazások](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Ezen a lapon megjelenik a szolgáltatásokat az előző lépésben kiválasztott alapján, a szinkronizált összes attribútum. A lista, a szinkronizált összes objektumtípusok kombinációi. Ha bizonyos adott attribútumok kell nem szinkronizálja, megszüntetheti e attribútumok.

![Választható funkció attribútumok](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Attribútumok eltávolítása hatással lehet a használható funkciók körét. Ajánlott eljárások és javaslatok című témakörben talál [attribútumok szinkronizálva](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Bővítmény-attribútum címtár-szinkronizálás
Az Azure Active Directory a szervezet vagy más attribútumait, az Active Directory által hozzáadott egyéni attribútumokkal rendelkező bővíthetik a sémában. Ez a funkció használatához válassza a **címtár-bővítmény attribútum szinkronizálás** **Választható funkció** lapon. Választhatja a további attribútumok szinkronizálása ezen a lapon.

![A címtár-bővítmények](./media/active-directory-aadconnect-get-started-custom/extension2.png)

További tudnivalókért olvassa el a [címtár-bővítmények](../active-directory-aadconnectsync-feature-directory-extensions.md)című témakört.

## <a name="configuring-federation-with-ad-fs"></a>Active Directory összevonási szolgáltatások összevonási konfigurálása
Active Directory összevonási szolgáltatások konfigurálása az Azure AD Connect egyszerű mindössze néhány kattintással. A következő van szükség a konfigurációs előtt.

- A Távfelügyelet engedélyezve van az összevonási kiszolgáló a Windows Server 2012 R2 webkiszolgálón
- A webes alkalmazás proxykiszolgáló Távfelügyelet engedélyezve van a Windows Server 2012 R2 kiszolgáló
- SSL-tanúsítvány, az összevonási szolgáltatás neve kívánja használni (például sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>Active Directory összevonási szolgáltatások konfigurációs előtti követelmények
Adja meg a AD FS-farm Azure AD Connect használ, győződjön meg arról WinRM engedélyezve van a távoli kiszolgáló. Ezenkívül a [táblázat 3 - Azure AD Connect és az összevonási kiszolgálók/WAP](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap)portok kötelező folyamatát.

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Új AD FS-farmok létrehozása vagy egy meglévő Active Directory összevonási szolgáltatások farm használata
Egy meglévő Active Directory összevonási szolgáltatások farm is használhatja, vagy hozzon létre egy új AD FS-farm választhatja. Ha úgy dönt, hogy hozzon létre egy újat, adja meg az SSL-tanúsítvány szükségesek. Ha az SSL-tanúsítvány jelszóval védett, a program kéri a jelszót.

![Active Directory összevonási szolgáltatások Farm](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Ha úgy dönt, hogy egy meglévő Active Directory összevonási szolgáltatások farm használja, közvetlenül a konfigurálása az Active Directory összevonási szolgáltatások és Azure Active Directory-képernyő adatvédelmi viszonya kattintva megnyílik.

### <a name="specify-the-ad-fs-servers"></a>Adja meg az AD FS-kiszolgálók
Adja meg a kiszolgálókat, telepítse az Active Directory összevonási szolgáltatások a kívánt. Egy vagy több kiszolgálót, a tervezési igényeinek kapacitása alapján is hozzáadhat. Csatlakozás Active Directoryhoz összes kiszolgálón, ebben a konfigurációban végrehajtása előtt. A Microsoft javasolja egyetlen AD FS-kiszolgáló nem numerikus és próbaüzem telepítésekhez telepítése. Ezután hozzáadása és üzembe további kiszolgálókat méretezési igényekhez Azure AD Connect futtatásával kezdeti konfigurálása után újra.

>[AZURE.NOTE]
Győződjön meg arról, hogy az összes kiszolgálón tartományhoz az Active Directory előtt a művelet Ez a beállítás előtt.

![Active Directory összevonási szolgáltatások kiszolgálón](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Adja meg a webes alkalmazás proxykiszolgálók
Írja be a webes alkalmazás proxykiszolgálók kívánt kiszolgálókhoz. A webes alkalmazás proxykiszolgáló a DMZ (extranetes elérhető) a telepítve van, és a extranetes érkező hitelesítési kérések támogatja. Egy vagy több kiszolgálót, a tervezési igényeinek kapacitása alapján is hozzáadhat. Egyetlen webes alkalmazás proxykiszolgáló nem numerikus és próbaüzem telepítésekhez telepíti a Microsoft javasolja. Ezután hozzáadása és üzembe további kiszolgálókat méretezési igényekhez Azure AD Connect futtatásával kezdeti konfigurálása után újra. Azt javasoljuk, hogy az intraneten hitelesítést kielégítéséhez proxykiszolgálók azonos számú problémákat.

>[AZURE.NOTE]
<li> Ha nem egy helyi rendszergazdai Active Directory összevonási szolgáltatások kiszolgálókon használt fiókkal, majd kéri rendszergazdai hitelesítő adataival.</li>
<li> Győződjön meg arról, hogy van az Azure AD Connect kiszolgáló és a webes alkalmazás proxykiszolgáló HTTP-/ HTTPS összekapcsolását ebben a lépésben futtatása előtt.</li>
<li> Győződjön meg arról, hogy van-e a HTTP-/ HTTPS-kapcsolat keresztül flow hitelesítési kérések engedélyezése az webalkalmazás-kiszolgáló és az AD FS-kiszolgáló között.</li>

![Web App alkalmazásban](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Írja be hitelesítő adatait, így a web application server létesíthetnek az AD FS-kiszolgáló biztonságos kapcsolatot kéri. A hitelesítő adatokat kell a helyi rendszergazdák az AD FS-kiszolgálón.

![Proxykiszolgáló](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Adja meg a szolgáltatás fiókot az AD FS-szolgáltatás
Az Active Directory összevonási szolgáltatások szolgáltatás hitelesítést végezni a felhasználók és a keresés felhasználói adatok az Active Directory tartományi szolgáltatás fiók szükséges. Kétféle szolgáltatás fiókok támogatja:

- **Csoport felügyelt szolgáltatásfiók** - az Active Directory tartományi szolgáltatások Windows Server 2012 jelent meg. A fiók típusa szolgáltatások, például az Active Directory összevonási szolgáltatások, egy adott fiókhoz anélkül, hogy a fiókhoz tartozó jelszó frissítése rendszeresen biztosít. Használja ezt a lehetőséget, ha már van Windows Server 2012 tartomány vezérlők tartományt, amelyikbe az AD FS-kiszolgálókon.
- **Tartományi fiók** - fiókot a következő típusú kéri, adja meg a jelszót, és rendszeresen módosítani a jelszót, ha a jelszó módosítása vagy lejártáról. Ez a beállítás csak akkor használja a tartományt, amelyikbe az AD FS-kiszolgálókon nincs telepítve a Windows Server 2012 tartomány vezérlők.

Ha a kijelölt csoport felügyelt szolgáltatásfiók, és ez a funkció soha nem használta az Active Directory, a vállalati rendszergazdai hitelesítő adatok kéri. Ezek a hitelesítő adatok kezdeményezni a fő áruházból, és engedélyezze a funkciót, az Active Directory használják.

![Active Directory összevonási szolgáltatások szolgáltatásfiók](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Jelölje ki a használni kívánt kapcsolt Azure Active Directory tartományt
A fenti konfiguráció beállítása az Active Directory összevonási szolgáltatások és Azure Active Directory összevonási viszonya használják. Konfigurálja az Active Directory összevonási szolgáltatások probléma biztonsági tokenek az Azure Active Directory és a konfigurálása az Active Directory összevonási szolgáltatások adott példány a tokenek megbízhatónak Azure AD. Ezen az oldalon csak lehetővé teszi a kezdeti telepítés egyetlen tartomány beállítása. További tartományok később beállítható Azure AD Connect újbóli futtatásával.

![Azure Active Directory tartományi](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>A kijelölt összevonási Azure Active Directory tartomány tulajdonjogának igazolása
Ha bejelöli a tartományt az összevonáshoz, Azure AD Connect biztosít a szükséges információkat egy nem ellenőrzött tartomány ellenőrzéséhez hivatkozásra. Lásd: [hozzáadása és a tartomány tulajdonjogának igazolása](../active-directory-add-domain.md) kapcsolatos információk.

![Azure Active Directory tartományi](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
A tartomány hitelesítése az konfigurálása szakaszban próbálja AD Connect. Állítsa be a szükséges DNS-rekordok hozzáadása nélkül továbbra is, ha a varázsló nem áll képes a konfigurálás befejezéséhez.

## <a name="configure-and-verify-pages"></a>Állítsa be, és ellenőrizze a lapok
A konfiguráció történik ezen a lapon.

>[AZURE.NOTE]
Telepítési a folytatás előtt, és úgy állította be, hogy összevonási, győződjön meg arról, hogy van-e beállítva [az összevonási kiszolgálók névfeloldás](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).

![Készen áll a konfigurálása](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Fejlesztői üzemmód
Új szinkronizálási kiszolgáló átmeneti tárolására mód párhuzamosan beállítási lehetőség. Ahhoz, hogy egy adott könyvtár a felhőben exportálása egy szinkronizálási kiszolgáló csak támogatja. De ha szeretné helyezni a kiszolgálóról egy másik, például egy futó DirSync, majd engedélyezheti átmeneti tárolására mód az Azure AD Connect. Ha engedélyezve van, a szinkronizálási motor importálása, és szinkronizálja a szokásos módon adatokat, de azt nem exportálja bármi Azure Active Directory vagy AD. A szolgáltatások jelszó szinkronizálása és a jelszó visszaírást a átmeneti tárolására üzemmódban le vannak tiltva.

![Fejlesztői üzemmód](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

A fejlesztői módban, akkor a szinkronizálási motor végezze el a szükséges módosításokat, és ellenőrizze, hogy mit kell tudni az exportálandó van. A konfiguráció elégedett az eredménnyel, ha ismét a telepítés varázslót, és fejlesztői üzemmód letiltása. Adatok letöltése a kiszolgálóról exportálása a Azure AD. Ellenőrizze, hogy a többi kiszolgáló letiltásához egyszerre Igen, csak egy kiszolgáló aktívan exportálja.

További tudnivalókért olvassa el a [fejlesztői üzemmód](../active-directory-aadconnectsync-operations.md#staging-mode)című témakört.

### <a name="verify-your-federation-configuration"></a>Az összevonási konfigurációjának ellenőrzése
Azure AD Connect ellenőrzi a DNS-beállításait, az ellenőrzés gombra kattintáskor.

![Befejezése](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Ellenőrzése](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Ezeken kívül a következő lépésekkel ellenőrzése:

- Ellenőrizze, hogy jelentkezzen be az számítógépről tartomány illesztett az intraneten böngészőből: https://myapps.microsoft.com kapcsolódni, és ellenőrizze a jelentkezzen be a naplózott fiókját. A beépített Active Directory tartományi szolgáltatások rendszergazdai fiók nem szinkronizálódik, és nem használható a tartomány ellenőrzéséhez.
- Ellenőrizze, hogy jelentkezzen be az extranet az eszközről. Egy otthoni számítógépen vagy mobileszközön használja, a https://myapps.microsoft.com csatlakozzon, és adja meg a hitelesítő adatait.
- Érvényesítse ügyfélprogramok bejelentkezés. Csatlakozás https://testconnectivity.microsoft.com, válassza az **Office 365** lapon, és válassza az **Office 365 egyszeri bejelentkezéses vizsgálat**.

## <a name="next-steps"></a>Következő lépések
A telepítés befejezése után jelentkezzen ki, és jelentkezzen be újra a Windows szinkronizálási szolgáltatáskezelő vagy szinkronizálás szabály szerkesztő használata előtt.

Most, hogy van-e telepítve Azure AD Connect lehet [ellenőrzi a telepítést, és a licencek hozzárendelése](../active-directory-aadconnect-whats-next.md).

További tudnivalók a következő funkciók, melyek engedélyezett a telepítés: [Törlés tiltása véletlen](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) és [Azure Active Directory csatlakozás állapota](../active-directory-aadconnect-health-sync.md).

További tudnivalók az alábbi általános témakörök: [ütemezőt, és hogy miként indíthatja el a szinkronizálási](../active-directory-aadconnectsync-feature-scheduler.md).

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Kapcsolódó dokumentáció

A témakör |  
--------- | ---------
Azure AD Connect – áttekintés | [A helyszíni identitások integrálása az Azure Active Directory](../active-directory-aadconnect.md)
Telepítse a Express beállításokkal | [Express telepítésével Azure AD Connect](active-directory-aadconnect-get-started-express.md)
A DirSync frissítés | [Frissítés az Azure Active Directory-szinkronizáló (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
A fiókok telepítéshez | [További információ: Azure AD Connect fiókok és engedélyek](active-directory-aadconnect-accounts-permissions.md)
