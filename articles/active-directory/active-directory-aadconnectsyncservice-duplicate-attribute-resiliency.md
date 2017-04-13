<properties
    pageTitle="Identitás-szinkronizálás és az ismétlődés attribútum tűrőképessége |} Microsoft Azure"
    description="Új működését (UPN) vagy ProxyAddress ütközést tartalmazó objektumok kezelése a címtár-szinkronizálás Azure AD Connect használata során."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Identitás-szinkronizálás és az ismétlődés attribútum tűrőképessége
Ismétlődő attribútum Tűrőképessége lehetővé teszi az Azure Active Directory, amely megszünteti a **UserPrincipalName** és okozta **ProxyAddress** ütközések egy Microsoft-szinkronizálás eszközei futtatásakor súrlódás.

Ez a két attribútum általában szükségesek egyedinek kell lennie az összes **felhasználó**, **csoport**vagy **partner** objektumok egy adott Azure Active Directory-bérlői webhelyen keresztül.

> [AZURE.NOTE] Csak azok a felhasználók UPN állhat.

Az új, amely lehetővé teszi, hogy ez a szolgáltatás működése az a felhő szövegterülete, a szinkronizálás során, ezért azt agnostic és valamelyik Microsoft szinkronizálási alkalmazást, többek között az Azure AD Connect, DirSync és MIM + összekötő vonatkozó ügyfél. Az általános kifejezés a "szinkronizálási ügyfélprogramot" e termékek bármelyikét ábrázolásához a dokumentumban használt.

## <a name="current-behavior"></a>Aktuális viselkedése
Ha valaki megpróbálja hozhatók létre egy új objektum (UPN) vagy ProxyAddress érték, amely sérti a egyediségét korlátozás, Azure Active Directory zárolja az adott objektum létrehozását. Hasonlóképpen ha egy objektum nem egyedi (UPN) vagy más ProxyAddress frissül, a frissítés sikertelen lesz. A kiépítési kísérletet vagy a frissítés által a szinkronizálási ügyfélprogramot után minden exportálás ciklust megismétlése, és továbbra is sikertelen addig, amíg az ütközés feloldása. Hiba a jelentés e-mailben minden kísérlet során jön létre, és hibát be van jelentkezve, a szinkronizálási ügyfélprogramot szerint.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Az ismétlődő attribútum Tűrőképessége viselkedése
Teljesen adatkapcsolat kiépítése vagy ismétlődő attribútum frissítse objektum, hanem Azure Active Directory "karanténba" az ismétlődő attribútum, amely esetben nem sértheti meg a egyediségét kényszer. Ha az attribútum szükség a kiépítési, például a UserPrincipalName, a szolgáltatás a helyőrző értéket rendeli. Az ideiglenes értékek esetén  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Ha az attribútum nem szükséges, például egy **ProxyAddress**, Azure Active Directory egyszerűen az ütközés attribútum karanténba, és folytatja a objektumainak létrehozása vagy módosítása.

Az ütközési információkat, a attribútum quarantining, az azonos hiba jelentés e-mailt a régi viselkedés használt küldi. Azonban ez az információ csak akkor jelenik meg a hibajelentés egy időben karantén történik, ha nem továbbra is be kell jelentkeznie jövőbeli e-maileket. Is, mivel az Exportálás az objektum sikeresen befejeződött, a szinkronizálási ügyfélprogramot nem jelentkezzen hibát jelez, és nem újra létrehozása és frissítése után későbbi szinkronizálási ciklust művelet.

Támogatja a jelenség új attribútum a felhasználók, csoportok és partner objektum osztályok lett hozzáadva:  
**DirSyncProvisioningErrors**

Ez a egy többértékű attribútum, amely esetben nem sértheti meg a egyediségét kényszer őket hozzá kell adni a szokásos módon nem ütközik-e attribútum tárolására szolgál. Háttér időzítő feladat engedélyezve van az Azure Active Directory ismétlődő attribútum ütközések feloldása után, amely automatikusan eltávolítja a szóban forgó attribútumok karantén keres óránként lefut.

