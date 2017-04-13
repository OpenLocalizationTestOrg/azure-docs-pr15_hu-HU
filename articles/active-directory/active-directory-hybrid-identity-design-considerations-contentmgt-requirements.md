<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - Tartalomkezelés követelmények meghatározása |} Microsoft Azure"
    description="Annak megállapítása, a vállalati Tartalomkezelés követelményeinek betekintést nyújt. Általában amikor egy felhasználó rendelkezik-e saját eszköz ő esetleg is több hitelesítő adatokat, hogy a program a ő használó alkalmazásokban szerint váltakozó. Fontos, hogy elkülönüljön a tartalom jött létre, és a vállalati hitelesítő adatok alapján létre lehetőségekből személyes hitelesítő adataival. A személyes megoldás tud majd kommunikálni a felhőben kell lennie ahhoz, hogy tal zökkenőmentesen közben a végfelhasználó szolgáltatások saját biztosítani, és adatok szivárgás elleni védelem növelése."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitás megoldás Tartalomkezelés követelmények meghatározása

A vállalati Tartalomkezelés követelményei ismertetése a döntési melyik hibrid identitás megoldást használni a közvetlen hatással lehet. A különféle eszközökön tömegpusztító és ahhoz, hogy a saját eszközök ([BYOD](http://aka.ms/byodcg)), a felhasználók lehetőséget a vállalat kell a saját adatok védelme biztonsági, de azt is kell tartani felhasználó adatvédelmi sértetlen. Általában amikor egy felhasználó rendelkezik-e saját eszköz ő esetleg is több hitelesítő adatokat, hogy a program a ő használó alkalmazásokban szerint váltakozó. Fontos, hogy elkülönüljön a tartalom jött létre, és a vállalati hitelesítő adatok alapján létre lehetőségekből személyes hitelesítő adataival. A személyes megoldás tud majd kommunikálni a felhőben kell lennie ahhoz, hogy tal zökkenőmentesen közben a végfelhasználó szolgáltatások saját biztosítani, és adatok szivárgás elleni védelem növelése. 

A megoldást az alábbi ábrán látható módon a tartalmak kezeléséhez biztosítása érdekében különböző technikai vezérlők kell kapcsolatos fog károkozásra:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**A identitáskezelő rendszer vizsgál fog biztonsági funkciók**

Az általános Tartalomkezelés követelmények fog kihasználhatja a identitáskezelő rendszer a következő helyeken:

- Adatvédelmi: azonosítása a felhasználót, hogy egy erőforrás tulajdonában van, és a megfelelő vezérlőelemeket adatintegritás fenntartása alkalmazásával.
- Adatok besorolás: azonosítsa a felhasználót vagy csoportot és besorolását szerint objektummá hozzáférési szintet. 
- Adatvédelem szivárgás: felelős szivárgás elkerülése érdekében adatok védelme biztonsági funkciók kell az identitás használhat a felhasználói azonosító érvényesítéséhez. A fontos is Képletvizsgálat pontosan célját.

>[AZURE.NOTE]
Olvassa el [a felhőben munkahelyi és intézményi adatok besorolás](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) gyakorlati tanácsokat olvashat, és adatok besorolás útmutató.

Ha a hibrid identitás-megoldás tervezése győződjön meg arról, hogy a következő kérdéseket aszerint, hogy a szervezet követelményei vannak kérdésekre:

- A vállalat rendelkezik biztonsági funkciók helyen meg lehessen őrizni a adatok adatvédelmi?
 - Ha igen, ezeket biztonsági funkciók fogja tudni fogja fogadja el a hibrid identitás-megoldások integrálása?
- A vállalat használja az adatok besorolás?
 - Ha igen, az aktuális a megoldást integrálása a hibrid identitás megoldás, hogy elfogadják fogja tudni?
- A vállalat jelenleg rendelkezik minden megoldást adatok szivárgás? 
 - Ha igen, az aktuális a megoldást integrálása a hibrid identitás megoldás, hogy elfogadják fogja tudni?
- A vállalat-nek erőforrásokhoz naplózandó?
 - Ha igen, milyen típusú erőforrások?
 - Ha igen, milyen információkat mértéke szükség?
 - Ha igen, ahol a napló kell lennie? A helyszíni és felhőbeli?
- A vállalat-nek titkosítása minden olyan bizalmas adatokat (SSNs, hitelkártya számok stb.) tartalmazó e-mailek?
- A vállalat-nek teljes dokumentumok/tartalom külső üzleti partnereivel megosztott titkosítása?
- A vállalat-nek meg lehessen őrizni a vállalati e-mailek bizonyos típusú házirendek (ne minden válasz, a Nincs átirányítás)?
 
>[AZURE.NOTE]
Győződjön meg róla, hogy minden válasz jegyzeteket, és megértette az answer logikája. [Adatok védelme stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) a program elküldi a rendelkezésre álló beállítások és az egyes beállítások előnyei és hátrányai fölé.  A fenti kérdések kiválasztott legjobb lehetőség illik üzleti problémákat megválaszolt szerint szükséges.


## <a name="next-steps"></a>Következő lépések
[Az access vezérlő követelmények meghatározása](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)
