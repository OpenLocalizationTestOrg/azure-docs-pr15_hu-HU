<properties
    pageTitle="Mi az Azure önkiszolgáló regisztrációs képernyője? | Microsoft Azure"
    description="Az Azure-áttekintés önkiszolgáló regisztrációs, hogy miként kezelheti a regisztrációs folyamat, és hogyan tartománynévre átvétele."
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


# <a name="what-is-self-service-signup-for-azure"></a>Mi az Azure önkiszolgáló regisztrációs képernyője?

Ez a témakör ismerteti, hogy az önkiszolgáló regisztrációs folyamat és hogyan átvétele tartománynévre.  

## <a name="why-use-self-service-signup"></a>Miért érdemes használni az önkiszolgáló regisztrációs?

- Ismerkedés az ügyfelek szolgáltatások gyorsabban szeretné őket.
- Hozzon létre egy szolgáltatás az e-mailes ajánlatok.
- Hozzon létre a regisztrációs e-mailes flow, amely gyorsan engedélyezése a felhasználóknak saját munka könnyen megjegyezhető e-mail aliasok használatával identitások létrehozása.
- Felügyelt Azure könyvtárak később be felügyelt könyvtárak tehetők és más szolgáltatásokhoz felhasználható.

## <a name="terms-and-definitions"></a>Fogalmak és definíciók

+ **Önkiszolgáló regisztráció**: Ez a lehetőség egy felhőalapú szolgáltatásba feliratkozik egy felhasználó és magában identitás automatikusan létrejön egy őket az Azure Active Directory (Azure Active Directory) alapján a levelezési tartomány.
+ **Felügyelt Azure directory**: Ez az a könyvtár, ahol identitás jön létre. Egy nem felügyelt könyvtár (könyvtárában, amelynek a globális rendszergazda nélkül).
+ **E-mailek ellenőrzött felhasználói**: a felhasználói fiók típusát: az Azure Active Directory. Egy felhasználó, aki rendelkezik a önkiszolgáló ajánlatra a regisztráció után automatikusan létrehozott identitás-e-mailek ellenőrzött felhasználói nevezik. Egy e-mailek ellenőrzött felhasználó tagja normál címkével ellátott creationmethod könyvtár = EmailVerified.

## <a name="user-experience"></a>Felhasználói élmény

Ha például tegyük fel, hogy egy felhasználó, amelynek küldése e-mailben Dan@BellowsCollege.com bizalmas fájlok mailben kapja. A fájlok védelemmel rendelkeznek Azure Rights Management (Azure RMS). De Dan a szervezet, csőrugó főiskolai, nem regisztrált az Azure Rights Management szolgáltatást, és nem rendelkezik rendszerbe az Active Directory Tartalomvédelmi. Ebben az esetben Dan regisztrálhat Tartalomvédelmi a személyekhez ingyenes előfizetést érdekében olvassa el a védett fájlokat.

Ha a Dan az első felhasználó számára a Regisztrálás a önkiszolgáló kínáló, majd egy nem felügyelt könyvtár létrejön BellowsCollege.com az Azure Active Directory BellowsCollege.com e-mail címre. Ha más felhasználók a BellowsCollege.com tartományból regisztrálni a felületek vagy hasonló önkiszolgáló felületek, azok is ugyanabban a mappában nem felügyelt Azure-ban létrehozott e-mail ellenőrzött felhasználói fiókokat.

## <a name="admin-experience"></a>Rendszergazdai élmény

A DNS-tartománynév egy nem felügyelt Azure könyvtár felhasználójának rendszergazda is átvétele vagy a címtárban egyesítése után igazolja a tulajdonjogát. A következő szakaszok ismertetik a felügyeleti felület részletesebben, de az alábbiakban összefoglaljuk:

- Ha elvégzi fölé egy nem felügyelt Azure directory, a nem felügyelt könyvtár globális rendszergazdaként egyszerűen válik. Egy belső nyilvános vételi is nevezik.
- Ha egy nem felügyelt Azure directory egyesítés, a nem felügyelt könyvtár DNS-tartomány nevét a felügyelt Azure directory és a felhasználók erőforrások szükségesek a hozzárendelés jön létre úgy felhasználók továbbra is elérhető szolgáltatások megszakítás nélkül. Egy külső nyilvános vételi is nevezik.

