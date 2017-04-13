<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - identitás követelmények meghatározása |} Microsoft Azure"
    description="Azonosítsa a vállalat az üzleti igények vezető adhatja meg a hibrid identitás Tervező követelményei."
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

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitás megoldás identitás követelmények meghatározása
Az első lépés a hibrid identitás megoldás tervezése határozza meg, hogy a megoldás vizsgál lesz az üzleti szervezet követelményei.  Hibrid identitás (támogatott felhő minden más megoldást, mert a hitelesítés) támogató szerepkör elindul, és Ugrás a adja meg a felhasználóknak új munkaterhelésekből zárolásának feloldása az új és érdekes funkcióját.  Ezek a munkaterhelésekből vagy szolgáltatások használni kívánt fogadja el a felhasználók számára a hibrid identitás Tervező követelményei szabja.  Ezek a szolgáltatások és munkaterhelésekből kell kihasználhatja a hibrid identitás mindkét a helyszíni és felhőbeli.  

Meg kell adni át ezeket a fő szempontokat megértéséhez, hogy mi az üzleti most annak követelménye, és mi a vállalati tervet, a jövőben. Ha még nincs telepítve a hosszú távú stratégia hibrid identitás Tervező láthatóságát, minden bizonnyal, hogy a megoldás nem lesz méretezhető az üzleti igények a nagyobb és módosítása.   A T általa-diagram alatt látható példa egy hibrid identitás architektúrája és a munkaterhelésekből, hogy a felhasználók számára is készül zárolását. Az összes új lehetőség, amely nem zárolt, és az egyszínű hibrid identitás stratégia kézbesítése egy példája. 
 