### <a name="enabling-duplicate-attribute-resiliency"></a>Ismétlődő attribútum Tűrőképessége engedélyezése
Ismétlődő attribútum Tűrőképessége lesz az új alapértelmezett működés összes Azure Active Directory-bérlők között. Szinkronizálás az első alkalommal 2016 augusztus 22.és vagy újabb beállított összes bérlők alapértelmezés szerint a lesz. Bérlők, hogy engedélyezve van a szinkronizálás előtt ezt a dátumot a szolgáltatás kötegekben engedélyezve lesz. A bevezetés meg is kezdi a szeptember 2016-ban, és e-mailben értesítést küld az adott napon, amikor a szolgáltatást engedélyezni kell minden bérlői technikai értesítés kapcsolatba.

Ha ismétlődő attribútum Tűrőképessége bekapcsolta nem lehet letiltani.

Annak ellenőrzéséhez, ha a szolgáltatás engedélyezve van-e a bérlő, ehhez le a Powershellhez Azure Active Directory modul legújabb verzióját, és fut:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Ha szeretné a szolgáltatás ezzel kapcsolatban beérkező engedélyezése előtt van kapcsolva a bérlő, ezt le a Powershellhez Azure Active Directory modul legújabb verzióját, és operációs rendszert futtató teheti meg:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Objektumok DirSyncProvisioningErrors azonosítása
Kétféleképp jelenleg a hibák miatt ismétlődő tulajdonság ütközések Powershellhez Azure Active Directory és az Office 365 felügyeleti portál tartalmazó objektumok azonosításához. Csomagjai meghosszabbítása további portál alapján jelentési a jövőben.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
A PowerShell-parancsmagok ebben a témakörben a következő teljesül:

- A következő parancsmagok összes kis-és nagybetűk.
- A **– ErrorCategory PropertyConflict** mindig szerepelnie kell. Jelenleg nem **ErrorCategory**más típusú, de ez a jövőben meghosszabbítható.

Első lépésként az első lépések **Csatlakozás-MsolService** fut, majd írja be a hitelesítő adatok egy Bérlői rendszergazda.

Ezután segítségével a következő parancsmagok és műveleti jelek különböző módokon hibák megtekintése:

