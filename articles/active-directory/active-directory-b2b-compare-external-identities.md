<properties
   pageTitle="Azure Active Directory használatával külső identitások kezelésére szolgáló funkciók összehasonlítása |} Microsoft Azure"
   description="Miben más az Azure Active Directory B2B együttműködési B2C és több bérlői alkalmazás külső identitások hitelesítési és engedélyezési támogatása"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Azure Active Directory használatával külső identitások kezelésére szolgáló funkciók összehasonlítása

Azure Active Directory (Azure Active Directory) mellett a mobil munkaerő hozzáférés szoftver alkalmazások kezelése, a szervezet erőforrások megosztása üzleti partnereket és előadása alkalmazások vállalkozások és a fogyasztói segítséget.

## <a name="developing-applications-for-businesses"></a>Üzleti alkalmazások fejlesztéséhez

Nyújt a szolgáltatásra vagy az alkalmazást, például egy bérszámfejtő szolgáltatás vállalkozások számára? Azure Active Directory adja, amely lehetővé teszi, hogy az alkalmazások létrehozását zökkenőmentes integráció a szervezeteknek szól, amelyek már van konfigurálva a nyekhttp://technet.microsoft.com/en-us/library/jj823129.aspx Office 365- vagy egyéb vállalati szolgáltatás részét képező Azure Active Directory milliónyi identitás platformot.

**Példa:** A gyógyszerészeti distributor orvosi éppen a Kellékek és a egészségügyi üzleti adatok rendszerek biztosít. Analytics alkalmazás orvosi eljárások és a kívánt ügyfelek kezelése a saját identitások mezőjének esetén szükséges. Ez a cég kiválasztott Azure AD az identitás platform, a gyakorlás alkalmazáshoz, nyújtó Azure AD-identitások kukac ügyfeleik felfelé szükség esetén. További információ a [Azure Active Directory fejlesztői](active-directory-developers-guide.md)útmutatójában.

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Üzleti partneri hozzáféréssel a vállalati erőforrások engedélyezése

Van az üzleti partnereivel vagy más telepíteniük kell a vállalati erőforrások, például egy SharePoint-webhelyen vagy a ERP rendszer elérése vállalaton kívüli? Azure Active Directory rendszergazdák lehetővé teszi a külső felhasználók (Előfordulhat, hogy, vagy nem létezik az Azure Active Directory) egyszeri bejelentkezési vállalati alkalmazások való hozzáférés megadását. Biztonsági Ez javítja a, a felhasználók elveszíti hozzáférését, ha elhagyják a partner szervezeti, miközben Ön a hozzáférési a szervezeten belül. Ez is egyszerűbbé teszi a felügyeletet, nem kell egy külső partner könyvtár vagy egy partner összevonási kapcsolatok kezelése.

**Példa:** Egy képkezelő vállalathoz kereskedők nyújtó szolgáltatások imaging fényképet, és nyomtatási helyek legnagyobb kiskereskedelmi hálózatához működik. Azok a kiválasztott ahhoz, hogy a kiskereskedelmi üzleti partnereivel, töltse le a legújabb partner marketinganyagok és a vállalat címkéjét extranetes éppen a Kellékek feldolgozása fénykép átrendezése a saját hitelesítő adatok használatával a felhasználók ezer Azure AD. További tudnivalókért lásd: az [Azure Active Directory B2B együttműködési](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Fogyasztói alkalmazások fejlesztéséhez

Szükség van biztonságosan és költséghatékonyan közzététele online alkalmazások, például a kiskereskedelmi üzlet elöl, több millió fogyasztói? Azure Active Directory közösségi felhasználónevével és jelszavával jelentkezzen be és engedélyezése platformot biztosít, védjegyzett önkiszolgáló regisztráció és önkiszolgáló jelszó alaphelyzetbe állítása, az az alkalmazás vonzóbbak lehetnek. Ez megnöveli kényelmesebbé a fogyasztói csökkentése a fejlesztők a betöltés közben.

**Példa:** A \#1 sportruházat franchise a világ közvetlenül a saját 450 millió ventillátortól folytasson szükséges. Ehhez Azure Active Directory használata a felhasználói hitelesítés és a profil tárolásához mobilalkalmazásban beépített őket. Ventillátortól egyszerűsített regisztrációs beszerzése és a bejelentkezési – például a Facebook közösségi fiókok használata, vagy használhatják hagyományos felhasználónév és jelszó-tal zökkenőmentesen át az iOS, Android és Windows telefonok. A létrehozott támaszkodva Azure AD-platform jelentősen csökkentett egyéni kódot, miközben a franchise biztosítva egyéni arculati és biztonsági adatok szabályok megsértésével és méretezhetőség kétségei enyhítése. További tudnivalókért olvassa el az [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)című témakört.

## <a name="comparison-of-azure-ad-capabilities"></a>Azure AD-funkciók összehasonlítása

A már a jelen cikkben tárgyalt forgatókönyveket mindegyike címzettjei képességek szerint Azure Active Directory belül. Az alábbi táblázat a jelezhető, hogy milyen lehetőségei legrelevánsabbnak segítségére lehetnek:

| **Fontolja meg a termék...**       | [Azure Active Directory több bérlői szoftver alkalmazás](active-directory-developers-guide.md)    | [Azure Active Directory B2B együttműködési](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Ha meg kell adnia...** | a szolgáltatás vállalkozások számára | saját alkalmazások partneri hozzáféréssel  | a szolgáltatás a felhasználóknak |
| **És hasonló vagyok...**  | Pharma distributor      | Vállalati Imaging            | A Sports franchise       |
| **Az alkalmazás telepítése...**  | Gyakorlat-kezelés     | Extranetes címkéjét          | Foci ventillátortól            |
| **Kiválasztásával...**        | Orvos irodák        | Jóváhagyott partnerei | Bárki a levelezés beállítása      |
| **Könnyen kezelhető Ha...**      | Ügyfél-felügyeleti hozzájárul | A felügyeleti felkérés           | A feliratkozik      |
