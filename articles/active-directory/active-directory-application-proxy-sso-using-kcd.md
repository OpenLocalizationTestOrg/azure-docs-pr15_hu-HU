<properties
    pageTitle="Egyszeri bejelentkezés alkalmazás proxyval |} Microsoft Azure"
    description="Megtudhatja, hogy hogyan egyszeri bejelentkezés Azure Active Directory szolgáltatásalkalmazás-Proxy használatával adja meg."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Egyszeri bejelentkezés a szolgáltatásalkalmazás-Proxy

Egyszeri bejelentkezés az Azure Active Directory-alkalmazás proxy kulcsfontosságú eleme. A legjobb felhasználói felület nyújtó az alábbi lépéseket:

1. A felhasználó bejelentkezik a felhőbe  
2. Az összes biztonsági ellenőrzések fordulhat elő, a felhőben (előhitelesítés)  
3. A kérelem küld a helyszíni alkalmazást, amikor az alkalmazás Proxy összekötő megszemélyesíti a felhasználót. A kódmentes alkalmazás lapon, ez a normál felhasználó lesz a tartományhoz tartozó eszközről.

![Access ábra a szolgáltatásalkalmazás-Proxy keresztül a végfelhasználói a vállalati hálózathoz](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Azure Active Directory szolgáltatásalkalmazás-Proxy segít, hogy a felhasználók egyszeri bejelentkezés (SSO) élmény a szükséges. Az alkalmazások használata az egyszeri bejelentkezés közzététele kövesse az alábbi lépéseket:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>Helyszíni IWA alkalmazások szolgáltatásalkalmazás-Proxy KCD használata az egyszeri bejelentkezés
Az alkalmazások használatával integrált Windows-hitelesítés (IWA) alkalmazás proxy pontra összekötők engedélyt adhat az Active Directory felhasználója, megszemélyesítés és működőképessé tehető más nevében tokenek engedélyezheti az egyszeri bejelentkezés.


### <a name="network-diagram"></a>Hálózatdiagram

Ez az ábra bemutatja a folyamat, alkalommal, amikor egy felhasználó IWA használó helyszíni alkalmazás eléréséhez.

![Hitelesítés Microsoft AAD munkafolyamat-diagram](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. A felhasználó ír be, hogy az URL-cím a helyszíni alkalmazás eléréséhez alkalmazás proxyn keresztül.
2. Szolgáltatásalkalmazás-Proxy hitelesítési szolgáltatások Azure Active Directory preauthenticate átirányítja a kérelmet. Ezen a ponton a Azure Active Directory bármely megfelelő hitelesítési és engedélyezési házirendek, például többtényezős hitelesítést vonatkozik. Ha a felhasználó érvényesítése, Azure Active Directory létrehoz egy jogkivonat, és elküldi azt a felhasználót.
3. A felhasználó a token szolgáltatásalkalmazás-Proxy továbbítja.
4. Szolgáltatásalkalmazás-Proxy a token ellenőrzi az egyszerű felhasználónév (UPN) beolvassa, és ezt követően a kérést, (UPN) és az egyszerű nevét (Szolgáltatásnevet) küld az összekötő keresztül dually hitelesített biztonságos csatornát.
5. Az összekötő hajt végre Kerberos korlátozott meghatalmazás (KCD) egyeztetés a helyszíni Active Directory, a felhasználót, hogy az alkalmazás Kerberos jogkivonat első magát.
6. Az Active Directory küld a Kerberos jogkivonat, az összekötő alkalmazáshoz.
7. Az összekötő küld az eredeti az application server használatával kapott a AD Kerberos jogkivonathoz.
8. Az alkalmazás küld az összekötőt, majd visszatér a szolgáltatásalkalmazás-Proxy szolgáltatás, amely a választ, és végül a felhasználó számára.

### <a name="prerequisites"></a>Előfeltételek

Előkészületek egyszeri bejelentkezéssel alkalmazás proxy, ügyeljen az alábbi beállításokat és a konfigurációk készen áll-e a környezet:

- Az alkalmazásokat, például a SharePoint-webalkalmazások esetében integrált Windows-hitelesítés használatára van beállítva. További információt talál [Kerberos-hitelesítés engedélyezése támogatása](https://technet.microsoft.com/library/dd759186.aspx), vagy ha SharePoint- [megtervezése a SharePoint 2013-ban Kerberos-hitelesítés](https://technet.microsoft.com/library/ee806870.aspx)látja.

- A minden alkalmazás szolgáltatás egyszerű nevek van.

- Az összekötő rendszert futtató kiszolgáló és a alkalmazást futtató kiszolgáló is tartományhoz csatlakoztatott az ugyanabban a tartományban vagy a meghatalmazó tartományok része. Tartomány összekapcsolásnál további tudnivalókért lásd: [a számítógép tartományhoz illesztés](https://technet.microsoft.com/library/dd807102.aspx).

- Az összekötő rendszert futtató kiszolgáló van hozzáférése a TokenGroupsGlobalAndUniversal olvassa el a felhasználók számára. Ez az alapértelmezett beállítás, amely a környezet hardening biztonsági hatással lehet. További segítség: Ezzel a [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Az Active Directory konfigurálása

Az Active Directory-konfiguráció attól függően, hogy az alkalmazás Proxy összekötő és a kiszolgálón közzétett ugyanabban a tartományban vagy nem változik.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Összekötő és a kiszolgálón közzétett ugyanabban a tartományban

1. Az Active Directory, válassza az **eszközök** > **felhasználók és a számítógépen**.
2. Jelölje be az összekötő rendszert futtató kiszolgáló.
3. Kattintson a jobb gombbal, majd válassza a **Tulajdonságok** > **Meghatalmazás**.
4. Jelölje be az **ezen a számítógépen, csak a megadott szolgáltatások delegálás a megbízható gombra** , és a **szolgáltatások, amelyhez a fiók bemutathatja delegált hitelesítő adatok**területén írja be az értéket a SPN identitás az application Server.
5. Az alkalmazás Proxy összekötőre, az Active Directory, az alkalmazások listájában definiált ellen megszemélyesítés felhasználók szolgáltatás lehetővé teszi.

![Összekötő-SVR tulajdonságok ablak képernyőképe](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Összekötő és a kiszolgálón közzétett különböző tartományban

1. Előfeltétel KCD használata tartományok listájának [Kerberos korlátozott meghatalmazás tartományok](https://technet.microsoft.com/library/hh831477.aspx)talál.
2. A Windows 2012 R2, használja a `principalsallowedtodelegateto` összekötő kiszolgálói a szolgáltatásalkalmazás-Proxy, az összekötő kiszolgáló, hol található a a kiszolgálón közzétett delegálásának engedélyezése tulajdonság `sharepointserviceaccount` és a meghatalmazó kiszolgáló `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`a SPS számítógépfiók és egy szolgáltatási fiókot, amely alatt a SPS alkalmazáskészlet fut is lehet.


### <a name="azure-classic-portal-configuration"></a>Azure klasszikus portál beállításai

1. [Közzététel-alkalmazások szolgáltatásalkalmazás-Proxy](active-directory-application-proxy-publish.md)ismertetett útmutatása szerint az alkalmazás a Közzététel gombra. Győződjön meg róla, hogy a **Előhitelesítés módszer** **Azure Active Directory** .
2. Után jelenik meg az alkalmazást az alkalmazáslistából, jelölje ki azt, és kattintson a **Konfigurálás**gombra.
3. A **Tulajdonságok**csoportban állítsa **Belső hitelesítési módszer** **Integrált Windows-hitelesítést**.  
  ![Speciális alkalmazás konfigurációja](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Adja meg az application Server a **Belső alkalmazás SPN** . Ebben a példában az a közzétett alkalmazás SPN http/lob.contoso.com.  

>[AZURE.IMPORTANT] Ha a helyszíni egyszerű felhasználónév és az Azure Active Directory (UPN) nem azonos, szüksége lesz állítsa be a [bejelentkezési identitást meghatalmazott](#delegated-login-identity) ahhoz, hogy a munkát előhitelesítés.

|  |  |
| --- | --- |
| Belső hitelesítési módszer | Ha az Azure Active Directory előhitelesítés használ, beállíthatja, hogy egy belső hitelesítési mód engedélyezése a felhasználóknak, hogy élvezhetik az egyszeri bejelentkezés (SSO) ennek az alkalmazásnak. <br><br> Jelölje be a **Beépített Windows-hitelesítés** (IWA), ha az alkalmazás IWA használ, és beállíthatja a Kerberos korlátozott meghatalmazás (KCD) SSO ahhoz, hogy az alkalmazás. KCD használatával IWA használó alkalmazások kell beállítania, egyéb esetben szolgáltatásalkalmazás-Proxy nem lesz közzétehetik ezeket az alkalmazásokat. <br><br> Válassza a **nincs** , ha az alkalmazás nem használja a IWA. |
| Belső alkalmazás SPN | A helyszíni Azure AD a az alkalmazás a belső (SPN) szolgáltatás –-egyszerű felhasználóneve. A SPN az alkalmazás Proxy összekötő-ból Kerberos tokenek KCD használó alkalmazás használja. |


## <a name="sso-for-non-windows-apps"></a>A-Windows alkalmazások egyszeri bejelentkezés
Az Azure Active Directory szolgáltatásalkalmazás-Proxy Kerberos meghatalmazás áramlás indul el, ha az Azure Active Directory hitelesíti a felhasználó a felhőben. Miután a kérelem érkezik, a helyszíni, az Azure Active Directory Proxy összekötő interaktív használata a helyi Active Directory a felhasználó nevében Kerberos jegy hibaelhárítása. Ez a folyamat hivatkozik, amely szerint Kerberos korlátozott meghatalmazás (KCD). A következő szakaszban kérelem érkezik a Kerberos jegy kódmentes alkalmazással. Számos protokollok, amelyek meghatározzák, hogy miként küldhet megkeresések. A legtöbb-Windows kiszolgálók egyeztetése/SPNego Azure Active Directory szolgáltatásalkalmazás-Proxy most támogatott várnak.

![Nem Windows SSO-diagram](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Delegált bejelentkezési azonosító
Delegált bejelentkezési azonosító két különböző bejelentkezési forgatókönyvek kezeléséhez nyújt segítséget:

- Nem Windows alkalmazást, amely általában a felhasználóazonosító a képernyőn, felhasználónév vagy SAM fiók nevét, e-mail cím nem első (username@domain).
- Alternatív login konfigurációk hol (UPN) az Azure Active Directory és a helyszíni Active Directoryban (UPN) eltérőek.

A szolgáltatásalkalmazás-Proxy mely azonosító beszerzése a Kerberos jegy használatával jelölje ki. Ez a beállítás egy alkalmazás nem. Minden említett lehetőség alkalmasak rendszerek, amely nem fogadja el az e-mail cím formát, mások az alternatív bejelentkezési készültek.

![Delegált bejelentkezési azonosító paraméter képernyőképe](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Delegált bejelentkezési azonosító használja, ha az érték nem lehet a tartományok vagy erdők a szervezet egyedi. Ez a probléma elkerülheti ezeket az alkalmazásokat kétszer csoportokkal két különböző összekötő közzétéve. Mivel az összes alkalmazás rendelkezik egy másik felhasználó célközönséget, csatlakozhat, hogy az összekötők másik tartomány használatára.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Egyszeri bejelentkezés használata esetén a helyszíni és felhőbeli identitások nem azonosak
Kivéve, ha egyébként konfigurálva, a szolgáltatásalkalmazás-Proxy feltételezi, hogy a felhasználó rendelkezik pontosan ugyanazt a felhőben, és a helyszíni identitás. Beállíthatja, az egyes alkalmazások, mely identitás kell használni, amikor az egyszeri bejelentkezés hajt végre.  

Ezt a funkciót lehetővé teszi, hogy sok szervezetek, amelyek különböző helyszíni és felhőbeli identitások, hogy az egyszeri bejelentkezés a felhőből helyszíni alkalmazások anélkül, hogy a felhasználók számára, írja be a másik felhasználóneveket és jelszavakat. Ide tartoznak a szervezetek, amely:

- Több tartománya van, belső (joe@us.contoso.com, joe@eu.contoso.com) és a felhőben egyetlen tartomány (joe@contoso.com).

- Belső van nem átirányítható tartománynév (joe@contoso.usa) és jogi egy, a felhőben.

- Ne használja a belső tartománynevek (Mintaügyvezető)

- Használata különböző aliasok helyszíni és felhőbeli. Pl. joe-johns@contoso.comviewben.joej@contoso.com  

Azt is segít nem fogadja el az e-mail címet, amely a Windows-kódmentes kiszolgálóinak gyakori eset formájában címek alkalmazásokkal.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Egyszeri bejelentkezés beállítása a különböző felhő és a helyszíni identitások

1. Azure AD Connect beállításainak konfigurálása, így a fő identitás lesz az e-mail címet (levelezés). Ez történik a Testreszabás folyamat részeként a **Felhasználói egyszerű név** mező szinkronizálási beállításainak módosításával. Megjegyzés:, hogy ezek beállításai határozzák meg is hogyan felhasználók jelentkezzen be az Office 365-ben, Windows10 eszközök, és egyéb, használja az Azure Active Directory az identitás áruházból alkalmazásokat.  
  ![Azonosító felhasználók képernyőképe - felhasználóneve legördülő menü](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Az alkalmazás beállításait módosítani szeretné az alkalmazást jelölje be a **Bejelentkezési identitást meghatalmazott** használható:
  - Felhasználónév elve szerint:joe@contoso.com  
  - Alternatív felhasználónév elve szerint:joed@contoso.local  
  - Felhasználónév elve felhasználónév részét: Mintaügyvezető  
  - Alternatív elve felhasználónév felhasználónév részét: joed  
  - A helyszíni SAM fióknév: attól függően, helyszíni tartomány vezérlő beállítása

  ![Delegált bejelentkezési azonosító legördülő menü képe](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Különböző identitások hibaelhárítási egyszeri bejelentkezés
Ha hiba lép fel, az egyszeri bejelentkezési folyamat meg kíván jeleníteni a összekötő gépi eseménynaplójának [Hibaelhárítás](active-directory-application-proxy-troubleshoot.md)leírtak szerint.
De bizonyos esetekben a kérelem sikeresen küld az kódmentes alkalmazás amíg ez az alkalmazás válaszol a különböző HTTP-válaszok. Ezekben az esetekben hibaelhárítási vizsgálata esemény száma 24029 a szolgáltatásalkalmazás-Proxy munkamenet eseménynaplójában összekötő gépen kell először. A meghatalmazás használt felhasználóazonosító megjelennek a "felhasználó" mező belül az esemény részleteit. Munkamenet napló bekapcsolásához jelölje be **elemző megjelenítése és elháríthatja a naplók** abban az esetben, ha a Nézet menüben megjelenítő.


## <a name="see-also"></a>Lásd még:

- [Szolgáltatásalkalmazás-Proxy-alkalmazások közzététele](active-directory-application-proxy-publish.md)
- [Szolgáltatásalkalmazás-Proxy tapasztal kapcsolatos problémák megoldása](active-directory-application-proxy-troubleshoot.md)
- [Követelések tartsa szem előtt alkalmazások használata](active-directory-application-proxy-claims-aware-apps.md)
- [Feltételes elérésének engedélyezése](active-directory-application-proxy-conditional-access.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
