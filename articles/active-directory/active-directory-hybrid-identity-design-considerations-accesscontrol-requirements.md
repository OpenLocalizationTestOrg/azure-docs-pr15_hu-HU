
<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - hozzáférési vezérlő követelmények meghatározása |} Microsoft Azure"
    description="Identitás és a hozzáférési követelmények forrásokat is tartalmaz a felhasználók hibrid környezetben az azonosító foglalkozik."
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

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitás megoldás access vezérlő követelmények meghatározása
Tervezésekor egy szervezet van a hibrid megoldást, tekintse át az access követelményei az erőforrásokat, azok tervezi, elérhetővé szeretné tenni a felhasználóknak is használhatják ezt a lehetőséget. Az adatok access közötti összes négy oszlopok személyazonosságáról, amelyek:

- Felügyelete
- Hitelesítés
- Engedély
- A naplózás

Az alábbi szakaszok kiterjed hitelesítés és további részleteket az engedélyezés, felügyelete és a naplózás a hibrid identitás életciklusáról részét képezik. Olvassa el a [meghatározása hibrid identitás adatkezelési feladatok](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) e ezekre a lehetőségekre további információt.

>[AZURE.NOTE]
Olvassa el [A négy oszlopok az identitás - hibrid kor informatikai az Identitáskezelés](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) az adott oszlopok mindegyik kapcsolatban további tudnivalókat.

## <a name="authentication-and-authorization"></a>Hitelesítés és az engedélyezés
Hitelesítés és engedélyezési különböző forgatókönyv, forgatókönyvekben lesz a hibrid identitás megoldás, hogy elfogadja a vállalat fog által teljesítendő adott követelményeket. Üzleti az üzleti (B2B) kapcsolati érintő esetek adhat hozzá egy további beavatkozás igazolására szolgáló eljárás informatikai rendszergazdáknak, mivel lesz szükségük annak érdekében, hogy a hitelesítési és hitelesítési módszert a szervezet által használt azok üzleti partnerekkel is kommunikálhatnak. A tervezése során hitelesítési és a hitelesítési követelményeket győződjön meg arról, hogy a következő kérdések és válaszok:

- Lesz a szervezet hitelesítheti, és engedélyezheti csak azok a felhasználók saját identitáskezelő rendszer található?
 - Vannak olyan esetek B2B tervei?
 - Ha igen, már tudja (SAML, OAuth, Kerberos, tokenek vagy tanúsítványok) protokollok használandó mindkét vállalkozások csatlakozni?
- A hibrid identitás megoldást, fogadja el a támogatási fogja azokat a protokollok elve

Fontolja meg egy másik fontos pontra, ahol a felhasználók és a partnerek által használt hitelesítési tárházba lesz található, és a használandó felügyeleti modell. Vegye figyelembe az alábbi két alapvető:
- A központi: Ebben a modellben a felhasználó hitelesítő adatait, házirendek és felügyeleti lehet központi a helyszíni, vagy a felhőben.
- Hibrid: Ebben a modellben a felhasználó hitelesítő adatait, házirendek és felügyeleti központi a helyszíni és lesz a replikált a felhőben.

A szervezet átveszi modellt üzleti igényeik szerint változhatnak, szeretné a identitáskezelő rendszer tároló és a felügyeleti üzemmódban használja az alábbi kérdésekre választ:

- Szervezetének jelenleg van egy Identitáskezelés helyszíni?
 - Ha igen, végezze el őket megtervezése tarthatja?
 - Vannak-e, hogy a szervezet hajtsa végre az adott megkövetel a identitáskezelő rendszer tároló kell bármely rendelkezések vagy a megfelelőségi követelmények?
- Szervezete használja az egyszeri bejelentkezés a helyszíni található alkalmazások vagy a felhőben?
 - Ha igen, a hibrid identitás mintájának elfogadása hatással van a folyamat?

## <a name="access-control"></a>Hozzáférés-vezérlés
Hitelesítési és engedélyezési elemei core felhasználó-ellenőrzési keresztül vállalati adatokhoz való hozzáférés engedélyezése, miközben fontos is szabályozhatja, hogy ezek a felhasználók lesz, és a rendszergazdák hozzáférési szintjének lesz, az erőforrásokat, hogy tartománya fölé hozzáférési szintet. A hibrid identitás megoldás ahhoz tartalmat hozzáadó felhasználók erőforrások, a meghatalmazás és az alap hozzáférés-vezérlés szerepkör részletes hozzáférést biztosít. Győződjön meg arról, hogy a következő kérdést hozzáférés-vezérlés kapcsolatban vannak kérdésekre:

- A vállalat rendelkezik az identitás rendszer kezeléséhez, emelt jogosultsági egynél több felhasználó?
 - Ha igen, minden felhasználó-nek azonos szintű hozzáférést?
- A vállalat kimutatásadatokat meghatalmazotti hozzáférést a felhasználóknak, hogy az adott erőforrások?
 - Igen, milyen gyakran ez történik, ha?
- A vállalat kimutatásadatokat integrálása helyszíni és felhőbeli között vezérlő elérést erőforrások?
- A vállalat kimutatásadatokat erőforrásokhoz bizonyos feltételek szerint korlátozza?
- Szeretné, hogy bármely alkalmazásban, amely az egyes erőforrások egyéni vezérlőelem hozzáférésre van szüksége a vállalat?
 - Ha igen, ezeket az alkalmazásokat helyét (a helyszíni, vagy a felhőben)?
 - Ha igen, hol vannak a cél erőforrásait (helyszíni, vagy a felhőben)?

>[AZURE.NOTE]
Győződjön meg róla, hogy minden válasz jegyzeteket, és megértette az answer logikája. [Adatok védelme stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) a program elküldi a rendelkezésre álló beállítások és az egyes beállítások előnyei és hátrányai fölé.  A fenti kérdések megválaszolása kijelöli legjobb lehetőség igényeinek leginkább megfelelő.

## <a name="next-steps"></a>Következő lépések

[Az esemény válasz követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)
