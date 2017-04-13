<properties
    pageTitle="Tartsa szem előtt alkalmazásainak szolgáltatásalkalmazás-Proxy jogcímalapú használata"
    description="Megtudhatja, hogy hogyan kezdeti lépéseket az Azure Active Directory szolgáltatásalkalmazás-Proxy."
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



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Tartsa szem előtt alkalmazásainak szolgáltatásalkalmazás-Proxy jogcímalapú használata

Tartsa szem előtt alkalmazások követelések szeretné a biztonsági jogkivonat szolgáltatás (STS), viszont kéri hitelesítő adatokat a felhasználó ellenében jogkivonat előtt, hogy a felhasználó átirányítása az alkalmazás, amely átirányítás végrehajtása Ahhoz, hogy a szolgáltatásalkalmazás-Proxy készült alábbi átirányítások, az alábbi lépéseket kell tenni.

## <a name="prerequisites"></a>Előfeltételek
A művelet elvégzése előtt győződjön meg róla, hogy a STS a követelések tartsa szem előtt alkalmazás képes a rendelkezésre álló a helyszíni hálózaton kívüli.

## <a name="azure-classic-portal-configuration"></a>Azure klasszikus portál beállításai

1. [Közzététel-alkalmazások szolgáltatásalkalmazás-Proxy](active-directory-application-proxy-publish.md)ismertetett útmutatása szerint az alkalmazás a Közzététel gombra.
2. Az alkalmazások listájában jelölje ki a követelések tartsa szem előtt alkalmazást, és kattintson a **Konfigurálás**gombra.
3. **Átadó** a **Előhitelesítés módot**választotta, győződjön meg róla, hogy a **Külső URL-cím** színsémák **HTTPS** .
4. **Azure Active Directory** a **Előhitelesítés módot**választotta, a **Belső hitelesítési módszer**válassza a **nincs** .


## <a name="adfs-configuration"></a>ADFS-konfiguráció

1. Nyissa meg az ADFS-kezelés.
2. Nyissa meg a **Fél megbízik használna**, kattintson jobb gombbal az alkalmazást, az alkalmazás Proxy tesz közzé, és válassza a **Tulajdonságok parancsot**.  
  ![Megbízó fél megbízik kattintson jobb gombbal az alkalmazás neve - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. A **Végpontok** lapon, a **Végpontok típusa**csoportban jelölje be a **Webszolgáltatás-összevonás**.
4. **URL-CÍMÉT megbízható** csoportban adja meg a **Külső URL-címe** alatt a szolgáltatásalkalmazás-Proxy a beírt URL-CÍMÉT, és kattintson az **OK gombra**.  
  ![Zárólap - hozzáadása URL-CÍMÉT megbízható értéke - kép beállítása](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Lásd még:

- [Szolgáltatásalkalmazás-Proxy-alkalmazások közzététele](active-directory-application-proxy-publish.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Szolgáltatásalkalmazás-Proxy tapasztal kapcsolatos problémák megoldása](active-directory-application-proxy-troubleshoot.md)
- [Natív ügyfele alkalmazások vezérléséhez proxy alkalmazások engedélyezése](active-directory-application-proxy-native-client.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
