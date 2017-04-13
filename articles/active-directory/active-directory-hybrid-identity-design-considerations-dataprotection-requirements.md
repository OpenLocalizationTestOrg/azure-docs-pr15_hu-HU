<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - adatok védelmére vonatkozó követelmények meghatározására |} Microsoft Azure"
    description="A hibrid identitás megoldás tervezésekor azonosítsa az adatok védelme követelményeiről vállalata, és milyen beállítások találhatók legjobb felel meg ezeknek a követelményeknek."
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

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Adatok biztonsági keresztül erős identitás megoldás fejlesztése megtervezése

Az első lépés az adatok védelme azonosítása, ki férhet hozzá az adatokat, és ez az identitás van szükség folyamatának részeként-megoldások is integrálódik a hitelesítési és engedélyezési lehetőségek megadására a rendszer fut. Hitelesítés és az engedélyezés gyakran zavaros egymással, és azok szerepköreit böngésző. A valóságban nem igazán egyeznek, az alábbi ábrán látható módon:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Mobileszköz management életciklusát ismertető szakaszok**

A hibrid identitás megoldás tervezésekor látnia kell az adatok védelmére vonatkozó követelmények a vállalkozás és milyen beállítások találhatók legjobb felel meg ezeknek a követelményeknek.
 
>[AZURE.NOTE]
Miután végzett az adatok biztonsági tervezéshez, tekintse át a [többtényezős hitelesítés követelmények meghatározására](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) annak érdekében, hogy azok a többtényezős hitelesítés követelmények vonatkozó beállítások nem érinti a ebben a részben elvégzett döntéseket.

## <a name="determine-data-protection-requirements"></a>Határozza meg az adatok védelmére vonatkozó követelmények
Mobility korát a legtöbb vállalat rendelkezik egy közös cél: engedélyezése a felhasználók segíti a hatékony munkát bárhonnan, miközben a helyszíni vagy távolról mobilkészülékükön hatékonyság növelése érdekében. Ez lehet egy közös cél, miközben vállalatok számára, hogy ilyen követelménynek is vonatkoznak, amely a vállalat adatok biztonsága és a felhasználó adatvédelmi karbantartása kell szüntethető veszélyek összegére. Egyes vállalatok, esetleg eltérő követelményei különböző tervezési döntéseket vezet különböző megfelelőségi szabályokat változhatnak aszerint, hogy melyik iparágban a vállalat éppen dolgozik. 

Vannak azonban olyan néhány biztonsági szempontjait megismerkedett és érvényesített, függetlenül attól, az üzleti, amelyeket a következő szakaszban.

## <a name="data-protection-paths"></a>Adatok védelme elérési út

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Adatok védelme elérési út**

A fenti ábrán az identitás-összetevő lesz a legelső adatok megjelenítése előtt ellenőrizendő. Azonban ezeket az adatokat lehet más államokból alatt az illetőknek elért volt. Ez az ábra a számok jelölik egy elérési utat, amelyben adatokat is elérhetők bizonyos pontján időben. A számok ismertetése az alábbiakban olvasható:

1. Adatvédelem a eszköz szintjén.
2. Adatvédelem a hálózaton átvitt.
3. Adatvédelem a többi a helyszíni.
4. Adatvédelem a többi a felhőben.

Bár a műszaki szabályozza, hogy fog engedélyezése informatikai magát az adatok védelme e szakaszból mindegyikre nem közvetlenül ajánlja fel a hibrid identitás megoldás, hogy a hibrid identitás megoldást is alkalmas az helyszíni és felhőbeli is szükség identitás kezelése erőforrások azonosítása, mielőtt a felhasználó hozzáférést az adatokat. Ha a hibrid identitás-megoldás tervezése győződjön meg arról, hogy a következő kérdéseket aszerint, hogy a szervezet követelményei vannak kérdésekre:

## <a name="data-protection-at-rest"></a>A többi adatok védelme
Függetlenül attól, ahol az adatokat a többi (eszközt használ, felhőalapú vagy a helyszíni) fontos, hogy a szervezet igényeinek e megértéséhez értékelést végrehajtása. Ezen a területen arról, hogy megkérdezi a program a következő kérdéseket:

- A vállalat-nek a többi adatok védelme érdekében?
 - Ha igen, az tudja a jelenlegi helyszíni infrastruktúra integrálása a hibrid identitás megoldást?
 - Ha igen, az a hibrid identitás megoldást tudja integrálása a munkaterhelésekből a felhőben?
- Az a felhő Identitáskezelés tudja a felhasználó hitelesítő adatait, és egyéb a felhőben tárolt adatok védelme?

## <a name="data-protection-in-transit"></a>A hálózaton átvitt adatok védelme
Az eszköz és a adatközponthoz vagy az eszköz és a felhő között hálózaton átvitt adatok kell védeni. Jó helyen jár a hálózaton átvitt éppen nem feltétlenül jelenti olyan összetevőt a felhőalapú szolgáltatás; kívüli kommunikáció folyamat Ugrás belső is, mint például egy két virtuális hálózatok. Ezen a területen arról, hogy megkérdezi a program a következő kérdéseket:

- A vállalat-nek a hálózaton átvitt adatok védelme érdekében?
 - Ha igen, a hibrid identitás megoldás az lehet például az SSL/TLS biztonságos vezérlők integrálása?
- Nem a felhőben az Identitáskezelés meg, hogy a forgalmat a címtár-tárolóban (belül és adatközpontokkal között) jelentkezett?


## <a name="compliance"></a>Megfelelőség
Szabályok, a törvényi és a szabályozási megfelelőségről követelmények aszerint, hogy az üzleti a vállalat tartozó változhatnak. Magas szabályozott iparágban cégek kell oldania kapcsolatos kompatibilitási problémák-Identitáskezelés kérdésére választ adhatnak. Identitás- és az access vonatkozó szabályzatát, például a Sarbanes-Oxley (SOX), állapot biztosítási hordozhatóságát és felelősség ellenőrzését törvény (HIPAA), a Gramm-kilúgzásos-Bliley törvény (GLBA) és a fizetési kártya üzleti adatok biztonsági szabványos (PCI DSS) nagyon szigorú. A hibrid identitás megoldást, amely a vállalat átveszi rendelkeznie kell egy vagy több e szabályzat követelményeinek teljesítenek alapvető funkcióját. Ezen a területen arról, hogy megkérdezi a program a következő kérdéseket:

- A hibrid identitás megoldás az felel meg az a vállalati szabályozói követelmények?
- Nem a hibrid identitás megoldást meg rendelkezik beépített funkciók, amelyek lehetővé teszik a vállalaton belüli kompatibilis szabályozói követelmények? 
 
>[AZURE.NOTE]
Győződjön meg róla, hogy minden válasz jegyzeteket, és megértette az answer logikája. [Adatok védelme stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) a program elküldi a rendelkezésre álló beállítások és az egyes beállítások előnyei és hátrányai fölé.  A fenti kérdések kiválasztott legjobb lehetőség illik üzleti problémákat megválaszolt szerint szükséges.

## <a name="next-steps"></a>Következő lépések
 [Tartalomkezelés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)
