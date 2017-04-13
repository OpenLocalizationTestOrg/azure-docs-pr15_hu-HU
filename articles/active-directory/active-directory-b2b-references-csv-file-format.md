<properties
   pageTitle="CSV-fájl formátuma Azure Active Directory B2B együttműködési előzetes verzió |} Microsoft Azure"
   description="Azure Active Directory B2B támogatja a vállalatot érintő kapcsolatok, mivel az üzleti partnereket, hogy a szelektív hozzáférés a vállalati alkalmazások"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure Active Directory B2B együttműködési előzetes: csv-fájl formátuma

Azure Active Directory B2B együttműködési előzetes verziója szükséges felhasználói partneradatok fel kell tölteni az Azure Active Directory-portálon keresztül tartalmazó CSV-fájlba. A CSV-fájl tartalmaznia kell a szükséges, az alábbi címkéket, és a szükséges mezőket nem kötelező. A minta CSV-fájlban (lásd lent) módosíthatja a helyesírás-ellenőrzés az első sor a feliratok módosítása nélkül.

>[AZURE.NOTE] Címkék (például az e-mailek, a DisplayName, és így tovább) első sorában lévő szükség a CSV-fájl sikeresen elemzendő. A helyesírás-ellenőrzés egyeznie kell az alább felsorolt mezők. Ezek a címkék azonosítása: a tartalom alatt. Választható mezők nem szükséges a címkék eltávolítható a CSV-fájlt. A teljes oszlop üresen hagyható.

## <a name="required-fields-br"></a>Kötelező mezőket: <br/>
**E-mail:** A meghívott felhasználó e-mail címe. <br/>
**DisplayName:** A meghívott felhasználó (általában az első és utolsó neve) nevet.<br/>


## <a name="optional-fields-br"></a>Választható mezők: <br/>

**InvitationText:** Testre szabhatja a felkérés e-mailek szövegének alkalmazás védjegyzés után, és a visszaváltás hivatkozás előtt.<br/>
**InvitedToApplications:** AppIDs vállalati alkalmazások hozzá szeretné rendelni a felhasználók számára. Hívja fel a PowerShellben beolvasható AppIDs`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** Felhasználó hozzáadása-csoportok ObjectIDs. Hívja fel a PowerShellben beolvasható ObjectIDs`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** URL-címe irányítsa át a meghívott felhasználók felkérés elfogadása után. Meg kell egy vállalatánál URL-CÍMÉT (például [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Ha ez nem kötelező mezőben nem szerepel, a rendszer az alkalmazás Access panelen, ha azokat is nyissa meg a választott vállalati alkalmazások forgalmat a meghívott felhasználók. Az űrlap van az alkalmazás hozzáférést Vezérlőpult URL-címe `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: E-mail cím mailezett meghívó másolásához. A CcEmailAddress mező használata esetén a felkérés e-mailek ellenőrzött felhasználó vagy a bérlő létrehozása nem használhatók.<br/>
**Nyelv:** Felkérés e-mailek és a visszaváltás a yammert, az "en" (angol nyelven) az alapértelmezett érték, ha nincs megadva nyelvét. A többi 10 kódok nyelvet támogatja:<br/>
1. de: német<br/>
2. végű: spanyol<br/>
3. FR: francia<br/>
4. azt: olasz<br/>
5. ja: japán<br/>
6. ko: koreai<br/>
7. pt-BR: portugál (brazíliai)<br/>
8. licencelési: orosz<br/>
9. zh-HANS: egyszerűsített kínai<br/>
10. zh-HANT: hagyományos kínai<br/>

## <a name="sample-csv-file"></a>Minta CSV-fájl
Az alábbiakban a minta CSV-módosíthatók.

>[AZURE.NOTE] Másolja és illessze be a Jegyzettömb alkalmazásba, és mentse a ".csv" fájl megnyitása. Módosítsa az Excelben. Az első sor adatfeliratokkal táblába épül.

> További választható mezőket hozzáadása az Excelben a címke megadása és az oszlop alatta feltöltése.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Kapcsolódó cikkek
Keresse meg a többi cikkek az Azure Active Directory B2B együttműködési

- [Mi az Azure Active Directory B2B együttműködési?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Működése](active-directory-b2b-how-it-works.md)
- [Részletes útmutató](active-directory-b2b-detailed-walkthrough.md)
- [Külső felhasználók jogkivonat formázása](active-directory-b2b-references-external-user-token-format.md)
- [Külső felhasználók objektum attribútum megváltozik](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuális előzetes korlátai](active-directory-b2b-current-preview-limitations.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
