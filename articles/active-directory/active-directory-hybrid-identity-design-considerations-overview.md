<properties
    pageTitle="Azure Active Directory hibrid identitást tervezési szempontjait - áttekintés |} Microsoft Azure"
    description="Áttekintés és a tartalom térképe hibrid identitás tervezési szempontjait útmutató"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory hibrid identitás tervezési szempontok

Fogyasztói-alapú eszközök vannak proliferating a vállalati világ, és felhőalapú szoftver-mint-a-(szoftver) szolgáltatásalkalmazások egyszerűen elfogadja. Emiatt az felhasználói hozzáférés-alkalmazás fenntartása belső adatközpontokkal és a felhő platformon ellen.  A Microsoft identitás-megoldások a helyszíni és felhőalapú funkciókat hitelesítés és az összes erőforrás, függetlenül a hely engedély egyetlen felhasználói azonosítót létrehozása időtartamát. A hibrid identitás hívása. Különböző tervezés, és konfigurációs beállításai a hibrid személyes Microsoft-megoldások használja, és bizonyos esetekben lehet nehezen állapíthatók meg, mely együtt a legjobb a szervezet igényeinek. Hibrid identitást tervezési szempontjait az útmutatóban megtanulása hozzásegíti megtudhatja, hogy hogyan való hibrid identitás-megoldás tervezése, amely legjobban megfelelő a vállalkozás és technológia van szüksége a szervezet számára.  Ez az útmutató ki lépéseket, és nem segít, hogy a szervezet egyedi megfelel hibrid identitás-megoldás tervezése követhet feladatok részletes információkat. A lépések és a tevékenységek teljes az útmutató bemutatja a kapcsolódó technológiák és a szolgáltatás beállítások munkahelyek és intézmények használhatják funkcionális értekezlet és a szolgáltatást minőségi (például elérhetőségét, méretezhetőség, teljesítmény, kezelhetőség és biztonsági) szintű követelményeknek. Kifejezetten a hibrid identitás szempontok útmutató céljai, az alábbi kérdésekre választ: 

- Milyen kérdést kell kérdezze meg, és a válasz egy hibrid identitás-specifikus tervezés technológia vagy probléma tartomány számára, hogy a legjobb megfelel a meghajtóra?
- Milyen, tetszőleges sorrendben követő tevékenységek kell befejezni technológia vagy probléma tartományhoz tartozó hibrid identitást megoldás tervezése? 
- Milyen hibrid identitás technológia és konfigurációs lehetőségek állnak rendelkezésre segítségre van szükségem a követelményeknek? Mik a kompromisszumok között ezeket a beállításokat, hogy a vállalati verzió a legjobb megoldást is választom?


## <a name="who-is-this-guide-intended-for"></a>Ki az útmutató számára íródott?
 CIO, CITO, Chief identitást építészek, vállalati építészek és informatikai építészek felelős tervezése a közepes vagy nagy méretű szervezetek hibrid identitást megoldást.

## <a name="how-can-this-guide-help-you"></a>Hogyan Ez az útmutató segíthetnek? 
Ez az útmutató segítségével megtudhatja, hogyan tudja a felhőalapú identitáskezelő rendszer integrálása a jelenlegi helyszíni identitás megoldás hibrid identitás megoldás tervezése. Az alábbi ábra mutatja, egy példa egy hibrid megoldást, amely lehetővé teszi az informatikai rendszergazdák integrálása az aktuális Windows Server Active Directory-megoldások kezelése a Microsoft Azure Active Directory engedélyezése a felhasználók számára az egyszeri bejelentkezés (SSO) számára, olvassa el a felhőben, és a helyszíni a helyszíni található.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


A fenti ábrán kihasználhatja a felhőalapú szolgáltatások integrálása helyszíni funkciók, a végfelhasználói hitelesítési folyamat egy egyszeri megoldást a biztosítása érdekében és megkönnyítése érdekében hibrid identitás megoldás példája informatikai ezek az erőforrások kezelésére. Bár ez lehet olyan gyakori eset, minden szervezete hibrid identitás Tervező valószínűleg ugyanaz, mint a példában az 1 miatt eltérő követelményeik illusztrált. Ez az útmutató ki lépéseket, és a szervezet egyedi követelményeknek hibrid identitás-megoldás tervezése követő tevékenységek. Az alábbi lépésekkel és feladatok, egész az útmutató hogyan jeleníti meg a megfelelő technológiák és a szolgáltatás elérhető beállítások, hogy az értekezlet funkcionális és szolgáltatást minőségi szintű követelmények a szervezet számára.

**Feltételezések**: a Windows Server, az Active Directory tartományi szolgáltatások és Azure Active Directory néhány élmény van. A jelen dokumentum feltételezzük, hogyan ezek megoldások is az üzleti igényeknek önállóan vagy egy integrált megoldáskészítő keres.

## <a name="design-considerations-overview"></a>Tervezési szempontjait – áttekintés
A dokumentum egy lépéseket és biztosít, amelyek a legjobban megfelel hibrid identitás-megoldás tervezése követő tevékenységek. A lépések egy rendezett sorrendben jelennek meg. Ismerje meg, ha későbbi lépéseiben tervezési szempontok megkövetelheti módosíthatja az elvégzett a korábbi lépések azonban miatt nem ütközik-e tervezés lehetőségek döntéseket. Minden kísérlet figyelmezteti potenciális tervezés ütközések szeretne a dokumentumban. 

A tervezés, hogy a legjobb megfelel a csak azt követően a műveletek elvégzéséhez, ahányszor szükséges, amelyekkel beépíthetők a a dokumentumon belül a megfontolások léptetés érkezik meg. 

| Hibrid identitás fázis:                                             | Témák listája                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Identitás követelmények meghatározása                                   | [Az üzleti igények meghatározása](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Címtár-szinkronizálás követelmények meghatározására](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Többtényezős hitelesítés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [A hibrid identitás átálláshoz meghatározása](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Adatok biztonsági keresztül erős identitás megoldás fejlesztése megtervezése | [Határozza meg az adatok védelmére vonatkozó követelmények](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Tartalomkezelés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Az access vezérlő követelmények meghatározása](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Az esemény válasz követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Adatok védelme stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Hibrid identitás életciklus megtervezése                                | [Hibrid identitás adatkezelési feladatok meghatározása](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Szinkronizálás kezelése](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hibrid identitás kezelése átálláshoz meghatározása](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Ez az útmutató letöltése
A hibrid identitás tervezési szempontjai útmutató egy PDF-verzióját letöltheti a [Technet-gyűjteményben](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
