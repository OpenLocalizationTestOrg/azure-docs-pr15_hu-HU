<properties
   pageTitle="Az alkalmazás az Azure Active Directory-alkalmazás gyűjteményben listázása"
   description="Hogyan listája, amely támogatja az egyszeri bejelentkezés az Azure Active Directory-gyűjteményben egy alkalmazás |} Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Az alkalmazás az Azure Active Directory-alkalmazás gyűjteményben listázása

Egy alkalmazás, amely támogatja az egyszeri bejelentkezés az Azure Active Directory címtárral az [Azure Active Directory-gyűjtemény](https://azure.microsoft.com/marketplace/active-directory/all/)listáját, az alkalmazás első kell végrehajtja a következő integrációs módok közül:

* **Csatlakozás OpenID** - közvetlen funkciók integrálása az Azure AD-használata OpenID Csatlakozás hitelesítés és az Azure Active Directory hozzájárulása API konfiguráció. Ha csak egy integrációs indítja, és az alkalmazás nem támogatja a SAML, majd az az ajánlási mód.

* **SAML** – az alkalmazás már rendelkezik, az azt jelenti, hogy állítsa be a külső Identitásszolgáltatók a SAML protokoll használatával.

Egyes üzemmódokban bejegyzésére követelményei alatt találhatók.

##<a name="openid-connect-integration"></a>OpenID csatlakozás integrációja

Az alkalmazás integrálása az Azure Active Directory, a [Fejlesztőeszközök utasításokat](active-directory-authentication-scenarios.md)követve. Töltse ki az alábbi kérdésekre, majd küldje el waadpartners@microsoft.com.

* Küldje el a próba-bérlő vagy a fiók hitelesítő adatait az alkalmazás tesztelése integrálása az Azure Active Directory csoport által használható.  

* Hogyan az Azure Active Directory csoport jelentkezzen be és Azure Active Directory egy példányának csatlakoztatása az alkalmazást az [Azure Active Directory framework beleegyezés](active-directory-integrating-applications.md#overview-of-the-consent-framework)útmutatást. 

* Útmutatást bármilyen további szükséges az Azure Active Directory csoport tesztelje az egyszeri bejelentkezés az alkalmazással. 

* Adja meg az alábbi adatokat:

> A cég neve:
> 
> Vállalati webhely esetén:
> 
> Alkalmazás neve:
> 
> Alkalmazás leírása (256 karakteres korlátját):
> 
> Alkalmazás webhely (tájékoztató):
> 
> Alkalmazás technikai támogatási webhelyének vagy kapcsolati adatait:
> 
> Ügyfél-azonosító az alkalmazás, a https://manage.windowsazure.com alkalmazás részleteit látható módon:
> 
> Hol ügyfelek megnyitásához jelentkezzen be az alkalmazás előfizetési URL és/vagy vásárolja meg az alkalmazást:
> 
> Válassza az alkalmazás szeretné, hogy a (a kategóriák lásd a Azure Active Directory piactér) legfeljebb három kategóriába:
> 
> Csatolása kis alkalmazásikonjára (PNG-fájlt, 45px, ha egyszínű hátteret által 45px):
> 
> Csatolása nagy alkalmazásikonjára (PNG-fájlt, 215px, ha egyszínű hátteret által 215px):
> 
> Csatolása alkalmazás embléma (PNG-fájlt, 150px 122px, átlátszó háttérszín szerinti):

##<a name="saml-integration"></a>SAML-integráció

Minden olyan alkalmazás, amely támogatja a SAML 2.0-s is lehet integrálva közvetlenül egy Azure AD-bérlő [ezeket az utasításokat követve egyéni alkalmazás felvétele](active-directory-saas-custom-apps.md)segítségével. Tesztelése, hogy működik-e az alkalmazás integrálása az Azure Active Directory, után küldje el az alábbi információkat <waadpartners@microsoft.com>.

* Küldje el a próba-bérlő vagy a fiók hitelesítő adatait az alkalmazás tesztelése integrálása az Azure Active Directory csoport által használható.  

* A SAML bejelentkezési URL-CÍMÉT, a kibocsátó URL-cím (egyed-azonosító) és a válasz URL-cím (állítás fogyasztói szolgáltatás) értéket adja, mint leírt [ide](active-directory-saas-custom-apps.md)az alkalmazás. Ha a szokásos SAML metaadat-fájlt részeként ad meg ezeket az értékeket, majd küldjön, amely is.

* Adjon meg egy rövid leírást, állítsa be az alkalmazás használatáról a SAML 2.0-identitásszolgáltató Azure Active Directory hogyan. Ha az alkalmazás támogatja a konfigurálás Azure AD-identitásszolgáltató önkiszolgáló felügyeleti portál keresztül, majd ellenőrizze, hogy a fenti hitelesítő adatokat tartalmazzák az azt jelenti, hogy ezeket a beállításokat.

* Adja meg az alábbi adatokat:

> A cég neve:
> 
> Vállalati webhely esetén:
> 
> Alkalmazás neve:
> 
> Alkalmazás leírása (256 karakteres korlátját):
> 
> Alkalmazás webhely (tájékoztató):
> 
> Alkalmazás technikai támogatási webhelyének vagy kapcsolati adatait:
> 
> Alkalmazás előfizetési URL-címe ügyfelek jelentkezzen be az, hogy hová és/vagy vásárolja meg az alkalmazást:
> 
> Adja meg az alkalmazás szeretné, hogy a (a kategóriák lásd az [Azure Active Directory piactérről](https://azure.microsoft.com/marketplace/active-directory/)) akár három kategóriába):
> 
> Kis alkalmazásikonjára (PNG-fájlt, 45px, ha egyszínű hátteret által 45px) csatolhat:
> 
> Csatolása nagy alkalmazásikonjára (PNG-fájlt, 215px, ha egyszínű hátteret által 215px):
> 
> Csatolása alkalmazás embléma (PNG-fájlt, 150px 122px, átlátszó háttérszín szerinti):
