<properties
    pageTitle="Azure AD Connect: A szinkronizálás során hibáinak elhárítása |} Microsoft Azure"
    description="Megtudhatja, hogyan kapcsolatos hibák elhárítása Azure AD Connect való szinkronizálás közben."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>A szinkronizálás során hibáinak elhárítása
Hiba akkor fordulhat elő, amikor identitás adatok szinkronizálása a Windows Server Active Directory (AD DS) Azure Active Directory (Azure Active Directory). Ez a cikk áttekintést nyújt a különböző típusú szinkronizálási hibákat, néhány azok a hibák és a hibák kijavításának potenciális módjai okozó lehetséges esetet tárgyal. Ez a cikk a gyakori hibák-típusokat, és előfordulhat, hogy nem tárgyalja a lehetséges hibák.

 Ez a cikk azt feltételezi, hogy az olvasó ismerős az alapul szolgáló [tervezése a Azure Active Directory és Azure AD Connect](active-directory-aadconnect-design-concepts.md).

A legfrissebb verziójához készült Azure AD Connect \(augusztus 2016 vagy újabb\), a szinkronizálási hibákat jelentés érhető el az [Azure-portálon](https://aka.ms/aadconnecthealth) szinkronizálási az Azure Active Directory csatlakozás állapot részeként.


2016 szeptember 1 kezdési [Azure Active Directory ismétlődő attribútum Tűrőképessége](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) szolgáltatás alapértelmezés szerint az összes *Új* Azure Active Directory bérlők engedélyezi. Ez a szolgáltatás automatikusan engedélyezi a meglévő bérlők a közelgő hónapra.

Azure AD Connect a könyvtárak szinkronizálja továbbra is a 3 típusú műveleteket hajtja végre: importálása és szinkronizálása, exportálás. Hiba történhet az összes művelet. Ez a cikk főként szolgáltatásaival kapcsolatos hibák Azure AD az exportálás során.

## <a name="errors-during-export-to-azure-ad"></a>Exportálás az Azure Active Directory hibák
Következő szakasz ismerteti a különböző típusú az exportálási művelet Azure ad az Azure Active Directory-összekötő használata során előforduló szinkronizálási hibákat. Ez az összekötő azonosítható a név formátumát alatt "contoso. *onmicrosoft.com*".
Exportálás az Azure Active Directory hibák jelzésére, hogy a művelet \(hozzáadása, frissítése és törlése stb\) megpróbálkozik vele a rendszer által Azure AD Connect \(Sync Engine\) Azure Active Directory sikertelen volt.

![Exportálás hibák áttekintése](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Adatok Típuseltérési hiba
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Leírás
- Ha az Azure AD Connect \(sync engine\) arra utasítja az Azure Active Directory hozzáadása vagy frissítése az objektumok, az Azure Active Directory megegyezik a bejövő objektum **sourceAnchor** attribútummal az objektumok **immutableId** attribútumának az Azure Active Directory. A hol.van neve a **Merevlemez megfelelően**.
- Ha az Azure Active Directory **nem talál** olyan objektumokat, amely megfelel a **immutableId** attribútum a bejövő objektum **sourceAnchor** attribútumot tartalmazó egy új objektum kiépítése előtt azt visszavált a ProxyAddresses és a UserPrincipalName attribútum segítségével talál egyezést. A hol.van neve **Finom megfelelően**. A lágy felel meg lett tervezve megfelelően jelenítik meg az azonos entitás (felhasználók, csoportok) helyszíni alatt a szinkronizálás során hozzáadott/frissített új objektumokkal már szerepel az Azure Active Directory (tehát az Azure Active Directory kifejezéskészletébe) objektumok.
- **InvalidSoftMatch** hiba történik, ha a merevlemez egyezést nem talál egyező objektumokat **és** finom megfelelően megtalálja a megfelelő objektum, de az objektum *immutableId* , mint a bejövő objektum *SourceAnchor*, javaslat, hogy a az egyező objektum lett szinkronizálva a helyszíni Active Directory egy másik objektum egy másik értékkel rendelkezik.

Más szóval ahhoz, hogy a munkát a finom egyezést, finom egyező az objektum nem kell a *immutableId*értékét. Ha bármelyik objektumot tartalmazó *immutableId* be egy érték nem működnek a merevlemez-egyezést, de a művelet eredményezne InvalidSoftMatch szinkronizálási hiba a finom-egyezés feltételeknek eleget tevő.

Azure Active Directory-séma nem teszi lehetővé, hogy az alábbi attribútumai azonos értékkel két vagy több objektum. \(Ez a teljes lista nem.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- Objektumazonosító

>[AZURE.NOTE] [Azure Active Directory-attribútum ismétlődő attribútum Tűrőképessége](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) funkció akkor is éppen közzétételének Azure Active Directory alapértelmezés szerint.  Ez csökkenti a szinkronizálási hibákat Azure AD Connect (, valamint egyéb szinkronizálási ügyfelek) látják számát Azure Active Directory rugalmasabb legyen a többszörös terhet ProxyAddresses, mind a UserPrincipalName attribútum a helyszíni Active Directory-verziót tartalmazó környezetek kezelését. Ez a funkció nem oldja meg az ismétlődési hibák. Így az adatok továbbra is javításra szorul. De lehetővé teszi, hogy kiépítési az új objektumokat, amelyeket egyébként programfájltípusok az Azure Active Directory folyamatban miatt ismétlődő értékeket. Ez is csökkenti a szinkronizálási hibákat a szinkronizálás ügyfél vissza számát.
Ez a funkció az bérlői webhelyen engedélyezett, ha nem jelenik meg az új objektumok kiépítési alatt látható InvalidSoftMatch szinkronizálási hibákat.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Példák a InvalidSoftMatch
1. Két vagy több objektumok a ProxyAddresses attribútumból azonos értékkel rendelkező létezik a helyszíni Active Directory. Csak az első kiépítve az Azure Active Directory.
2. A userPrincipalName azonos értékkel rendelkező két vagy több objektum létezik a helyszíni Active Directory. Csak az első kiépítve az Azure Active Directory.
3. Objektum hozzá lett adva az alkalmazás a helyi Active Directory, a ProxyAddresses attribútumból azonos értékkel rendelkező megegyezik egy meglévő Azure Active Directory-objektum. Az objektum helyszíni hozzáadott nincs az Azure Active Directory első kiépítve.
4. Objektum hozzá lett adva a helyszíni Active Directory, a userPrincipalName attribútum azonos értékkel rendelkező, mint a Azure Active Directory-fiók. Az objektum nincs az Azure Active Directory első kiépítve.
5. A szinkronizált fiók helyezett erdőből A erdő b Azure AD Connect (szinkronizálás motor) – ObjectGUID attribútum használta a SourceAnchor számítja ki. Az erdő áthelyezés után a SourceAnchor értéke különböző. Az új objektum (a erdő B) az Azure Active Directory a meglévő objektummal szinkronizálása sikertelen.
6. A szinkronizált objektumok használ véletlenül törölt helyszíni Active Directory egy új objektum létrehozásának és az Active Directory az azonos entitás (például a felhasználó) az Azure Active Directory-fiók törlése nélkül. Az új fiók nem tudja szinkronizálni a meglévő Azure AD-objektumot.
7. Azure AD Connect eltávolítása és újbóli telepítve. Ismét a telepítés során egy másik attribútum választotta, mint a SourceAnchor. Az objektumok, amelyek korábban is szinkronizálta a InvalidSoftMatch jelű leállt.

#### <a name="example-case"></a>Példa eset:
1. **Péter Kovács** az Azure Active Directory-a helyszíni Active Directory *contoso.com* szinkronizált felhasználó
2. Péter Kovács **UserPrincipalName** van beállítva **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** Péter Kovács **objectGUID** az alkalmazás használatával Azure AD Connect kiszámított **SourceAnchor** helyiségek az Active Directory, amely olyan, a **immutableId** Péter Kovács az Azure Active Directory-.
4. Péter tartalmaz értéket a **proxyAddresses** attribútumból a következő:
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Új felhasználó, **Péter Taylor**, hozzáadódik az alkalmazás helyszíni Active Directory.
6. Péter Taylor **UserPrincipalName** van beállítva **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** Péter Taylor **objectGUID** az alkalmazás használatával Azure AD Connect kiszámított **sourceAnchor** helyiségek az Active Directory. Péter Taylor objektum még nem szinkronizált Azure Active Directory még.
8. Péter Taylor rendelkezik az alábbi értékeket, a proxyAddresses attribútumból számára
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Szinkronizálás során Azure AD Connect felismer Péter Taylor a helyszíni Active Directory hozzáadásával, és kérje meg az azonos módosítást Azure AD.
10. Azure Active Directory először végez kemény egyezés. Ez azt jelenti, hogy ha az objektumra a immutableId egyenlő megkeresi "abcdefghijkl0123456789 ==". Merevlemez egyezés meghiúsul, nincs más objektum az Azure Active Directory fog rendelkezni, hogy immutableId.
11. Azure Active Directory majd megkísérli Péter Taylor finom-egyezés. Ez azt jelenti, hogy a fog keresni Ha bármelyik objektumot a proxyAddresses a három értéket, beleértve a egyenlősmtp:bob@contoso.com
12. Azure Active Directory megtalálja a finom-egyezés feltételeknek Péter Kovács objektumot. Az objektum immutableId értéke, de = "abcdefghijklmnopqrstuv ==". ami azt jelenti, hogy az objektum szinkronizálás időpontjának helyszíni Active Directory át egy másik objektumról. Ennélfogva Azure Active Directory nem finom-egyezés ezeknek az objektumoknak és **InvalidSoftMatch** szinkronizálási hibát eredményez.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Javítás InvalidSoftMatch hiba
A leggyakoribb a InvalidSoftMatch hiba oka két különböző SourceAnchor objektumok \(immutableId\) ugyanazt az értéket a ProxyAddresses és/vagy a UserPrincipalName attribútum, a finom-egyezés során Azure Active Directory használt van. Az érvénytelen finom egyező javítása érdekében

1.  Azonosítsa az ismétlődő proxyAddresses, userPrincipalName vagy más attribútumérték a hibát okozó. Is azonosítani tudja az két \(vagy több\) objektumok játszik szerepet az ütközés. A jelentés [Szinkronizálása az Azure Active Directory csatlakozás állapot](https://aka.ms/aadchsyncerrors) által generált segíthet azonosítani a két objektum.
2. Azonosítja, melyik objektumhoz továbbra is, hogy a másolt értéket, és melyik objektumhoz nem kellene.
3. A másolt értéket eltávolítása az objektumot, amely nem kell rendelkeznie az adott értéket. Figyelje meg, hogy kell a változtatásokat a címtárban, ahol az objektum származó. Egyes esetekben előfordulhat törölje az ütköző az objektumok egyikét.
4. A módosítást AD meg helyiségben lehetővé teszik a Azure AD Connect szinkronizálása a módosítást.

Ne feledje, hogy belül szinkronizálása az Azure Active Directory csatlakozás állapota szinkronizálás hibákról szóló jelentéseket 30 percenként frissül a legújabb szinkronizálási kísérlet hibák tartalmazza.

>[AZURE.NOTE] ImmutableId, definíció, ne módosítsa az objektum időtartama. Azure AD Connect nem konfigurálta az egyes az jelenik meg a fenti listában szem előtt, ha Ön is a végeredmény olyan helyzetben, ahol a Azure AD Connect, az Active Directory-objektum, amely megegyezik az SourceAnchor különböző értéket adja eredményül, amely tartalmaz egy meglévő Azure Active Directory-objektum továbbra is használni kívánt személy (például egy felhasználó vagy csoport vagy partner stb).

#### <a name="related-articles"></a>Kapcsolódó cikkek
- [Az ismétlődő vagy érvénytelen attribútumok megakadályozzák a címtár-szinkronizálás az Office 365-ben](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Leírás
Amikor megkísérel Azure Active Directory világos felel meg a két objektumok, akkor lehet, hogy a különböző két objektumok "objektumtípus" (például a felhasználói, csoport, kapcsolattartó stb.) az attribútumok végzik a finom hol.van azonos értékkel rendelkezik. A következő attribútumok párhuzamos nem engedélyezve van az Azure Active Directory, a művelet "ObjectTypeMismatch" szinkronizálási hibát eredményez.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Példák ObjectTypeMismatch hibák
- Az Office 365-ben engedélyezett levelezési biztonsági csoport jön létre. Rendszergazdai helyszíni (tehát nem szinkronizálja a rendszer az Azure Active Directory még) AD a ProxyAddresses attribútumból az azonos értékű-e az Office 365-csoport összeadja az új felhasználó vagy névjegyet.

#### <a name="example-case"></a>Példa eset

1. Új engedélyezett levelezési biztonsági csoport létrehozása az Office 365-ben a adó részleg rendszergazdája, és egy e-mail címet, mint itt tax@contoso.com. Ez rendel a ProxyAddresses attribútumból értékét a csoport**smtp:tax@contoso.com**
2. Új felhasználó csatlakozik, akkor a Contoso.com és a fiók jön létre a felhasználó a proxyAddress, mint a helyszínen**smtp:tax@contoso.com**
3. Azure AD Connect szinkronizálja az új felhasználói fiókot, amikor azt a "ObjectTypeMismatch" hibaüzenet jelenik meg.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Javítás ObjectTypeMismatch hiba
A leggyakoribb a ObjectTypeMismatch hiba oka két különböző típusú (felhasználói, csoport névjegy stb.) objektumoknak ugyanazt az értéket a ProxyAddresses attribútumból. Annak érdekében, hogy a ObjectTypeMismatch javítása:

1.  Azonosítsa az ismétlődő proxyAddresses (vagy más attribútum) érték, amely a hibát okoz. Is azonosítani tudja az két \(vagy több\) objektumok játszik szerepet az ütközés. A jelentés [Szinkronizálása az Azure Active Directory csatlakozás állapot](https://aka.ms/aadchsyncerrors) által generált segíthet azonosítani a két objektum.
2. Azonosítja, melyik objektumhoz továbbra is, hogy a másolt értéket, és melyik objektumhoz nem kellene.
3. A másolt értéket eltávolítása az objektumot, amely nem kell rendelkeznie az adott értéket. Figyelje meg, hogy kell a változtatásokat a címtárban, ahol az objektum származó. Egyes esetekben előfordulhat törölje az ütköző az objektumok egyikét.
4. A módosítást AD meg helyiségben lehetővé teszik a Azure AD Connect szinkronizálása a módosítást. Szinkronizálási hibákról szóló jelentéseket belül szinkronizálása az Azure Active Directory csatlakozás állapot 30 percenként printet frissítik, és a legújabb szinkronizálási kísérlet hibák tartalmaz.


## <a name="duplicate-attributes"></a>Ismétlődő attribútumok
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Leírás
Azure Active Directory-séma nem teszi lehetővé, hogy az alábbi attribútumai azonos értékkel két vagy több objektum. Ez az egyes objektumokra az Azure Active Directory kelljen egy egyedi érték, a következő attribútumok rendelkezik egy adott példányhoz.

- ProxyAddresses
- UserPrincipalName

Ha Azure AD Connect megpróbál hozzáadása egy új objektum vagy egy meglévő objektum frissítse a fenti attribútumok egy érték, amely már hozzá van rendelve egy másik objektum az Azure Active Directory, a művelet a "AttributeValueMustBeUnique" szinkronizálási hibát eredményez.
#### <a name="possible-scenarios"></a>Lehetséges alkalmazási helyzetek:
1. Ismétlődő érték már szinkronizált objektum, amely egy másik szinkronizált objektum ütközik van rendelve.

#### <a name="example-case"></a>Példa eset:
1. **Péter Kovács** az Azure Active Directory-a helyszíni Active Directory contoso.com szinkronizált felhasználó
2. Péter Kovács **UserPrincipalName** helyszíni van beállítva **bobs@contoso.com**.
3. Péter tartalmaz értéket a **proxyAddresses** attribútumból a következő:
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Új felhasználó, **Péter Taylor**, hozzáadódik az alkalmazás helyszíni Active Directory.
5. Péter Taylor **UserPrincipalName** van beállítva **bobt@contoso.com**.
6. **Péter Taylor** a **ProxyAddresses** attribútumban i a következő értékeket tartalmaz. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Péter Taylor objektum szinkronizálja a rendszer az Azure Active Directory sikeresen.
8. Rendszergazda úgy döntött, hogy Péter Taylor **ProxyAddresses** attribútumban frissítse a következő értéket: lehet. **smtp:bob@contoso.com**
9. Azure Active Directory megpróbálja Péter Taylor objektum frissítése az Azure Active Directory, a fenti értéket, de ez a művelet sikertelen lesz, hogy értéket a ProxyAddresses már hozzá van rendelve Péter Kovács "AttributeValueMustBeUnique" hibát eredményez.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Javítás AttributeValueMustBeUnique hiba
A leggyakoribb a AttributeValueMustBeUnique hiba oka két különböző SourceAnchor objektumok \(immutableId\) ugyanazt az értéket a ProxyAddresses és/vagy a UserPrincipalName attribútum van. Annak érdekében, hogy AttributeValueMustBeUnique hiba javítása

1.  Azonosítsa az ismétlődő proxyAddresses, userPrincipalName vagy más attribútumérték a hibát okozó. Is azonosítani tudja az két \(vagy több\) objektumok játszik szerepet az ütközés. A jelentés [Szinkronizálása az Azure Active Directory csatlakozás állapot](https://aka.ms/aadchsyncerrors) által generált segíthet azonosítani a két objektum.
2. Azonosítja, melyik objektumhoz továbbra is, hogy a másolt értéket, és melyik objektumhoz nem kellene.
3. A másolt értéket eltávolítása az objektumot, amely nem kell rendelkeznie az adott értéket. Figyelje meg, hogy kell a változtatásokat a címtárban, ahol az objektum származó. Egyes esetekben előfordulhat törölje az ütköző az objektumok egyikét.
4. A módosítást AD meg helyiségben lehetővé teszik a rögzített get-hibának a változás szinkronizálása Azure AD Connect.

#### <a name="related-articles"></a>Kapcsolódó cikkek
-[Az ismétlődő vagy érvénytelen attribútumok megakadályozzák a címtár-szinkronizálás az Office 365-ben](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Adatok Fájlellenőrzésének sikertelensége
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Leírás
Azure Active Directory különböző korlátozások magát az adatok előtt lehetővé teszi, hogy az adatok ahhoz, hogy a címtár-be kényszeríti. Ez a annak érdekében, hogy végfelhasználók számára beolvasása a legjobb lehetőség változat függő ezeket az adatokat az alkalmazások használata közben.
#### <a name="scenarios"></a>Felhasználási területei
egy. A UserPrincipalName attribútum érték/nem támogatott érvénytelen karaktereket tartalmaz.
b. A UserPrincipalName attribútum nem fogadja el a szükséges formátumot.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Javítás IdentityDataValidationFailed hiba

egy. Győződjön meg arról, hogy a userPrincipalName attribútum karakterek és a szükséges formátumú támogat.

#### <a name="related-articles"></a>Kapcsolódó cikkek
- [Hozhatók létre felhasználók az Office 365 címtár-szinkronizáláson keresztül]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Leírás
Ez a **"DataValidationFailed"** szinkronizálási hibát eredményez, amikor a felhasználó UserPrincipalName a toldalékot módosul szövetséges tartományokból más szövetséges tartományban nagyon konkrét esetből.

#### <a name="scenarios"></a>Felhasználási területei
Szinkronizált felhasználók esetében a UserPrincipalName utótag változott szövetséges tartományokból helyszíni más szövetséges tartományban. Ha például *UserPrincipalName = bob@contoso.com * változott *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Példa
1. Péter Szabó egy fiókot, akkor a Contoso.com a rendszer hozzáadja az Active Directory és a UserPrincipalName az új felhasználókéntbob@contoso.com
2. Péter egy másik osztási Fabrikam.com nevű contoso.com lép, és a UserPrincipalName változikbob@fabrikam.com
3. A contoso.com és a fabrikam.com tartományai az Azure Active Directory címtárral szövetséges tartományban.
4. Péter userPrincipalName nem frissülnek, és a "DataValidationFailed" szinkronizálási hibát eredményez.

#### <a name="how-to-fix"></a>Javítás
Ha egy felhasználó UserPrincipalName utótag frissült a bob@ **contoso.com** való bob@ **fabrikam.com**, **akkor a contoso.com** és a **fabrikam.com** hol **szövetséges tartományban**, majd hajtsa végre az alábbi lépéseket követve javítsa ki a szinkronizálási hibát

1. A felhasználó UserPrincipalName frissítése az Azure Active Directory bob@contoso.com való bob@contoso.onmicrosoft.com. Az alábbi PowerShell-parancsot is használhatja az Azure Active Directory PowerShell-modult tartalmazó:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. A következő szinkronizálási ciklus megpróbálja a szinkronizálás engedélyezése Az idő szinkronizálás sikeres lesz, és frissíti a Péter UserPrincipalName való bob@fabrikam.com is.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Leírás
Ha attribútum meghaladja a megengedett méretkorlátok vonatkoznak azokra, hossz vagy Azure Active Directory-séma által meghatározott darab határértékén, a szinkronizálási művelet eredménye a **LargeObject** vagy **ExceededAllowedLength** szinkronizálási hiba lesz. A szokásos Ez a hiba történik, az alábbi attribútumok

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Esetek
1. Péter userCertificate attribútum Péter rendelt túl sok tanúsítványok van tárolásához. Előfordulhat, hogy az érintett régebbi, a lejárt tanúsítványok.
2. Az Active Directoryban beállított Péter thmubnailPhoto fájl túl nagy ahhoz, hogy szinkronizálja az Azure Active Directory.
3. Az Active Directory a ProxyAddresses attribútumból automatikus sokasága, során objektum rendelt gépe van > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Javítás

1. Győződjön meg arról, hogy a hibát okozó attribútum belül az engedélyezett korlátozást.

## <a name="related-links"></a>Kapcsolódó hivatkozások
- [Keresse meg az Active Directory-objektumokkal az Active Directory felügyeleti központ] (https://technet.microsoft.com/library/dd560661.aspx)
- [Hogyan lekérdezés Azure Active Directory-objektum Azure Active Directory PowerShell használatával](https://msdn.microsoft.com/library/azure/jj151815.aspx)