1. [Látható a teljes](#see-all)

2. [Típus szerint](#by-property-type)

3. [Nem ütközik-e érték szerint](#by-conflicting-value)

4. [Egy karakterlánc keresése szolgáltatással](#using-a-string-search)

5. [Rendezett](#sorted)

6. [Az összes vagy egy korlátozott mennyiség](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Látható a teljes
Miután létrejött, attribútum kiépítési általános listájának megjelenítéséhez bérlőhöz hibák futtatása:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Egy eredménye az alábbihoz hasonló:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Típus szerint
Hibák tulajdonság típus szerint megtekintéséhez adja hozzá a **UserPrincipalName** vagy a **ProxyAddresses** argumentum **- Tulajdonságnév** jelölő:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Vagy

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Nem ütközik-e érték szerint
Kapcsolatos hibákat egy adott tulajdonság vegye fel a **- Tulajdonságérték** jelző (**- Tulajdonságnév** kell használni, valamint a jelző hozzáadása):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Egy karakterlánc keresése szolgáltatással
Egy széles karakterlánc keresési használja a **- KeresendoString** jelölőre. Ez használható egymástól függetlenül minden a fenti jelölők kivételével **ErrorCategory PropertyConflict**, ami viszont mindig szükséges:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Az összes vagy egy korlátozott mennyiség
1. **MaxResults <Int> ** korlátozni szeretné a lekérdezés adott számú értékek is használható.

2. **Az összes** használható annak érdekében, hogy az összes eredményt abban az esetben, amely sok hibák létezik beolvasható.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Az Office 365 felügyeleti portál

A címtár-szinkronizálási hibák megtekintése az Office 365 felügyeleti központban. Az Office 365 portálon a jelentés csak ezeket a hibákat tartalmazó **felhasználói** objektumok jeleníti meg. Nem jelenik a **csoportok** és a **partnerek**közötti ütközések információt.


![Az aktív felhasználók] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Az aktív felhasználók")

A címtár-szinkronizálási hibák megtekintése az Office 365 felügyeleti központban, tanulmányozza [az Office 365-ben azonosítása címtár-szinkronizálási hibákat](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Identitás szinkronizálási hibára vonatkozó jelentést
Ha egy objektum egy ismétlődő attribútum ütközés kezeli értesítés szerepel a szabványos identitás szinkronizálási hibára vonatkozó jelentést e-mailt, a rendszer elküldi a technikai értesítés új megoldásához kérje meg a bérlői. Nincs azonban fontos változás az Ez a probléma. Az elmúlt egy ismétlődő attribútum ütközési információkat felveszik minden későbbi hibákról szóló jelentéseket mindaddig, amíg az ütközés megoldását. Az új megoldásához az adott ütközés hiba vonatkozó csak jelennek meg egyszer - az ütköző attribútum van karanténba helyezett időben.

Íme egy példa az értesítő e-mailt néz ki egy ProxyAddress ütközés:  
    ![Az aktív felhasználók](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Ütközések és hibák megoldása
Stratégia és a felbontás taktikák a hibák elhárítása az ismétlődő attribútum hibák múltbeli voltak kezelése nem oldalakhoz. Az egyetlen különbség, hogy az időzítő feladat automatikus hozzáadása a szóban forgó attribútum a megfelelő objektum után az ütközés megoldódott a szolgáltatás oldalon bérlői webhelyen keresztül halmokat.

A következő cikk ismerteti a különböző hibaelhárítási és megoldási stratégiák: [ismétlődő vagy érvénytelen attribútumok megakadályozzák a címtár-szinkronizálás az Office 365-ben](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Ismert problémák
Ismert problémák egyik hatására adatok elvesztése vagy szolgáltatás csökkenés. Több esztétikai, más szabványos "*előtti tűrőképessége*" ismétlődő attribútum hibát okoznak helyett az ütközés attribútum quarantining elő kell, és egy másik hatására az egyes hibák extra kézi fix felfelé megkövetelésére.

**Alapvető viselkedése:**

1. Az adott attribútumot konfigurációk objektumok nem szűnnek meg exportálás hibákat, nem pedig az éppen karanténba helyezett ismétlődő attribútum.  
Példa:

    egy. Új felhasználót hoznak létre egy egyszerű Felhasználónévi, az Active Directory **Joe@contoso.com** és ProxyAddress**smtp:Joe@contoso.com**

    b. Az objektum jellemzőinek ütköznek egy meglévő csoportot, és hol található a ProxyAddress **SMTP:Joe@contoso.com**.

    c billentyűkombinációt. Az exportálás után egy **ProxyAddress ütközés** hiba bízza meg az ütközés attribútumok karanténba helyezett elő. A művelet megismétlése, minden későbbi szinkronizálási ciklus, a tűrőképessége szolgáltatás engedélyezése előtt volna.

2. Két csoport jön létre a helyszíni SMTP ugyanazt a címet, ha egy kiépítése ismétlődő **ProxyAddress** standard hiba elsőre nem sikerül. Azonban az ismétlődő érték van megfelelően karanténba helyezett alapján a következő szinkronizálási ciklus.

**Az office portál jelentés**:

1. Két-objektumot egy egyszerű Felhasználónévi ütközés halmaz részletes hibaüzenet megegyezik. Ez azt jelzi, hogy azok mindkét volt a UPN megváltozott / karanténba helyezett, ha valójában csak egy ezek közül volt a módosított adatokat.

2. Az egyszerű Felhasználónévi ütközés részletes hibaüzenet a hibás displayName egy felhasználóhoz, akinek a módosított/karanténba helyezett UPN volt látható. Példa:

    egy. Az első be **"a" felhasználó** szinkronizálja **(UPN) = User@contoso.com **.

    b. **Felhasználói B** megpróbált szinkronizálható a következő lépés **(UPN) = User@contoso.com **.

    c billentyűkombinációt. **A "B" felhasználó** Egyszerű Felhasználónévi megfelelően módosul **User1234@contoso.onmicrosoft.com** és **User@contoso.com** **DirSyncProvisioningErrors**lett hozzáadva.

    d. A **Felhasználó** b hibaüzenet jelzi, hogy a **felhasználónak** már van **User@contoso.com** egyszerű Felhasználónévi, de a **Felhasználó B** saját displayName látható.



**Identitás szinkronizálási hibára vonatkozó jelentést**:

A hivatkozást *a probléma megoldásához* lépéseiről nem megfelelő:  
    ![Az aktív felhasználók](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

[Https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency)kell mutatnia.


## <a name="see-also"></a>Lásd még:

- [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md)

- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)

- [Az Office 365-ben címtár-szinkronizálási hibák azonosítása](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
