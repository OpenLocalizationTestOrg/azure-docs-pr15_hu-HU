<properties
    pageTitle="Azure feltételes Access-alkalmazások szoftver |} Microsoft Azure"
    description="Az Azure Active Directory feltételes access lehetővé teszi, hogy alkalmazásonként többtényezős hitelesítés hozzáférési szabályok és az azt jelenti, hogy tiltani a nem megbízható hálózaton felhasználók hozzáférésének konfigurálását. "
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Azure Active Directory feltételes Access – első lépések

Azure Active Directory feltételes Access- [szoftver](https://azure.microsoft.com/overview/what-is-saas/) alkalmazást, és Azure AD össze apps lehetővé teszi, hogy csoport, helyét és alkalmazás magánjellegű alapuló feltételes hozzáférést beállíthatja. 

Alkalmazás magánjellegű alapuló feltételes hozzáféréssel többtényezős hitelesítés (MFA) hozzáférési szabályok egy alkalmazást beállíthatja. MFA egy alkalmazás lehetővé teszi a felhasználóknak, akik nem megbízható hálózaton hozzáférésének letiltása az. Az alkalmazás, vagy csak a megadott biztonsági csoportok belüli felhasználók rendelt összes felhasználó MFA szabályok alkalmazhat.  Felhasználók lehet zárni MFA kötelező, ha az alkalmazás az IP-címet, amely a szervezet hálózatán belül férnek hozzá.

Ezek a funkciók érhetők el, amely az Azure Active Directory prémium licencet vásárolt ügyfeleknek.

## <a name="scenario-prerequisites"></a>Forgatókönyv vonatkozó követelmények
* Licenc az Azure Active Directory prémium verzió

* A szövetséges vagy felügyelt Azure Active Directory-bérlői

* A szövetséges bérlők kell engedélyezni, hogy többtényezős hitelesítés.

## <a name="configure-per-application-access-rules"></a>Alkalmazás hozzáférési szabályok beállítása

Ez a szakasz ismerteti a állítsa be az alkalmazás hozzáférési szabályokat.

1. Jelentkezzen be az Azure klasszikus portált, amely az Azure Active Directory egy globális rendszergazdai fiókkal.
2. A bal oldali ablaktáblában jelölje ki az **Active Directory**.
3. A címtár lapon jelölje ki a címtárban.
4. Jelölje ki az **alkalmazások** fülre.
5. Jelölje ki az alkalmazást, a szabály beállítása.
6. Kattintson a **beállítás** fülre.
7. Görgessen le a hozzáférési szabályok szakaszában. Jelölje ki a kívánt hozzáférési szabályokat.
8. Adja meg, hogy a felhasználók a szabály érvényesek lesznek.
9. Engedélyezze a házirendet **engedélyezve kell az**kiválasztásával.

##<a name="understanding-access-rules"></a>Hozzáférési szabályok ismertetése

Ez a szakasz részletes leírását a hozzáférési szabályok támogatja az Azure feltételes alkalmazás hozzáférést biztosít.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Azok a felhasználók a hozzáférési szabályok alkalmazása

Alapértelmezés szerint a házirend az alkalmazás hozzáféréssel rendelkező összes felhasználó érvényesek lesznek. Azonban is korlátozhatja a házirendet, amelyek a megadott biztonsági csoportok tagjai számára. Jelölje ki egy vagy több csoportot, amely a hozzáférési szabályok érvényesek lesznek a csoport kiválasztása párbeszédpanel szolgál a **Csoport hozzáadása** gombra. Ez a párbeszédpanel a kijelölt csoport eltávolítása is használható. A szabály alkalmazása a csoportok kijelölésekor a hozzáférési szabályok csak szabályon a felhasználóknak, hogy a megadott biztonsági csoportok egyik tagja.

Biztonsági csoportok is is kell kifejezetten zárni a házirendet a **kivételével** lehetőséget, és egy vagy több csoportok megadásával. Felhasználók, amelyek egy csoportba, **kivéve a** listában nem esetektől többtényezős hitelesítés kötelező, akkor is, ha egy csoportba, amely az access szabály vonatkozik.
Az alább látható módon hozzáférési szabályokat a menedzserek csoport többtényezős hitelesítést használ, amikor az alkalmazás minden felhasználónak van szükség.

![A MFA feltételes hozzáférési szabályok beállítása](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>MFA feltételes Access szabályokkal
Felhasználó van konfigurálva, a felhasználónkénti többtényezős hitelesítés funkció használata, ha ezt a beállítást a felhasználó kombinálni fogja a többtényezős hitelesítés szabályokat, az alkalmazás. Ez azt jelenti, hogy egy felhasználó felhasználónkénti többtényezős hitelesítéshez konfigurált kötelező többtényezős hitelesítés végrehajtani, akkor is, ha az alkalmazás többtényezős hitelesítés szabályok alól. További tudnivalók a többtényezős hitelesítés és a felhasználói beállításokat.

### <a name="access-rule-options"></a>A szabály az Access beállításai
A következő lehetőségek használhatók:

* **Többtényezős hitelesítés szükséges**: A felhasználókat, akinek a hozzáférési szabályokat alkalmazni, az alkalmazás, amely a házirend vonatkozik megnyitása előtt teljes többtényezős hitelesítés szükséges lesz.

* **Ha nem a munkahelyén többtényezős hitelesítést igényel**: elérhető lesz a megbízható IP-címet a felhasználó nem kötelező többtényezős hitelesítéshez. A megbízható IP-címtartományok beállítható a többtényezős hitelesítés beállításai lapon.

* **Ha nem a munkahelyén hozzáférésének letiltása**: A felhasználó nem származó egy megbízható IP-címet, azzal blokkolhatja. A megbízható IP-címtartományok beállítható a többtényezős hitelesítés beállításai lapon.

### <a name="setting-rule-status"></a>A szabály állapot beállítása
Az Access szabály állapota lehetővé teszi, hogy a szabályok be- és kikapcsolhatja. Ha a hozzáférési szabályok ki van kapcsolva, a többtényezős hitelesítés megkövetelésének nem lép életbe.

### <a name="access-rule-evaluation"></a>Az Access szabály értékelése

Amikor a felhasználó hozzáfér egy összevont alkalmazás OAuth 2.0-s verziója, a OpenID csatlakozni, a SAML vagy a Webszolgáltatás-összevonás használó hozzáférési szabályok értékeli ki. Ezeken kívül hozzáférési szabályok kiértékelése használatakor az OAuth 2.0-s és OpenID csatlakoztatása a frissítés jogkivonat szerezheti be egy hozzáférési jogkivonat. Házirend-értékelést sikertelen, amikor az egy frissítés jogkivonat, ha a hiba **invalid_grant** vissza kell, az azt jelenti, hogy a felhasználónak kell újból hitelesíteni az ügyfél számára.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Többtényezős hitelesítés megadására összevonási szolgáltatások beállítása:

A szövetséges bérlők, a program Azure Active Directory vagy a helyszíni MFA végezhetik AD FS-kiszolgáló.

Alapértelmezés szerint az weblapon Azure Active Directory által üzemeltetett MFA lépnek fel. Helyszíni MFA konfigurálásához az **– SupportsMFA** tulajdonság meg kell **Igaz** az Azure Active Directory, az Azure Active Directory modul Windows Powershellhez készült használatával.

A következő példa bemutatja, hogyan engedélyezheti a helyszíni MFA a contoso.com címen működő bérlőn a [Set-MsolDomainFederationSettings parancsmag](https://msdn.microsoft.com/library/azure/dn194088.aspx) használatával:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

A jelző kívül, az összevont bérlőjén az AD FS-példány kell beállítania, hogy többtényezős hitelesítéshez. Kövesse az utasításokat az [Azure többtényezős hitelesítés a helyszíni telepítésével](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Kapcsolódó cikkek

- [Azure Active Directory csatlakozik az Office 365-ben és más alkalmazások való hozzáférés biztonságossá tétele](active-directory-conditional-access.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
