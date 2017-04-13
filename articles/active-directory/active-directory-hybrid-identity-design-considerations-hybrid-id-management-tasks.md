<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontok – határozza meg a hibrid identitás adatkezelési feladatok |} Microsoft Azure"
    description="Feltételes hozzáférés-vezérlés az Azure Active Directory ellenőrzi az adott feltételeknek választhat, amikor a felhasználó hitelesítése és előtt, hogy az alkalmazás hozzáférést. Ha ezen feltételek teljesülése esetén a felhasználó a hitelesített és az alkalmazás férhet hozzá."
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

# <a name="plan-for-hybrid-identity-lifecycle"></a>Hibrid identitás életciklus megtervezése 

Identitás egyike, a vállalati mobilitás és az alkalmazás hozzáférést stratégiájának alapjainak. Akár a mobileszköz és a szoftver alkalmazás fizet, a személyazonosság minden hozzáférjenek a billentyűt. A legfelső szinten egy identitáskezelési megoldást lefedi a egységesítésére használható, és az Identitáskezelés tárházakban automatizálása és erőforrások kiépítési folyamata összegyűjtő tartalmazó közötti szinkronizálást. Az identitás-megoldás egy központi identitás kell a helyszíni és felhőbeli és segítségével is identitás összevonási valamilyen központi hitelesítési karbantartása biztonságosan megoszthatja és külső felhasználók és vállalkozások együttműködéshez. Erőforrások operációs rendszerek és a személyek az alkalmazások között, vagy viszonyban egy szervezet. Szervezeti struktúra is változtatható meg a kiépítési házirendek és eljárásokat igazodik.

Érdemes Emellett fontos, hogy az identitás megoldás kitűző a felhasználók kapjon az önkiszolgáló élményt megtartása hatékonyabban megadásával. Az identitás megoldás, megbízhatóbb, ha egyszeri bejelentkezés a felhasználók számára lehetővé teszi a rendszergazdák minden eléréséhez szükséges összes erőforrás között szintek segítségével szabványos eljárások felhasználói hitelesítő adatok kezelése. A felügyeleti egyes szintek csökkentik, illetve alverzió, attól függően, hogy a kiépítési projektmenedzsment megoldás szélessége is. Ezenkívül biztonságosan oszthat felügyeleti lehetőségeket, automatikus vagy kézi különböző szervezetek között. Ha például a tartományi rendszergazdája szolgálhat csak a személyek és erőforrások az adott tartományban. Ennek a felhasználónak rendszergazdai és kiépítési műveleteket hajthat végre, de nem engedélyezett a végezze el a feladatokat, például a munkafolyamatok létrehozása.


## <a name="determine-hybrid-identity-management-tasks"></a>Hibrid identitás adatkezelési feladatok meghatározása
A szervezet felügyeleti feladatok terjesztése javítja a pontosságát és felügyeleti hatékonyságát, és javítja a terhelést a szervezet egyenlege. Az alábbiakban a alapú kimutatások egy robusztus identitáskezelő rendszer meghatározó.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Hibrid identitás adatkezelési feladatok definiálásához látnia kell a szervezet, amely a hibrid identitás elfogadása fog néhány alapvető jellemzői. Fontos, hogy a használt identitás források aktuális tárházakban megértéséhez. Kívánt ismeretében core elemekre, eligazodást követelmények lesz, és alapján, hogy meg kell, hogy jobban tervezés döntés a identitás megoldás vezet, finomabb kérdéseket.  

Közben definiálásáról a szóban forgó követelményeket, győződjön meg arról, hogy legalább az alábbi kérdések és válaszok

- A kiépítési beállításai: 
 - A hibrid identitás megoldást támogatja a hozzáférés-kezelés partnerkezelési és létesítési rendszerével?
 - Hogyan azok a felhasználók, csoportok és a jelszavakat, látogasson el a kezelhető?
 - Az identitás életciklus-kezelése megszakítására? 
      - Mennyi ideig jelszó frissítések fiók felfüggesztése tart?
      
- Licenc kezelése: 
 - A hibrid megoldás fogópontok licencet az Identitáskezelés nem?
     - Ha igen, milyen funkciókat érhetők el?
- Nem a megoldást kezelése csoport alapú licenc management? 
      – Ha igen, az esetleges biztonsági csoport hozzárendelése? 
       – Ha igen, a felhőben könyvtár automatikusan hozzárendel licencek a csoport összes tagja? 
        – Mi történik, ha a felhasználók később hozzáadni, vagy a csoportból eltávolítani, fog licenc automatikusan rendelt vagy eltávolítása szükség szerint? 

- Egyéb külső Identitásszolgáltatók integrálása:
- A hibrid megoldás integrálhatók külső Identitásszolgáltatók végrehajtásához az egyszeri bejelentkezés?
- Van erre lehetőség, hogy az összes különböző Identitásszolgáltatók össze nem összefüggő identitás rendszerbe?
- Ha igen, hogyan, és melyek azok, és milyen funkciók állnak rendelkezésére?

## <a name="synchronization-management"></a>Szinkronizálás kezelése
Szinkronizált engedélyezni szeretné a Identitásszolgáltatók hozása és tarthatja őket egy identitáskezelő a kitűzött célok közül. A szinkronizált adatok megtartani mérvadó fő identitásszolgáltató alapján. Hibrid azonosító, és egy szinkronizált management modellt helyzetben egy helyszíni kiszolgálót az összes felhasználó és eszköz identitás kezelése, és a fiókok és, ha szükséges, jelszavakat a felhőbe szinkronizálása. A felhasználó az azonos jelszó helyszíni ad meg, hogy kikkel jelent, a felhőben, illetve a bejelentkezés, a jelszót az identitás megoldás által ellenőrizve. Ez a modell címtár-Szinkronizáló eszköz használja.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Nagy tervezési a szinkronizálás a hibrid identitás megoldás győződjön meg arról, hogy a következő kérdések és válaszok: • Mik azok a hibrid identitás megoldás a szinkronizálási megoldások?
• Mik azok a rendelkezésre álló lehetőségek egyszeri bejelentkezéses?
• Mik a lehetőségek identitás-összevonáshoz B2B és B2C között?

## <a name="next-steps"></a>Következő lépések
[Hibrid identitás kezelése átálláshoz meghatározása](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)