## <a name="what-gets-created-in-azure-active-directory"></a>Mi az Azure Active Directory kap létrehozott?

#### <a name="directory"></a>a címtár

- Az Azure Active directory tartományhoz tartozó jön létre, egy adott könyvtár egy tartományt.
- Az Azure Active directory még nincs globális rendszergazdaként.

#### <a name="users"></a>Felhasználók

- A user objektumban minden olyan felhasználóhoz, aki feliratkozik, az Azure Active directory jön létre.
- Egyes felhasználói objektum be van jelölve a külső.
- Minden felhasználónak van hozzáférési a szolgáltatás, hogy előfizetett.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Hogyan formál önkiszolgáló Azure AD könyvtár az enyém a tartomány?

Azure AD önkiszolgáló igényelhet, tartomány-ellenőrzés végrehajtásával könyvtár. Tartomány-ellenőrzés igazolja, hogy Öné a tartomány DNS-rekordok létrehozásával.

Két módon végezze el a DNS nyilvános vételi egy Azure Active Directory:

- belső nyilvános vételi (felügyeleti egy nem felügyelt Azure directory találja, és szeretne egy felügyelt könyvtár átalakítása)
- külső nyilvános vételi (Admin biztos, hogy az új tartomány felvétele a felügyelt Azure directory)

Előfordulhat, hogy a tartomány tulajdonjogával, mert véve fölé egy nem felügyelt könyvtár felhasználó elvégzett való önkiszolgáló regisztráció vagy, előfordulhat, hogy kell tartományt vehet fel az új egy meglévő felügyelt könyvtár érvényesítése érdekli. Például a contoso.com névvel ellátott tartománnyal rendelkezik, és fel szeretné venni a contoso.co.uk vagy contoso.uk nevű új tartományt.

## <a name="what-is-domain-takeover"></a>Mi az, hogy a tartományban nyilvános vételi?  

Ez a szakasz bemutatja, hogy miként, hogy Öné a tartomány ellenőrzése

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Mi az tartomány-ellenőrzés, és miért van rá használt?

Annak érdekében, hogy könyvtárában műveletek hajthatók végre, az Azure Active Directory szükséges, hogy a DNS-tartomány tulajdonjogának ellenőrzése.  A tartomány hitelesítése lehetővé teszi a címtár, és előléptetése az önkiszolgáló Directoryval, hogy egy felügyelt könyvtár formál, vagy az önkiszolgáló könyvtár egyesítése meglévő felügyelt könyvtárat.

## <a name="examples-of-domain-validation"></a>Példák a tartomány-ellenőrzés

Két módon végezze el a DNS nyilvános vételi könyvtár:

+ belső nyilvános vételi (például rendszergazda önkiszolgáló, nem felügyelt könyvtárában találja, és szeretne felügyelt címtár átalakítása)
+ külső nyilvános vételi (például egy felügyeleti próbál új tartomány felvétele az felügyelt könyvtárában)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Belső nyilvános vételi - kell egy felügyelt könyvtár önkiszolgáló, nem felügyelt könyvtárában előléptetése

Belső nyilvános vételi ekkor a címtárban a kap egy nem felügyelt címtárból felügyelt könyvtárában alakulnak. DNS tartomány neve érvényességi, ahol hoz létre az MX rekord vagy egy TXT rekordot a DNS-zóna szükséges. Ezt a műveletet:

+ Ellenőrzi, hogy Öné a tartomány
+ A felügyelt könyvtár teszi
+ Lehetővé teszi, hogy a globális rendszergazdák a címtár

Tegyük fel, hogy a csőrugó főiskolai rendszergazdai találja, hogy az iskola felhasználóit regisztráltak önkiszolgáló szeretne rendelni. A DNS regisztrált tulajdonosának neve BellowsCollege.com, az informatikai rendszergazda is Azure-ban a tartománynév tulajdonjogának ellenőrzése és a nem felügyelt könyvtár majd átvétele. A könyvtár lesz egy felügyelt könyvtár, és az informatikai rendszergazda a globális rendszergazdai szerepkör az BellowsCollege.com könyvtár van-e hozzárendelve.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Külső nyilvános vételi - önkiszolgáló könyvtárában egyesítése egy meglévő felügyelt címtár