A hibrid identitás architektúra részét képező bizonyos összetevői![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Az üzleti igények meghatározása
Egyes vállalatok eltérő követelményei, lesz, még akkor is, ha ezek a vállalatok azonos üzleti, a követelmények eltérőek lehetnek a valós business részét képezik. Továbbra is élvezheti gyakorlati tanácsok a az üzleti, de végül a vállalat az üzleti igények vezető adhatja meg a hibrid identitás Tervező követelményei. 

Győződjön meg arról, hogy az üzleti igények azonosítja az alábbi kérdésekre választ:

- A vállalat keresi, kivágáshoz informatikai működési költség?
- A vállalat keresi biztonságos, felhőalapú eszközök (szoftver alkalmazások, infrastruktúra)?
- A vállalat keresi az informatikai korszerűsítésére?
  - A felhasználók számára, hogy több mobil és nagy informatikai hozhat létre a kivételek engedélyezése a különböző típusú más forrásokat-alapú forgalmat a DMZ-be?
  - A vállalat rendelkezik a szükséges modern felhasználóknak tehetők közzé, de nem egyszerűen átírásának régebbi alkalmazásai?
  - Nem a vállalat kell elvégezni ezeket a feladatokat és hozása felügyelheti egyszerre?
- Felhasználói identitások biztonságos és a kockázat csökkentése érdekében új eszköz, amelyek a Microsoft Azure biztonsági szakértelmet a helyszíni tapasztalatok kihasználhatja keresi a vállalat?
- A vállalat próbál helyszíni dreaded "külső" fiókok volna, és helyezze át őket a felhőben, ahol már nincs belül a helyszíni környezet inaktív veszélyt?

## <a name="analyze-on-premises-identity-infrastructure"></a>A helyszíni identitás infrastruktúra elemzése
Most, hogy miben kapcsolatban a vállalat üzleti igényeinek megfelelően, kell a helyszíni identitás infrastruktúra felmérése. A kiértékelés fontos integrálása a felhőben identitáskezelő rendszer az aktuális azonosító megoldási műszaki követelmények meghatározására. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- Milyen hitelesítési és engedélyezési megoldást jelent a vállalat helyszíni? 
- A vállalat jelenleg rendelkezik a helyszíni szinkronizálási szolgáltatások?
- Használja a vállalat bármely külső Identitásszolgáltatók (IdP)?

Meg kell tartsa szem előtt a felhőalapú szolgáltatások, amelyek a vállalat lehet. Az aktuális integráció a szoftver, IaaS vagy PaaS modellek a környezetben megértéséhez értékelést elvégzéséhez nagyon fontos. Győződjön meg arról, hogy az alábbi kérdésekre választ a felmérés során:
- A vállalat rendelkezik egy felhőalapú szolgáltatónál bármilyen integrációt?
- Ha igen, mely szolgáltatások használják?
- Ez az integráció jelenleg gyártási vagy próbaüzem?


>[AZURE.NOTE]
Ha nem az összes alkalmazás egy pontos hozzárendelés és cloud services, az a felhő alkalmazás feltárás eszköz is használhatja. Ez az eszköz betekintést kap abba, hogy a szervezet üzleti és a fogyasztói felhő alkalmazások biztosíthat az informatikai részleggel. Amely megkönnyíti az mint minden eddiginél felderítési történő árnyék informatikai a szervezetéhez, beleértve a szokásai és az összes felhasználó elérése a felhőben alkalmazások tájékoztatást. Hozzáférés Ez az eszköz használja a [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Identitás integrációs követelmények felmérése
Következő lépésként meg kell az identitás-integráció követelmények felmérése. A kiértékelés fontos, hogy hogyan fog hitelesíteni a felhasználóknak, hogyan fog kinézni a szervezet jelenléti a felhőben, hogyan lehetővé teszi a szervezet engedély és a felhasználói felület okok kell technikai követelményei határozza meg. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- A szervezet használni összevonási, a szabványos hitelesítés és a mindkettő?
- Követelmény jó a szövetség?  Miatt a következőket:
 - Kerberos-alapú egyszeri bejelentkezés
 - A vállalat rendelkezik egy (vagy a belső vagy 3 fél épült) a helyszíni alkalmazások SAML vagy hasonló összevonási funkciók használó.
 - Intelligens kártya keresztül MFA. RSA SecurID stb.
 - Az alábbi kérdésekre választ hozzáférési szabályok ügyfél:
     1. Letilthatja az Office 365, az ügyfél IP-címe alapján a külső hozzáférést?
     1. Megakadályozhatja a Office 365-ben, kivéve az Exchange ActiveSync-alapú külső hozzáférést?
     1. Megakadályozhatja a minden külső hozzáférést az Office 365-be, kivéve a böngészőalapú alkalmazásokat (az Outlook Web App, a SharePoint online)
     1. Lehet-e letiltani Office 365-ben a külső hozzáférést kijelölt Active Directory-csoportok tagjai
- Biztonsági és naplózás szempontok
- Már meglévő befektetés szövetséges hitelesítés
- Milyen nevet tagként a szervezet használja a tartományt a felhőben?
- Van-e a szervezet egyéni tartományt?
    1. Nyilvános és könnyen ellenőrizhető DNS keresztül, akkor azt a tartományt?
    1. Ha nem, majd van egy alternatív egyszerű Felhasználónévi regisztrálása az Active Directory használt nyilvános tartomány?
- A felhasználói azonosítóját egységes felhő megjelenítéséhez vannak? 
- Van-e a szervezet integráció a felhőszolgáltatások igénylő alkalmazások?
- Nem a szervezet több tartománya van, és fogja azokat az összes normál vagy szövetséges hitelesítést használ?

## <a name="evaluate-applications-that-run-in-your-environment"></a>A környezetben futtatott alkalmazások felmérése
Most, hogy arról kapcsolatban a helyszíni és felhőbeli infrastruktúra, kell az alábbi környezetben futtatott alkalmazások felmérése. A kiértékelés fontos, hogy ezeket az alkalmazásokat a felhőben identitáskezelő rendszer integrálása az alább határozza meg. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- Hol fog live az alkalmazások?
- Felhasználók érik el a helyszíni környezetbe applications?  Az a felhő? Vagy mindkettő?
- Vannak olyan tervezi, hogy a meglévő alkalmazás feladatok készíthet, és helyezze át őket az a felhő?
- Vannak olyan, vagy a helyszíni tárolnak a felhőben, amelyekkel a cloud új alkalmazások fejlesztéséhez csomagok hitelesítést?

## <a name="evaluate-user-requirements"></a>Felhasználói követelmények felmérése
Akkor is, a felhasználói követelmények ki szeretné számítani. A kiértékelés fontos, hogy a családi és segítik a felhasználókat, ahogy azok a felhőbe átmenet szükséges lépések megadása. Győződjön meg arról, hogy az alábbi kérdésekre választ:

- Felhasználók érik el helyszíni alkalmazásokat?
- Felhasználók érik el a felhőben alkalmazásokat?
- Hogyan általában a felhasználókat a helyszíni környezetbe login?
- Hogyan fog felhasználók bejelentkezés az a felhő?

>[AZURE.NOTE]
Győződjön meg róla, hogy minden válasz jegyzeteket, és megértette az answer logikája. [Meghatározása az esemény választ követelmények](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) a program elküldi a rendelkezésre álló beállítások és a szakemberek és negatívumokat az egyes beállítások fölé.  A fenti kérdések kiválasztott legjobb lehetőség illik üzleti problémákat megválaszolt szerint szükséges.

## <a name="next-steps"></a>Következő lépések
[Címtár-szinkronizálás követelmények meghatározására](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)
