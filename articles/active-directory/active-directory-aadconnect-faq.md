<properties
    pageTitle="Azure AD Connect: Gyakori kérdések |} Microsoft Azure"
    description="Ezen az oldalon még kapcsolatos gyakori kérdések Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD Connect – gyakori kérdések

## <a name="general-installation"></a>Általános telepítési
**Kérdés: telepítési működnek, ha az Azure Active Directory globális rendszergazdai 2FA engedélyezve van?**  
A buildjeiben február 2016-ban, az Ez támogatott.

**Kérdés: van egy telepítse az Azure AD Connect felügyelet nélküli mód?**  
Telepítse az Azure AD Connect a telepítővarázslóval csak támogatott. A felügyelet nélküli és csendes telepítés nem támogatott.

**Kérdés: van egy erdő, ha egy tartomány nem lehet Önnel kapcsolatba lépni. Hogyan tudom telepíteni a Azure AD Connect?**  
A buildjeiben február 2016-ban, az Ez támogatott.

**Probléma: az Active Directory tartományi szolgáltatások állapotát ügynök működik a kiszolgáló core?**  
igen. Miután telepítette a agent, hajtsa végre a regisztrációs folyamat az alábbi PowerShell-parancsmag használatával: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Hálózati
**Kérdés: van a tűzfalat, a hálózati eszköz, vagy mást, hogy korlátozza a maximális időt kapcsolatok maradhat, nyissa meg a saját hálózaton. Mennyi ideig az ügyfél egymás időtúllépés küszöbértéke kell Azure AD Connect használata esetén?**  
Az összes hálózati szoftver, a fizikai eszközök vagy bármely más, amely a maximális időt kapcsolatok felkérések korlátozza a kiszolgálón, ahol az Azure AD Connect ügyfél telepítve van, és Azure Active Directory közötti kapcsolatot legalább 5 perce (300 másodperc) küszöbértéket kell használni. A korábban megjelent Microsoft Identity Szinkronizáló eszközök is érinti.

**Kérdés: van (az egyetlen címke tartományok) által támogatott?**  
Nem, Azure AD Connect nem támogatja a helyszíni erdők/tartományok által használatával.

**Kérdés: van "pontozott" nevű NetBios támogatott?**  
Nem, Azure AD Connect nem támogatja a helyszíni erdők/tartományok hol NetBios nevét tartalmazza, ponttal "." a neve.

## <a name="federation"></a>Összevonás
**K: Mit tegyek, ha lehet, de kéri, hogy az Office 365 tanúsítvány megújítása kapott e-mailt**  
Az útmutatást, amely a tanúsítványt megújítása [tanúsítványok](active-directory-aadconnect-o365-certs.md) témakörében keret használja.

**Kérdés: van "Automatikus frissítése megbízó fél" beállítása az Office 365-ös megbízó buli alaphangját. Kell tennie semmit, amikor a jogkivonat-aláíró tanúsítvány automatikusan összesíti az?**  
Az útmutatást, amely a keret határolja [tanúsítványok](active-directory-aadconnect-o365-certs.md)cikk használja.

## <a name="environment"></a>Környezet
**Kérdés: meg támogatott átnevezése a kiszolgálón, Azure AD Connect telepítése után?**  
nem. A kiszolgáló nevének módosításakor a szinkronizálási motor nem tud csatlakozni az SQL-adatbázissal, és a szolgáltatás nem tud elindulni.

## <a name="identity-data"></a>Személyes adatok
**Kérdés: az Azure Active Directory UPN (userPrincipalName) attribútum értéke nem egyezik meg a helyszíni egyszerű felhasználónév - miért?**  
Az alábbi cikkekben talál:

- [Az Office 365-ben, Azure vagy Intune felhasználóneveket nem egyeznek a helyszíni egyszerű felhasználónév vagy egy alternatív bejelentkezési azonosító](https://support.microsoft.com/en-us/kb/2523192)
- [Felhasználói fiók használatához a más szövetséges tartományban (UPN) módosítása után a módosítások nem az Azure Active Directory szinkronizáló eszköz szinkronizálása](https://support.microsoft.com/en-us/kb/2669550)

Azure Active Directory engedélyezése a szinkronizálási motor frissítheti a userPrincipalName, [Azure AD Connect szinkronizálási szolgáltatások](active-directory-aadconnectsyncservice-features.md)ismertetett módon is beállítható.

## <a name="custom-configuration"></a>Egyéni konfiguráció
**Kérdés: hol vannak a PowerShell-parancsmagok az Azure AD Connect dokumentált?**  
A parancsmagok dokumentált ezen a webhelyen, kivételével más Azure AD Connect található PowerShell-parancsmagok vannak használata nem támogatott ügyfél.

* *Kérdés: van lehetőség a "exportálási/kiszolgáló importálása" található *szinkronizálás szolgáltatáskezelő* konfiguráció áthelyezése fiókok között servers? **  
nem. Ez a beállítás nem beolvasni a beállításokat, és nem használható. A varázsló inkább a alapkonfiguráció a második-kiszolgálón hozza létre, és minden egyéni szabály áthelyezése kiszolgáló közötti PowerShell-parancsfájlokat készítése a szinkronizálási szabály szerkesztővel kell használni. Lásd: [a kiszolgáló átmeneti tárolásra szolgáló aktív egyéni konfiguráció áthelyezése](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Hibaelhárítás
**Kérdés: Hogyan kaphatok Azure AD Connect segítségre?**

[Keresse meg a Microsoft Tudásbázis (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Keresse meg a Microsoft Tudásbázis (KB) közös töréspont-problémák kijavítása Azure AD Connect támogatása technikai megoldásainak.

[Microsoft Azure Active Directory-fórumok](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Keresés és a technikai kérdések és válaszok Közösségtől a bejegyzésére vagy kérje meg a saját kérdés gombra kattintva keresse meg [az alábbi](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD Connect ügyfélszolgálat](https://manage.windowsazure.com/?getsupport=true)

- Ez a hivatkozás segítségével technikai támogatás kérése az Azure portálon keresztül.
