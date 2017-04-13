<properties
   pageTitle="Részletes ismertetését megtalálja az Azure Active Directory B2B együttműködési preview használatakor |} Microsoft Azure"
   description="Azure Active Directory B2B együttműködési támogatja a vállalatot érintő kapcsolatok, mivel az üzleti partnereket a vállalati alkalmazások szelektív eléréséhez"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Azure Active Directory B2B együttműködési előzetes verzió: részletes ismertetését megtalálja

Az útmutató ismerteti, hogyan használhatja az Azure Active Directory B2B együttműködési. A Contoso informatikai rendszergazdaként szeretnénk alkalmazások megosztása a három partner cégek által készített alkalmazottakkal. A partner vállalatok egyike sem kell Azure AD.

- Az egyszerű Partner szervezeti Anna
- Péter, közepes Partner szervezeti, az alkalmazások halmazának hozzáférésre van szüksége.
- Fülöp összetett Partner szervezeti, az alkalmazások és a csoportok a Contoso tagsága hozzáférésre van szüksége.

Meghívók partner felhasználók elküldi, miután azt beállíthatja őket az access-alkalmazás és a csoportoknak az Azure-portálon keresztül tagság megadását Azure AD. Első lépésként Anna hozzáadásával.

## <a name="adding-alice-to-the-contoso-directory"></a>A Contoso könyvtár Anna hozzáadása
1. A fejlécek létrehozása .csv fájl feltöltése csak Anna **levelezési** **DisplayName**és **InviteContactUsUrl**látható módon. **DisplayName** , a meghívóban megjelenő neve is a Contoso Azure Active directory megjelenő neve. **InviteContactUsUrl** Anna kapcsolatba lépni a Contoso-módja. A következő példában a InviteContactUsUrl Itt adhatja meg a Contoso LinkedIn profiljában. Fontos pontosan meghatározott a [CSV formátum fájlhivatkozás](active-directory-b2b-references-csv-file-format.md)a .csv fájl első sorában a címkék helyesírását.  
![Példa CSV-fájl Anna](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Az Azure-portálon: új felhasználó át a Contoso directory (az Active Directory > Contoso > felhasználók > felhasználó hozzáadása). Az "Írja be a felhasználó" legördülő listájára jelölje be a "Felhasználók partner vállalatok". A .csv fájl feltöltése. Győződjön meg arról, hogy a a .csv fájl feltöltése előtt befejeződik.  
![Anna a CSV-fájl feltöltése](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. A Contoso Azure Active directory külső felhasználók Anna most jelöli.  
![Anna szerepel az Azure Active Directory](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Anna az alábbi e-mailt kap.  
![Anna meghívó e-mailt](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Anna a hivatkozásra kattint, és azt kéri, fogadja el a meghívót, és jelentkezzen be a saját munka hitelesítő adataival. Anna nem szerepel az Azure Active directory, ha az Anna kéri a regisztráció.  
![Regisztráció után Anna szóló meghívó](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Anna a rendszerünk átirányítja az alkalmazás hozzáférést panelen üres mindaddig, amíg kikkel van hozzáférése alkalmazások.  
![Anna Access Panel](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Ez az eljárás lehetővé teszi, hogy a B2B együttműködési legegyszerűbb formájában. A Contoso Azure AD-címtárban felhasználóként Anna is hozzáférési jogosultsággal alkalmazások és a csoportok, az Azure portálon keresztül. Most már Péter, akinek van szüksége az alkalmazás Moodle és a Salesforce-hozzáférés hozzáadása.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Péter hozzáadása a Contoso könyvtár és alkalmazások hozzáférés engedélyezése
1. A Windows PowerShell az Azure Active Directory modul telepítve van az alkalmazás lehetővé tevő dokumentumazonosítók Moodle és a Salesforce keresése a. Az azonosítók a parancsmaggal tudja visszaszerezni: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` ekkor megjelenik a Contoso és azok AppPrincialIds az összes rendelkezésre álló alkalmazás listáját.  
![Péter azonosítók lekérése](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Péter levelezési és DisplayName **InviteAppID**, **InviteAppResources**és InviteContactUsUrl tartalmazó .csv fájl létrehozása. **InviteAppResources** AppPrincipalIds a Moodle és található PowerShell, szóközzel elválasztva a Salesforce adataival. **InviteAppId** márkajegyeket helyezhet el az e-mailt, és jelentkezzen be egy Moodle emblémával lapok a Moodle azonos AppPrincipalId való feltöltéséhez.  
![Példa CSV-fájl Péter](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Töltse fel a CSV-fájlból az Azure-portálon keresztül, ahogyan azt az Anna történt. Péter ettől kezdve a Contoso Azure Active directory a külső felhasználók.

4. Péter az alábbi e-mailt kap.  
![A meghívó levelezés Péter](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Péter a hivatkozásra kattint, és elfogadhatja a meghívást kéri. Miután ő be van jelentkezve, ő átirányításához az Access panelen, és Moodle és a Salesforce már használhatja.  
![Péter Access Panel](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Hozzáadunk Fülöp tovább, akinek van szüksége az access-alkalmazások és a csoportoknak a Contoso címtárban tagság egyaránt.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Fülöp hozzáadása a Contoso könyvtár, megadása az access-alkalmazás és csoporttagság megadása

1. A Windows PowerShell használata az Azure Active Directory modul telepítve van a Alkalmazásazonosítók és a csoport azonosítók Contoso belül.
 - Parancsmaggal AppPrincipalId beolvasásához `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, ugyanaz, mint a Péter
 - Csoportok parancsmaggal objektumazonosító lekérése `Get-MsolGroup | fl DisplayName, ObjectId`. Ekkor megjelenik a Contoso és azok ObjectIds az összes csoport listája. Csoport azonosítók tudja visszaszerezni az objektum ID a Tulajdonságok lapon annak a csoportnak az Azure-portálon is.  
![Fülöp azonosítók és csoportok lekérése](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Hozzon létre a .csv fájl feltöltése Fülöp meg e-mailek, DisplayName, InviteAppID, InviteAppResources, **InviteGroupResources**és InviteContactUsUrl. **InviteGroupResources** a ObjectIds MyGroup1 és összesen, szóközzel elválasztva a csoportok szerint van kitöltve.  
![Példa CSV-fájl Fülöp](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Töltse fel a CSV-fájlból az Azure portálon keresztül.

4. Fülöp a Contoso címtár-felhasználó, és is a tagja a csoportok MyGroup1 és összesen, az Azure-portálon látható módon.  
![Az Azure Active Directory Fülöp szerepel a csoportban](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Fülöp elfogadhatja a meghívást mutató hivatkozást tartalmazó e-mailben kap. Miután kikkel bejelentkezik, Noémi a rendszerünk átirányítja az alkalmazás hozzáférést panelen hozzáférjen Moodle és a Salesforce.  

Ez az összes van, és a felhasználók felvétele a partner készült Azure Active Directory B2B együttműködés. Ez a forgatókönyv mutatott Anna, Péter és Fülöp felhasználók hozzáadása a Contoso könyvtár három külön .csv fájl használatával. Ez a folyamat végezhető könnyebben el a külön .csv fájlokban kondenzációs egyetlen fájlba.  
![Anna, Péter és Fülöp minta CSV-fájlt](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Kapcsolódó cikkek
Tallózással keresse meg a többi cikkek az Azure Active Directory B2B együttműködési:

- [Mi az Azure Active Directory B2B együttműködési?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Működése](active-directory-b2b-how-it-works.md)
- [CSV-fájl formázása hivatkozás](active-directory-b2b-references-csv-file-format.md)
- [Külső felhasználók jogkivonat formázása](active-directory-b2b-references-external-user-token-format.md)
- [Külső felhasználók objektum attribútum megváltozik](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuális előzetes korlátai](active-directory-b2b-current-preview-limitations.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
