<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - címtár-szinkronizálás követelmények meghatározására |} Microsoft Azure"
    description="Azonosítsa a szinkronizálás között a felhasználók milyen követelmények van szükség a = a helyszín és a vállalati felhő."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-directory-synchronization-requirements"></a>Címtár-szinkronizálás követelmények meghatározására
Szinkronizálás az összes kezeléséről a felhasználók a felhőben, a helyszíni személyazonosságot alapján identitás az. Attól függetlenül, hogy azok szinkronizált fiók vagy szövetséges hitelesítést fogja használni, a felhasználók továbbra is kell lennie az identitás a felhőben.  Ez az identitás kell tartani, és rendszeres időközönként frissülnek.  A frissítések többféle lehet, a jelszó módosítása címének módosítása.  

Indítsa el a szervezetek helyszíni identitás megoldás és felhasználói követelmények kiértékeléséből. A kiértékelés fontos, hogy hogyan felhasználói identitások létrejön és karbantartani a felhőben technikai követelményeinek meghatározása.  A legtöbb szervezetnek, az Active Directory a helyszíni és a helyszíni könyvtár, amelyeket a felhasználók által fognak szinkronizálja a azonban bizonyos esetekben ez nem lesz az esetet.  

Győződjön meg arról, hogy az alábbi kérdésekre választ:


- Van egy AD erdő, több és nincs?
 - Hány Azure AD-könyvtárak fog meg kell szinkronizál az?
 
    1. Használja szűrés?
    2. Több Azure AD Connect kiszolgálók tervezett vannak?
  
- Jelenleg van a szinkronizálást a helyszíni eszköz?
  - Ha igen, nem a felhasználók, ha a felhasználók egy virtuális directory /-integráció a azonosítók?
- Van bármilyen más címtárát helyileg (pl. LDAP-címtár, HR adatbázis stb) szinkronizálni kívánt?
  - Lesz a bármely GALSync lehet elvégezni?
  - Mi az, hogy a szervezet UPN aktuális állapotát? 
  - Van egy másik könyvtárra, hogy a felhasználók hitelesítést?
  - A vállalat használja a Microsoft Exchange?
    - Végezze el őket, hogy az exchange hibrid telepítés megtervezése?

Most, hogy arról a szinkronizálás vonatkozó követelményeket, meg kell határoznia, hogy melyik eszköze a megfelelő sem felel meg ezeknek a követelményeknek.  A Microsoft számos eszközt, hogy miként végezhet el a címtár-integráció és a szinkronizálás biztosít.  További információt a [hibrid identitás címtár integrációs eszközök összehasonlító táblázat](active-directory-hybrid-identity-design-considerations-tools-comparison.md) megjelenítéséhez. 
   
Most, hogy a szinkronizálás követelmények és a eszközt, amelyet a program miként végezhet el ez a cég számára, a következő címtárszolgáltatásaival használó alkalmazások felmérése szeretne. A kiértékelés fontos, hogy ezeket az alkalmazásokat a felhőbe integrálása az alább megadása. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- Ezeket az alkalmazásokat átkerül a felhőbe, és használja a címtárban?
- Vannak olyan speciális tulajdonságokat, amely letölthető a felhőbe, ezért ezeket az alkalmazásokat is használatuk sikeresen?
- Ahhoz, hogy újra kihasználhatja a felhőben auth kell ezeket az alkalmazásokat?
- Ezeket az alkalmazásokat továbbra is szerepelni fognak a helyszíni live, miközben felhasználók férhetnek hozzá őket a felhőben identitás használatával?

Is meg kell határoznia a biztonsági követelményeknek és korlátozásokkal címtár-szinkronizálás. A kiértékelés fontos, hogy a követelményeknek, létrehozása és kezelése a felhőben felhasználói identitások szükséges listájának. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- Ha a szinkronizálás kiszolgáló kerül?
- Lesz tartományhoz csatlakoztatott?
- A kiszolgáló helyezkednek el egy korlátozott hozzáférésű hálózati tűzfal, például egy DMZ?
  - Akkor fogja tudni nyissa meg a szükséges tűzfalportokat szinkronizálási támogatására?
- A szinkronizálás kiszolgáló katasztrófa helyreállítási tervet vannak?
- A szinkronizálni kívánt összes erdőkben a megfelelő jogosultsággal rendelkező fiók van?
 - Ha a vállalat nem tudja a válasz a kérdésre, tekintse át a szakasz "Jelszó-szinkronizálás engedélyeit" [telepítse az Azure Active Directory szinkronizáló szolgáltatás](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) című témakörben, és megállapíthatja, hogy ezeket az engedélyeket tartalmazó fiókkal már, vagy ha módosítani szeretné, hogy hozzon létre egy újat.
- Ha a mutli erdőn szinkronizálási eljuthassanak erdőkön, a szinkronizálási kiszolgáló?
 
>[AZURE.NOTE]
Győződjön meg róla, hogy minden válasz jegyzeteket, és megértette az answer logikája. [Meghatározása az esemény válasz követelmények](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) a program elküldi az elérhető lehetőségek fölé. A fenti kérdések kiválasztott legjobb lehetőség illik üzleti problémákat megválaszolt szerint szükséges.

## <a name="next-steps"></a>Következő lépések
[Többtényezős hitelesítés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)
