<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások áttekintése |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások áttekintése"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások

## <a name="overview"></a>– Áttekintés
Azure infrastruktúrájának szolgáltatásai lehetővé teszi a számítások megoldások Agilis módon széles köre üzembe. Az Azure virtuális gépeken futó, majdnem azonnal telepíthet, és csak a percet fizet. Támogatás a Windows, Linux, SQL Server, az Oracle, az IBM, SAP, és használt BizTalk, telepítheti a bármely terhelést, bármely nyelvre, szinte bármilyen operációs rendszeren. Ezek juttatások: lehetővé teszi, hogy a helyszíni telepített régebbi alkalmazások áttelepítése az Azure működési kiadások mentéséhez.

Az Azure áttelepítése a helyszíni alkalmazások kulcsfontosságú ezeket az alkalmazásokat identitás szükségleteinek kezeli. Címtár-et használó alkalmazások olvasási és írási hozzáféréssel a vállalati címtárban is LDAP támaszkodhat vagy a Windows-hitelesítés (Kerberos vagy NTLM-hitelesítés) hitelesítést végezni a végfelhasználók használja arra. A vállalati verzió (üzleti) alkalmazásokat a Windows Server rendszeren futó rendszerint a tartományhoz csatlakoztatott gépeken vannak telepítését, hogy azok is kezelhető biztonságosan csoportházirend. A vállalati identitás infrastruktúra a függőségeket helyszíni "felvonó – és – műszak" alkalmazást a felhőbe, el kell hárítani kell.

A rendszergazdák gyakran be egy, az alábbi tippekkel telepítését az Azure kérelmeiket identitás igényeinek kielégítéséhez:

- Telepítse az Azure infrastruktúrájának szolgáltatásai és a vállalati címtárban helyszíni futó feladatok közötti webhely virtuális Magánhálózati kapcsolat.
- A vállalati AD tartományerdő/infrastruktúra bővítése replika tartomány vezérlők használata Azure virtuális gépeken futó beállításával.
- Telepítse egy önálló tartományt használja a tartomány vezérlők Azure virtuális gépeken futó telepítése Azure-ban.

E két megközelítés érheti magas költség és felügyeletet. A rendszergazdák Azure virtuális gépeken futó használata tartomány vezérlők üzembe van szükség. Ezenkívül van szükségük kezelése, biztonságos, javítás, figyelése, biztonsági mentése és ezek virtuális gépeken futó elhárítása. A helyszíni címtár-virtuális Magánhálózati kapcsolatot támaszkodik telepítését az Azure érinti a átmeneti hálózati problémák vagy kimaradások munkaterhelésekből okoz. A hálózati kihagyások az alsó üzemidőt és a csökkentett megbízhatóság ezeket az alkalmazásokat az eredményez.

Azt készült Azure Active Directory tartományi szolgáltatások könnyebben alternatív megadását.


## <a name="introducing-azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások bemutatása
Azure Active Directory tartományi szolgáltatások felügyelt tartományi szolgáltatásokból, például a tartomány illesztés, csoport-házirend LDAP, Kerberos/NTLM-hitelesítés teljesen kompatibilis a Windows Server Active Directory ismertetése Az alábbi tartományi szolgáltatások meg, hogy telepíthető, kezelése és javítás tartomány vezérlők a felhőben nélkül mobilalkalmazásokban Azure Active Directory tartományi szolgáltatások integrálódik a meglévő Azure AD-bérlő, így lehetővé teszi a felhasználóknak, hogy jelentkezzen be a vállalati hitelesítő adataival. Emellett használhatja a meglévő csoportok és a felhasználói fiókok biztonságos módon elérhetők az erőforrásokat, ezáltal biztosítva egy egyenletesebb "felvonó – és – műszak" az Azure infrastruktúrájának szolgáltatásai helyszíni erőforrások.

