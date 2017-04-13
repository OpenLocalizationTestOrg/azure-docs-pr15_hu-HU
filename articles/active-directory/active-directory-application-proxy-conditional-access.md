<properties
    pageTitle="Feltételes Access alkalmazáshoz közzétett Azure Active Directory szolgáltatásalkalmazás-Proxy"
    description="Bemutatja, hogy miként állíthatja be a feltételes access-alkalmazásokhoz közzéteheti Azure Active Directory szolgáltatásalkalmazás-Proxy segítségével távolról is elérhető."
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
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Feltételes access használata

Hozzáférési szabályok való hozzáférést feltételes alkalmazások szolgáltatásalkalmazás-Proxy használatával közzé is beállíthatja. Lehetővé teszi, hogy:

- Egy alkalmazás többtényezős hitelesítést igényel
- Többtényezős hitelesítés szükséges, csak akkor, amikor a felhasználók nem rendelkeznek a munkahelyén
- Felhasználók hozzáférjenek a az alkalmazás, ha nem működő letiltása

Szabályok alkalmazható az összes felhasználókhoz és csoportokhoz, vagy csak a megadott felhasználók és csoportok. Alapértelmezés szerint a szabály alkalmazása hozzáféréssel rendelkező összes felhasználó érvényesek lesznek. Azonban a szabály is korlátozhatja, amelyek a megadott biztonsági csoportok tagjai számára.  

Amikor a felhasználó hozzáfér egy összevont alkalmazás OAuth 2.0-s verziója, a OpenID csatlakozni, a SAML vagy a Webszolgáltatás-összevonás használó hozzáférési szabályok értékeli ki. Ezeken kívül hozzáférési szabályok kiértékelése OAuth 2.0-s és OpenID csatlakozás frissítés jogkivonat szerezheti be egy hozzáférési jogkivonat segítségével.

## <a name="conditional-access-prerequisites"></a>Feltételes access vonatkozó követelmények

- Azure Active Directory prémium előfizetés
- A szövetséges vagy felügyelt Azure Active Directory bérlői webhelyre
- A szövetséges bérlők kell engedélyezni, hogy többtényezős hitelesítés (MFA)  
    ![Állítsa be a hozzáférési szabályok - többtényezős hitelesítést igényel](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Alkalmazás többtényezős hitelesítés beállítása
1. Jelentkezzen be rendszergazdaként az Azure klasszikus portálon.
2. Nyissa meg az Active Directory, és jelölje ki a könyvtárat, amelyben a szolgáltatásalkalmazás-Proxy engedélyezni szeretné.
3. Kattintson az **alkalmazások** , és görgessen le a **Hozzáférési szabályok** szakaszban. A hozzáférési szabályok szakasz csak akkor jelenik meg a szolgáltatásalkalmazás-Proxy használatával közzé szövetséges hitelesítést használó alkalmazások.
4. A szabály engedélyezése a **Hozzáférési szabályok engedélyezése** **a**kiválasztásával.
5. Adja meg a felhasználók és csoportok, akinek a szabályok érvényesek lesznek. A **Csoport hozzáadása** gombra használatával jelölje ki a hozzáférési szabály vonatkozni fog egy vagy több csoportokat. Ez a párbeszédpanel a kijelölt csoport eltávolítása is használható.  A szabály alkalmazása a csoportok kijelölésekor a hozzáférési szabályok csak szabályon a felhasználóknak, hogy a megadott biztonsági csoportok egyik tagja.  

  - Biztonsági csoportok kifejezetten zárni a szabályt, **kivéve,** jelölje be, és adja meg a egy vagy több csoportot. A felhasználók, akik a kivéve listában a csoport tagjai nem szükséges többtényezős hitelesítéshez.  

  - Ha egy felhasználó konfigurálták felhasználónként többtényezős hitelesítés funkcióval, ezt a beállítást elsőbbséget az alkalmazás többtényezős hitelesítés szabályokat. Ez azt jelenti, hogy a felhasználók, akik felhasználónkénti többtényezős hitelesítéshez konfigurált lesz, ha az alkalmazás többtényezős hitelesítés szabályok a alól többtényezős hitelesítés elvégzéséhez szükséges. További tudnivalók a [többtényezős hitelesítés és a felhasználói beállításokat](../multi-factor-authentication/multi-factor-authentication.md).

6. Jelölje ki a hozzáférési szabály meg szeretné adni:
    - **Megkövetelése többtényezős hitelesítés**: a felhasználókat, akinek a hozzáférési szabályok vonatkoznak kell teljes többtényezős hitelesítés megnyitása a szabály alkalmazása előtt.
    - **Megkövetelése többtényezős hitelesítés, ha nem a munkahelyén**: elérni kívánt az alkalmazás egy megbízható IP-címet a felhasználók nem kötelező többtényezős hitelesítéshez. A megbízható IP-címtartományok beállítható a többtényezős hitelesítés beállításai lapon.
    - **Ha nem a munkahelyén hozzáférésének letiltása**: elérni kívánt az alkalmazás a vállalati hálózatán kívülről a felhasználók nem tudnak hozzáférni az alkalmazást.


## <a name="configuring-mfa-for-federation-services"></a>MFA konfigurálása az összevonási szolgáltatások számára
A szövetséges bérlők, a program Azure Active Directory vagy a helyszíni többtényezős hitelesítés (MFA) végezhetik AD FS-kiszolgáló. Alapértelmezés szerint MFA akkor fordul elő, az egyik Azure Active Directory által üzemeltetett lapon. Annak érdekében, hogy a helyszíni MFA adja meg, futtassa a Windows PowerShell, és – SupportsMFA tulajdonságot használja az Azure Active Directory modul beállítása.

A következő példa bemutatja, hogyan engedélyezheti a helyszíni MFA a contoso.com címen működő bérlőn a [Set-MsolDomainFederationSettings parancsmag](https://msdn.microsoft.com/library/azure/dn194088.aspx) használatával:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

A jelző kívül, az összevont bérlőjén az AD FS-példány kell beállítania, hogy többtényezős hitelesítéshez. Kövesse az utasításokat a [Microsoft Azure többtényezős hitelesítés helyszíni telepítésével](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).


## <a name="see-also"></a>Lásd még:

- [Követelések tartsa szem előtt alkalmazások használata](active-directory-application-proxy-claims-aware-apps.md)
- [Szolgáltatásalkalmazás-Proxy-alkalmazások közzététele](active-directory-application-proxy-publish.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [A saját tartománynév alkalmazások közzététele](active-directory-application-proxy-custom-domains.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
