
<properties
    pageTitle="Azure Active Directory hibrid identitás tervezési szempontjait - határozza meg az esemény rResponse követelmények |} Microsoft Azure vonatkozó követelmények "
    description="A hibrid identitás megoldás, amely szerint valószínűleg kihasználják nyomon követése és jelentéskészítési funkciók meghatározása informatikai műveletekhez azonosításához és egy potenciális kockázatok csökkentésében."
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>A hibrid identitás megoldás az esemény válasz követelmények meghatározása

Nagy- és középvállalatok szervezetek valószínűleg fog rendelkezni [az esemény válasz biztonsági](https://technet.microsoft.com/library/cc700825.aspx) érdekében helyen informatikai műveletekhez lehetőséget az esemény szintű. A identitáskezelő rendszer oka az, a válasz az esemény folyamat fontos összetevője használható egy adott műveletet a célértékhez viszonyított végre, akik azonosítása érdekében. A hibrid identitás megoldást ahhoz tartalmat hozzáadó felhasználók nyomon követése és jelentéskészítési funkciók, amelyek szerint valószínűleg kihasználják megadására informatikai azonosításához és a potenciális veszélyt csökkentésében műveletekhez. Egy tipikus az esemény válasz csomagban a terv részeként választania kell az alábbi lépéseket:

1.  Kezdeti értékelése.
2.  Az esemény kommunikáció.
3.  Kárt ellenőrzése és a kockázatok csökkentése.
4.  Mi azonosítása biztonságos és erőssége volt.
5.  Bizonyítékokra megőrzésével.
6.  Értesítés a megfelelő feleknek.
7.  Rendszer-helyreállítás.
8.  Dokumentáció.
9.  Sérülések és a költség felmérése.
10. Folyamatábra, és a csomag verziójának.

Milyen informatikai azonosítása során biztonságos és súlyosságát-fázis, szükség az, hogy melyik kerülhetett, amelyek van nyitottak, és meghatározzák, hogy ezeket a fájlokat a magánjellegű lesz. A hibrid identitás rendszer látnia kell ezeknek a követelményeknek, amely segít a felhasználót, hogy a változtatásokat azonosító teljesítése érdekében. 

## <a name="monitoring-and-reporting"></a>Ellenőrzés és jelentéskészítés
Az identitás rendszer sokszor segíthetnek kezdeti értékelési fázis főleg, ha a rendszer rendelkezik beépített naplózási és jelentéskészítő képességeit. A kezdeti felmérés során informatikai rendszergazda a gyanús tevékenységének azonosítani tudja kell lennie, vagy a rendszer automatikusan alapú egy előre konfigurált tevékenység eseményindító látnia kell. Sok tevékenység jelezheti a lehetséges támadások, azonban más esetben a rosszul konfigurált rendszer vezethet téves számos egy behatolási észlelési rendszerben. 

A identitáskezelő rendszer segítséget kell informatikai rendszergazdák azonosításához és gyanús azoknál a tevékenységeknél jelentést. Általában ezeknek a követelményeknek technikai is teljesülniük összes rendszerek figyelése és a problémát egy, a potenciális veszélyek kiemelheti jelentéskészítési lehetőséget. Használja az alábbi kérdésekre úgy a hibrid identitás megoldás során figyelembe az esemény válasz követelmények figyelembevételével tervezze meg:

- A vállalat rendelkezik biztonsági az esemény választ helyen?
 - Ha igen, az aktuális identitáskezelő rendszer használja a program a folyamat részeként?
- Nem a vállalat kell azonosítania a gyanús bejelentkezési kísérletet a felhasználó különböző eszközök között?
- A vállalat-nek potenciális biztonságos felhasználói hitelesítő adatok feltárása?
- A vállalat-nek naplózása a felhasználó hozzáférést és művelet?
- A vállalat-nek tudnivalók, amikor a felhasználó saját jelszó alaphelyzetbe állítása?

## <a name="policy-enforcement"></a>Házirend-kényszerítés

Során sérülések vezérlőelem és a kockázatok csökkentésére-fázis fontos, hogy gyorsan csökkentheti a tényleges és lehetséges hatásának egy. A művelet, amelyet a hosszabb, ezen a ponton fel, hogy egy kisebb és a fő egy közötti különbség. A pontos választ a szervezet és az, hogy Ön arcra támadások jellegét függ. A kezdeti felmérés kötött, hogy a fiók jutott, ha szüksége lesz a hivatkozási házirendjének ehhez a fiókhoz. Ez csak egy példa, ahol a identitáskezelő rendszer kapcsolatos károkozásra. Használja az alábbi kérdésekre a hibrid identitás megoldás tervezésekor figyelembe véve hogyan házirendek szabályon a folyamatban lévő esemény reagálni:

- A vállalat muszáj házirendek helyen felhasználók letiltása az access a hálózat szükség esetén?
 - Ha igen, nem az aktuális megoldás integrálása a hibrid identitáskezelő rendszer, hogy elfogadják fogja?
- A vállalat-nek meg lehessen őrizni a feltételes hozzáférés karantén szereplő felhasználókat? 
 
>[AZURE.NOTE]
Győződjön meg róla, hogy minden válasz jegyzeteket, és megértette az answer logikája. [Meghatározás adatok védelme stratégia](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) a program elküldi a rendelkezésre álló beállítások és az egyes beállítások előnyei és hátrányai fölé.  A fenti kérdések kiválasztott legjobb lehetőség illik üzleti problémákat megválaszolt szerint szükséges.

## <a name="next-steps"></a>Következő lépések
[Adatok védelme stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Lásd még:
[Tervezési szempontjait – áttekintés](active-directory-hybrid-identity-design-considerations-overview.md)
