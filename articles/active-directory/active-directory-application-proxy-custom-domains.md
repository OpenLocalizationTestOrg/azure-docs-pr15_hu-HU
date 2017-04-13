<properties
    pageTitle="Az Azure Active Directory szolgáltatásalkalmazás-Proxy egyéni tartományok használata |} Microsoft Azure"
    description="Fedőlap az Azure Active Directory szolgáltatásalkalmazás-Proxy egyéni tartományok hogyan dolgozhat."
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

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Az Azure Active Directory szolgáltatásalkalmazás-Proxy egyéni tartományok használata

Az alapértelmezett tartomány használatával lehetővé teszi az azonos URL-cím beállítása a belső és külső URL-címet, az alkalmazás eléréséhez, így a felhasználók csak egy URL-cím ne feledje, hogy az alkalmazás mindegy, hol férnek hozzá a eléréséhez. Ez lehetővé teszi a egyetlen parancsikon létrehozása az Access panelen az alkalmazáshoz. Ha használja az alapértelmezett tartományt az Azure Active Directory szolgáltatásalkalmazás-Proxy által biztosított, van szüksége ahhoz, hogy a tartomány, nincs további beállítási. Abban az esetben, ha az egyéni tartomány használata, van néhány dolgot kell tennie, hogy arról, hogy a szolgáltatásalkalmazás-Proxy felismeri a tartományt, és ellenőrzi a tanúsítványok.

## <a name="selecting-your-custom-domain"></a>Az egyéni tartomány kijelölése

1. [Közzététel-alkalmazások szolgáltatásalkalmazás-Proxy](active-directory-application-proxy-publish.md)útmutatása szerint az alkalmazás közzé.
2. Után jelenik meg az alkalmazást az alkalmazáslistából, jelölje ki azt, és kattintson a **Konfigurálás**gombra.
3. A **Külső URL-CÍMÉT**írja be az egyéni tartomány.
4. Ha a külső URL-cím https, a rendszer kéri töltse fel a tanúsítvány úgy, hogy az Azure ellenőrizheti az alkalmazás URL-CÍMÉT. Töltse fel a megfelelő külső URL-CÍMÉT az alkalmazás által használt helyettesítő tanúsítvány is. Ebben a tartományban kell lennie az [Azure ellenőrzése a tartományok](https://msdn.microsoft.com/library/azure/jj151788.aspx)listájában. Azure tartományi URL-CÍMÉT az alkalmazást, vagy egy helyettesítő tanúsítvány, amely megfelel a külső URL-CÍMÉT az alkalmazás a tanúsítvánnyal kell rendelkeznie.
5. Ellenőrizze, hogy be a DNS-rekordot, amely a belső URL-cím továbbítja az alkalmazás, amely lehetővé teszi, hogy van-e belső és külső hozzáférés és az alkalmazás egy parancsikont egy URL-CÍMÉT a felhasználó alkalmazások listájában.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Gyakori kérdések a egyéni tartományok használata

K: már feltöltött tanúsítvány is kiválasztása nélkül tölt fel újra?  
A: korábban feltöltött tanúsítványok az alkalmazás automatikusan kötött, és az alkalmazás állomásnév egyező pontosan egy tanúsítványt.  

Kérdés: Hogyan adhatok hozzá egy tanúsítványt, és milyen formátumban kell az exportált tanúsítványt fel kell tölteni a?  
Megoldás: a tanúsítványt az alkalmazás beállítása lapon az fel kell tölteni. A tanúsítvány PFX fájl kell lennie.  

Kérdés: ECC tanúsítványok használható?  
V: nem aláírás módszerek explicit korlátozás nélkül.  

Kérdés: SAN tanúsítványok használható?  
V: Igen.  

Kérdés: helyettesítő tanúsítványok használható?  
V: Igen.  

Kérdés: egy másik tanúsítványt használható minden alkalmazás?  
Válasz: Igen, kivéve, ha a két alkalmazás megosztása ugyanarra a külső irányíthassák.  

Probléma: Ha új tartomány regisztrálásához van lehetőség, hogy a tartomány?  
Válasz: Igen, a tartományok listájában dokumentum a bérlő igazolt tartományt listából.  

K: Mi történik a tanúsítvány lejártakor?  
A: fog megjelenik egy figyelmeztetés az alkalmazás beállítása lapon a tanúsítvány szakaszában. Ha a felhasználó megpróbál az alkalmazás eléréséhez, biztonsági figyelmeztetés jelenik meg.  

Kérdés: milyen kell tenni, ha egy adott alkalmazás a tanúsítvány lecserélése szeretnék?  
Megoldás: töltse fel az alkalmazás beállítása lapon az új tanúsítványt.  

K: lehet törölni a tanúsítvány és helyére?  
A: feltöltésekor új tanúsítvány, ha a régi tanúsítványt nem használja-e egy másik alkalmazás, akkor automatikusan törli.  

K: Mi történik, amikor egy tanúsítvány visszavonták?  
A: visszavonási nem engedélyezné tanúsítványok. Ha a felhasználó megpróbál eléréséhez az alkalmazás, attól függően, hogy a böngészőben, biztonsági figyelmeztetés jelenhetnek meg.  

K: önaláírt tanúsítványt használni?  
Válasz: Igen, önaláírt tanúsítványok engedélyezett. Figyelje meg, hogy ha egy privát hitelesítésszolgáltató használ, a CRL terjesztési pont (tanúsítvány-visszavonási pont terjesztési pont) a tanúsítvány kell nyilvános.  

Kérdés: van-e a hely minden tanúsítvány megtekintéséhez a bérlői webhelyemhez?  
Megoldás: Ez a jelenlegi verziójában nem támogatott.  


## <a name="see-also"></a>Lásd még:

- [Szolgáltatásalkalmazás-Proxy-alkalmazások közzététele](active-directory-application-proxy-publish.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Feltételes elérésének engedélyezése](active-directory-application-proxy-conditional-access.md)
- [Azure AD az egyéni tartománynév hozzáadása](active-directory-add-domain.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