Egy külső nyilvános vételi, a már van egy felügyelt könyvtárat és azt szeretné, hogy az összes felhasználók és csoportok nem felügyelt Directoryból felügyelt könyvtár Bekapcsolódás helyett saját két külön könyvtárak.

A felügyelt könyvtár rendszergazdaként felvett egy tartományt, és azt a tartományt, hogy egy nem felügyelt könyvtár társítva történik.

Ha például tegyük fel, hogy Ön rendszergazdai és -e már egy felügyelt könyvtár a Contoso.com a tartománynév adott a szervezethez regisztrált. Észleli, hogy a szervezeten belüli felhasználók elvégeztetni önkiszolgáló be egy egyesíti az e-mailek tartománynév használatával user@contoso.co.uk, melyiket érdemes a tulajdonosa a szervezet egy másik tartománynév. Azoknak a felhasználóknak jelenleg nincs fiókja egy nem felügyelt contoso.co.uk Directoryban.

Nem kívánt kezelése a két külön könyvtárak, így a nem felügyelt könyvtár contoso.co.uk egyesítése meglévő felügyelt informatikai címtárában a contoso.com.

Külső nyilvános vételi DNS ellenőrzési folyamatot követi, belső nyilvános vételi.  Éppen különbség: felhasználók és szolgáltatások újra a felügyelt informatikai címtárhoz társítva.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Mi az, hogy milyen következményekkel járnak végrehajtása egy külső nyilvános vételi?

Egy külső nyilvános vételi, a felhasználók-erőforrások megfeleltetésének jön létre, így a felhasználók továbbra is zavartalanul szolgáltatások eléréséhez. Sok alkalmazások, többek között a Tartalomvédelmi a személyekhez, kezeli jól felhasználók-erőforrások hozzárendelése a, és a felhasználók továbbra is elérhető szolgáltatások módosítás nélkül. Ha egy alkalmazás nem kezeli a felhasználók-erőforrások hozzárendelése hatékony, külső nyilvános vételi kifejezetten blokkolhatja a felhasználók számára letiltott művelet a gyenge élmény.

#### <a name="directory-takeover-support-by-service"></a>a címtár vételi támogatási szolgáltatás

Jelenleg az alábbi szolgáltatásokat támogatja a nyilvános vételi:

- DIRECTORY TARTALOMVÉDELMI SZOLGÁLTATÁSOK


Az alábbi szolgáltatások hamarosan fog támogató nyilvános vételi:

- PowerBI

A következő nem, és csak a felhasználói adatok áttelepítése után egy külső nyilvános vételi további felügyeleti műveletet.

- A SharePoint/onedrive-on


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Hogyan végezhetők el a DNS tartomány neve nyilvános vételi

Hajtsa végre a tartomány-ellenőrzés (és végezze el a nyilvános vételi, ha azt szeretné, hogy) módját néhány lehetőség közül választhat:

1.  Azure Kezelőportálja segítségével

    Nyilvános vételi induljanak hajtsa végre a tartomány hozzáadásával.  Ha egy könyvtár már létezik a tartományhoz, a vezérlőt, amellyel végrehajtása egy külső nyilvános vételi fognak rendelkezésére állni.

    Jelentkezzen be az Azure-portálra a hitelesítő adataival.  Nyissa meg a címtárában meglévő, majd a **tartomány hozzáadása**.