Azure Active Directory tartományi szolgáltatások funkcióit problémamentesen működik, függetlenül attól, hogy a Azure AD-bérlő-e a felhőben, vagy az a helyszíni Active Directory címtárral szinkronizált.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure Active Directory tartományi szolgáltatások, a felhőben szervezetek
A felhő csak Azure AD-bérlő (gyakran nevezik "felügyelt bérlők") nem tartalmaz olyan helyszíni identitást helyigénye. Más szóval felhasználói fiókokat, a jelszavakat és csoporttagság az összes natív a felhőbe – Ez azt jelenti, létrehozása és kezelése a Azure AD. Fontolja meg egy pillanatra, hogy a Contoso csak egy felhőalapú Azure AD-bérlő. Ahogy az alábbi ábrán látható, Contoso rendszergazda úgy állította be a virtuális hálózati az Azure infrastruktúrájának szolgáltatásai. Alkalmazások és a kiszolgálói feladatok Azure virtuális gépeken futó virtuális hálózat van telepítve. Mivel a Contoso felhőben bérlői webhelyet, az összes felhasználói identitások, a hitelesítő adatait, és csoporttagság létrehozása és kezelése az Azure Active Directory.

![Azure Active Directory tartományi szolgáltatások – áttekintés](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso rendszergazdai engedélyezheti az Azure AD-bérlő Azure Active Directory tartományi szolgáltatások és a virtuális hálózat hozzáférhetővé tartományi szolgáltatások. Ezt követően Azure Active Directory tartományi szolgáltatások rendelkezések felügyelt tartományt, és a virtuális hálózaton számára elérhetővé teszi azt. Az összes felhasználói fiókokat, csoporttagság és felhasználói hitelesítő adatokat a Contoso Azure AD-bérlő elérhető is elérhetők az újonnan létrehozott ebben a tartományban. Ez a szolgáltatás lehetővé teszi a felhasználóknak a szervezet a tartományhoz a vállalati hitelesítő adatait – például távolról való csatlakozáskor a tartományhoz gépek távoli asztali keresztül jelentkezzen be. A rendszergazdák is kiépítése erőforrásokhoz az tartományban meglévő csoporttagság használatával. Virtuális gépeken futó a virtuális hálózaton telepített alkalmazások használhatja a szolgáltatásokat, például a tartomány illesztés, LDAP olvasható, LDAP kötés, NTLM és Kerberos-hitelesítés és csoportházirend.

Már kiépítve Azure Active Directory tartományi szolgáltatások által felügyelt tartomány néhány dolgozhat részei a következők:

- Contoso rendszergazdai, nem szükséges kezelése, javítása vagy figyelése ebben a tartományban vagy bármelyik tartomány vezérlők a felügyelt tartományhoz.
- Nincs szükség az Active Directory-replikáció a tartományhoz kezeléséhez. Felhasználói fiókok, csoporttagság és hitelesítő adatait a Contoso Azure AD-bérlő érhetők el automatikusan a felügyelt tartományon belül.
- Mivel az Azure Active Directory tartományi szolgáltatások, a Contoso cég kezeli a tartomány rendszergazdai tartományi rendszergazdája vagy vállalati rendszergazdai jogokkal nem rendelkezik a tartomány.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>A hibrid szervezetek Azure Active Directory tartományi szolgáltatások
Kombinálja a felhőben erőforrásokat a helyszíni és hibrid informatikai infrastruktúrát szervezetek mobilalkalmazásokban Olyan szervezetek szinkronizálni az identitás adatokat a helyszíni címtárból az Azure AD-bérlő. Szerint hibrid szervezetek további áttelepítendő keresse meg a felhőben, különösen régebbi címtár-et használó alkalmazások saját helyszíni alkalmazások Azure Active Directory tartományi szolgáltatások lehet őket.

Bárdos Corporation elkezdte [Azure AD Connect](../active-directory/active-directory-aadconnect.md)szinkronizálni az Azure AD-bérlő a helyszíni címtárból azonosító információkat. Az azonosító adatok szinkronizált magában foglalja a felhasználói fiókok, a saját hitelesítő adatok tiltva (szinkronizálás jelszó) hitelesítési és csoporttagság.

> [AZURE.NOTE] **Jelszó-szinkronizálás kötelező, a hibrid szervezetek Azure Active Directory tartományi szolgáltatások használatára**-e. Ez a követelmény oka az, hogy a felhasználók hitelesítő adatok szükségesek az Azure Active Directory tartományi szolgáltatások, által felügyelt tartomány, ezek a felhasználók NTLM vagy Kerberos hitelesítési módszereket hitelesítést végezni.

![Azure Active Directory tartományi szolgáltatások, a Bárdos Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

A fenti ábrán informatikai infrastruktúrát Bárdos Corporation, például hibrid rendelkező szervezetek használatát Azure Active Directory tartományi szolgáltatások. Bárdos 's alkalmazások és a kiszolgáló munkaterhelésekből tartományi szolgáltatások igénylő virtuális másokkal az Azure infrastruktúrájának szolgáltatásai van telepítve. Bárdos meg rendszergazdai engedélyezheti az Azure AD-bérlő Azure Active Directory tartományi szolgáltatások és a virtuális hálózat hozzáférhetővé felügyelt tartományt. Bárdos értéke az informatikai infrastruktúrát hibrid szervezetével, mivel fiókokkal, csoportok és a hitelesítő adatok Directoryból vannak szinkronizálva az Azure Active Directory-bérlőjére a helyszíni. Ez a szolgáltatás lehetővé teszi a felhasználóknak: Jelentkezzen be a tartomány használata a vállalati hitelesítő adatait – például csatlakozás távoli gépek a tartományhoz távoli asztali keresztül. A rendszergazdák is kiépítése erőforrásokhoz az tartományban meglévő csoporttagság használatával. Virtuális gépeken futó a virtuális hálózaton telepített alkalmazások használhatja a szolgáltatásokat, például a tartomány illesztés, LDAP olvasható, LDAP kötés, NTLM és Kerberos-hitelesítés és csoportházirend.

Már kiépítve Azure Active Directory tartományi szolgáltatások által felügyelt tartomány néhány dolgozhat részei a következők:

- A felügyelt tartománya önálló tartományt. Még nem Bárdos a helyszíni tartomány kiterjesztését.
- Bárdos meg rendszergazdai nem kell kezelni, javítás, vagy figyelheti a tartomány vezérlők a felügyelt tartományhoz.
- Nincs szükség az ebben a tartományban AD a replikáció kezeléséhez. Felhasználói fiókok, csoporttagság és hitelesítő adatok Bárdos a helyszíni címtárból vannak szinkronizálva az Azure Active Directory Azure AD Connect keresztül. A felhasználói fiókok, csoporttagság és hitelesítő adatok érhetők el automatikusan a felügyelt tartományon belül.
- Mivel az Azure Active Directory tartományi szolgáltatások, Bárdos cég kezeli a tartomány rendszergazdai tartományi rendszergazdája vagy vállalati rendszergazdai jogokkal nem rendelkezik a tartomány.


## <a name="benefits"></a>Legfontosabb előny
Azure Active Directory tartományi szolgáltatások elérhesse a következő előnyökkel jár:

-   **Egyszerű** – is felel meg a virtuális gépeken futó Azure infrastruktúrájának szolgáltatásai, néhány egyszerű kattintással rendszerbe állított identitást szükségleteinek. Nem kell üzembe helyezéséhez és kezeléséhez identitás infrastruktúra a vissza a helyszíni identitás infrastruktúra az Azure vagy beállítási kapcsolat.

-   **Integrált** – Azure Active Directory tartományi szolgáltatások, a Azure AD-bérlő szorosan integrálódik. Azure AD-integrált felhőalapú címtárból, amely a alkalmazásokat modern és a hagyományos címtár-et használó alkalmazások igényeihez caters most használni.

-   **Kompatibilis** – Azure Active Directory tartományi szolgáltatások épül a Windows Server az Active Directory igazolt vállalati minőségű infrastruktúra. Az alkalmazások emiatt a Windows Server Active Directory-funkciókat tartalmazó kompatibilitási nagyobb fokú számíthat. Nem az összes elérhető a Windows Server AD szolgáltatások jelenleg elérhető az Azure Active Directory tartományi szolgáltatások. Azonban elérhető szolgáltatások kompatibilisek a Windows Server AD a helyszíni infrastruktúra támaszkodhat megfelelő funkcióival. LDAP, Kerberos, NTLM, csoportházirend vagy tartomány Bekapcsolódás lehetőségeit elért ajánlja fel, tesztelése és különböző Windows Server-verziókban fölé Iskolás alkotnak.

-   **Költséghatékony** – az Azure Active Directory tartományi szolgáltatások, elkerülése érdekében a infrastruktúra és kezelés terhet társított identitás infrastruktúrát támogatja a hagyományos címtár-et használó alkalmazások kezelése. Helyezze át ezeket az alkalmazásokat Azure infrastruktúrájának szolgáltatásai, és műveleti költségekről nagyobb megtakarítási összekapcsolhatók.
