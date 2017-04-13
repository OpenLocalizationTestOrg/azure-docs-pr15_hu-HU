<properties
   pageTitle="Azure Active Directory B2B együttműködési |} Microsoft Azure"
   description="Azure Active Directory B2B-alapú együttműködés lehetővé teszi, hogy az üzleti partnereket, hogy a vállalati alkalmazások eléréséhez, az összes a felhasználók egy egyetlen Azure Active Directory jelöli fiók"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B együttműködési

Azure Active Directory (Azure Active Directory) B2B-alapú együttműködés lehetővé teszi a partner felügyelt identitások a vállalati alkalmazások hozzáférést. Több-vállalatot kapcsolatok létrehozása meghívása és engedélyezése a felhasználók partner vállalatoktól erőforrásait eléréséhez. Összetettsége csökken, mivel minden vállalat egyszer federates Azure Active Directory és minden felhasználó egyetlen képviseli Azure AD-fiókot. Ha üzleti partnereivel az Azure Active Directory kezelése a fiókok, mivel az access visszavonták, amikor megszakítja megtekintsék a felhasználók a partnert, és belső könyvtárak tagsága várt hozzáférésének miatt nem tudja biztonsági nő. Üzleti partnereivel, aki még nincs Azure Active Directory, a B2B együttműködési tartalmaz egy Azure Active Directory-fiókok számára az üzleti partnereivel egyesíti előfizetési funkcióit.

-   Üzleti partnereivel saját bejelentkezési hitelesítő adatait, amelyek üzletvitelre az egy külső partner directory kezelése, majd a hozzáférés eltávolítása, amikor a felhasználók távozó a partner kell használni.

-   Hozzáférés kezelése a business partnere fiók életciklus függetlenül az alkalmazásait. Ez azt jelenti, például hogy anélkül, hogy kérje az informatikai részleg a vállalati partner valami hozzáférést vonhatja.

## <a name="capabilities"></a>Funkciók

B2B együttműködési egyszerűsíti kezelését, és vállalati erőforrások, beleértve a szoftver alkalmazások, például az Office 365-ben, Salesforce, Azure-szolgáltatások és mindegyik mobile, a felhő és a helyszíni követelések szem előtt alkalmazás partneri hozzáféréssel biztonsága javítja. B2B együttműködés lehetővé teszi a partnerek kezelése a saját fiók és vállalkozások biztonsági házirendeket alkalmazhatja partneri hozzáféréssel.

Együttműködési Gyerekjáték állítható be az Azure Active Directory B2B egyszerűsített előfizetési a különféle méretű partnerek számára, akkor is, ha nincs is saját Azure Active Directory-e-mailek ellenőrzött folyamat keresztül. Érdemes emellett könnyen kezelhető nincs külső könyvtárak vagy egy partner összevonási konfigurációk.

## <a name="b2b-collaboration-process"></a>B2B együttműködési folyamatábra

1. Azure Active Directory B2B-alapú együttműködés lehetővé teszi, hogy egy vállalati rendszergazdai meghívása, és engedélyezheti a külső felhasználók egy csoportja által 2000-nél több vonalak vesszővel elválasztott értékek (CSV) fájlok feltöltése a B2B együttműködési webhely.

  ![CSV-fájl feltöltése párbeszédpanel](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. A portál elküldi ezeket a külső felhasználók e-mailben meghívót.

3. A meghívott felhasználók fog meglévő munkahelyi fiók (az Azure Active Directory kezelése) Microsoft jelentkezzen be, vagy új munka-fiók beszerzése az Azure Active Directory.

4. Miután bejelentkezett, a felhasználó lesz irányítva a velük megosztott volt az alkalmazást.

Felkérés fogyasztói e-mail címét (például Gmail- vagy [*comcast.net*](http://comcast.net/)) jelenleg nem támogatott.

További B2B együttműködési működése a kövesse [ezt a videót](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Következő lépések
Tallózással keresse meg a többi cikkek az Azure Active Directory B2B együttműködési.

- [Mi az Azure Active Directory B2B együttműködési?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Működése](active-directory-b2b-how-it-works.md)
- [Részletes útmutató](active-directory-b2b-detailed-walkthrough.md)
- [CSV-fájl formázása hivatkozás](active-directory-b2b-references-csv-file-format.md)
- [Külső felhasználók jogkivonat formázása](active-directory-b2b-references-external-user-token-format.md)
- [Külső felhasználók objektum attribútum megváltozik](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuális előzetes korlátai](active-directory-b2b-current-preview-limitations.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