2.  Az Office 365-ben

    Az Office 365-ben a [tartományok kezelése](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) lapon a beállítások segítségével a tartományait és DNS-rekordokat. Lásd: [az Office 365-ben a tartomány tulajdonjogának igazolása](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  A Windows PowerShell

    Az alábbi lépéseket a Windows PowerShell használatá érvényesség elvégzéséhez szükséges.

    Lépés    |   Parancsmag használata
    ------- | -------------
    Hitelesítőadat-objektum létrehozása | Get-hitelesítő adatok
    Azure Active Directory összekapcsolása | Csatlakozás MsolService
    a tartományok listájának   | Get-MsolDomain
    Hozzon létre egy beavatkozás igazolására szolgáló eljárás  | Get-MsolDomainVerificationDns
    DNS-rekord létrehozása   | Ehhez a DNS-kiszolgálón
    Ellenőrizze a beavatkozás igazolására szolgáló eljárás    | Erősítse meg MsolEmailVerifiedDomain

Példa:

1. Azure AD az önkiszolgáló felületek megválaszolásához használt hitelesítő adataival csatlakozni: a modul importálása MSOnline $msolcred = get-hitelesítő adatok csatlakoztatása msolservice-$msolcred hitelesítő adatok

2. A tartományok listájának megjelenítése:

    Get-MsolDomain

3. Futtassa a Get-MsolDomainVerificationDns parancsmag bonyolulttá létrehozása:

    Get-MsolDomainVerificationDns – tartománynév *tartománynév* – mód DnsTxtRecord

    Példa:

    Get-MsolDomainVerificationDns – DomainName contoso.com – mód DnsTxtRecord

4. Ez a parancs a visszaadott érték (kérdés) másolása

    Példa:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. A nyilvános DNS-névtér hozzon létre egy DNS-txt rekord, amely tartalmazza az előző lépésben kimásolt értéket.

    Ez a rekord nevét a szülő tartománynevet, így a DNS-szerepkör Windows Server használatával e erőforrás rekordot hoz létre, ha a rekord neve üres, és csak beillesztése a hagyja a szövegdobozba

6. Futtassa a fájlformátum-MsolDomain parancsmagot a kérdés ellenőrzése:

    Erősítse meg MsolEmailVerifiedDomain - DomainName *tartománynév*

    Példa:

    Erősítse meg MsolEmailVerifiedDomain - DomainName contoso.com

A sikeres bonyolulttá eredménye a kérdésre hiba nélkül.

## <a name="how-do-i-control-self-service-settings"></a>Hogyan szabályozható önkiszolgáló beállítások?

A rendszergazdák két önkiszolgáló vezérlők ma van. Vezérelheti:

- Engedélyezi vagy tiltja a felhasználók csatlakozhatnak a címtárban mailben.
- Függetlenül attól, felhasználók is licenc maguk alkalmazások és szolgáltatások számára.


### <a name="how-can-i-control-these-capabilities"></a>Hogyan szabályozhatja e ezekre a lehetőségekre?

Ezek a funkciók ezek Azure AD-parancsmag Set-MsolCompanySettings paramétereket alkalmaz a rendszergazda adhatja meg:

+ **AllowEmailVerifiedUsers** szabályozza, hogy a felhasználó létrehozása, vagy egy nem felügyelt könyvtár Bekapcsolódás. Ha a paraméter értéke $false, nincs e-mail ellenőrzött felhasználók is csatlakozni tudnak a címtár.
+ **AllowAdHocSubscriptions** meghatározza, hogy a felhasználók felfelé önkiszolgáló végrehajtásához. Ha a paraméter értéke $false, senki való önkiszolgáló regisztráció végezheti el.


### <a name="how-do-the-controls-work-together"></a>A vezérlők működése együtt

Önkiszolgáló pontosabban szeretné szabályozni definiálása felfelé e két paraméterek használható együtt. Ha például a következő parancs segítségével a felhasználók hajtsa végre, önkiszolgáló felfelé, de csak ha ezek a felhasználók már rendelkezik fiókkal az Azure Active Directory (más szóval, a felhasználók, akik kellene hozható létre az e-mailek ellenőrzött fiók nem hajtható végre önkiszolgáló felfelé):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Az alábbi folyamatábra ismerteti, hogy a különböző kombinációk paraméterértékek és az eredményül kapott feltételek könyvtár és önkiszolgáló be.

![][1]

További információt és példákat, hogyan használhatja ezeket a paramétereket olvassa el a [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)című témakört.

## <a name="see-also"></a>Lásd még:

-  [Telepítse és állítsa be a Azure PowerShell hogyan](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
