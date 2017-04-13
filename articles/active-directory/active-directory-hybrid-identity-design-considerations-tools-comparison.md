<properties
    pageTitle="Hibrid identitás: Címtár-integrációs eszközök összehasonlító |} Microsoft Azure"
    description="Ez az oldal ismerteti, hogy miben más a különböző címtár-integrációs eszközök címtár-integrációs használható teljes táblázat."
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
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hibrid identitás címtár-integrációs eszközök összehasonlítása

Az évek során a címtár-integrációs eszközök termesztett és evolved.  A dokumentum, adja meg, ezeket az eszközöket összevont nézetét, és a minden elérhető szolgáltatásainak összehasonlítása érdekében.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect megfelelő a összetevők és a korábban már megjelent Dirsync és AAD Sync funkciók. Ezek az eszközök már nem egyenként kiadott, és minden jövőbeni fejlesztések fog szerepelni Azure AD Connect, frissítések, hogy mindig képben hol találja meg a legújabb funkciókhoz.
>
>DirSync és Azure AD-szinkronizálás használata ajánlott. További információt az [Itt](active-directory-aadconnect-dirsync-deprecated.md)található.


Használja a következő kulcsot a tábla minden egyes.

● = most már elérhető  
FR = az elkövetkező kiadásokban  
PP = nyilvános előzetes verzió  

## <a name="on-premises-to-cloud-synchronization"></a>A helyszíni szinkronizálás a felhőbe

| A szolgáltatás  | Csatlakozás Azure Active Directory  | Azure Active Directory-szinkronizálási szolgáltatások (AAD Sync) | Azure Active Directory-szinkronizáló (DirSync)| A Forefront identitáskezelő 2010 R2 (FIM) |A Microsoft Identitáskezelő 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Csatlakozás egy helyszíni Active Directory erdő | ● | ● | ● | ● |● |
| Csatlakozás több helyszíni Active Directory erdők |●  | ● |  | ● |● |
| Csatlakozás több a helyszíni Exchange-szervezetek | ● |  |  |  | |
| Csatlakozás helyszíni egyetlen LDAP-címtár | FR |  |  | ● |● |
| Csatlakozás több helyszíni LDAP címtárakban |FR  |  |  | ● |● |
| Csatlakozás helyszíni Active Directory és a helyszíni LDAP címtárakban |FR  |  |  | ● |● |
| Csatlakozás egyéni rendszerek (azaz SQL, az Oracle, MySQL stb.) | FR |  |  | ● |● |
| Szinkronizálja a felhasználó által definiált attribútumok (címtár extensions) | ● |  |  |  |  |
|Csatlakozás helyszíni HR (tehát SAP, Oracle gazdaságot, PeopleSoft)| FR |  |  | ● |● |
|A helyszíni rendszerek kiépítési FIM szinkronizálási szabályok és összekötők támogatja.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>A helyszíni szinkronizálás a felhőbe

| A szolgáltatás  | Csatlakozás Azure Active Directory  | Azure Active Directory-szinkronizálási szolgáltatások | Azure Active Directory-szinkronizáló (DirSync) | A Forefront identitáskezelő 2010 R2 (FIM) |A Microsoft Identitáskezelő 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Az eszközök visszaírást | ● |  | ● |  ||
| Attribútum visszaírást (a hibrid Exchange-telepítés) | ● | ● | ● | ● |● |
| A felhasználók és csoportok objektumok visszaírást |  ●|  | |  ||
| A jelszavak (az önkiszolgáló jelszó-visszaállítás és a jelszó módosítása) visszaírást |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Hitelesítés által támogatott funkciók

| A szolgáltatás  | Azure Active Directory csatlakoztatása | Azure Active Directory-szinkronizálási szolgáltatások | Azure Active Directory-szinkronizáló (DirSync) | A Forefront identitáskezelő 2010 R2 (FIM) |A Microsoft Identitáskezelő 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Egyetlen jelszó-szinkronizálás a helyszíni Active Directory erdő | ● | ● | ● |  ||
| Több jelszó-szinkronizálás a helyszíni Active Directory erdők |  ●| ● |  |  ||
| Egyszeri bejelentkezés a összevonási szolgáltatása | ● | ● | ● | ● |● |
| A jelszavak (a SSPR és a jelszó módosítása) visszaírást |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Beállítási és telepítési

| A szolgáltatás  | Csatlakozás Azure Active Directory  | Azure Active Directory-szinkronizálási szolgáltatások | Azure Active Directory-szinkronizáló (DirSync) | A Microsoft Identitáskezelő 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Telepítési támogatja a tartományvezérlőnek a | ● | ● | ● |  |
| Az SQL-Expresst használja telepítési támogatja | ● | ● | ● |  |
| A DirSync egyszerűen frissítés |● |  |  |  |
| A Windows Server nyelvekre a felügyeleti UX honosítási | ● | ● | ● |  |
|A Windows Server nyelvekre UX végfelhasználói honosítási| |  |  |● |
| Windows Server 2008 és Windows Server 2008 R2 támogatása | ● a szinkronizálás során nem összevonáshoz| ● | ●  | ● |
| A Windows Server 2012 és a Windows Server 2012 R2 támogatása | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Szűrés és konfiguráció

A szolgáltatás  | Csatlakozás Azure Active Directory | Azure Active Directory-szinkronizálási szolgáltatások | Azure Active Directory-szinkronizáló (DirSync) | A Forefront identitáskezelő 2010 R2 (FIM)| A Microsoft Identitáskezelő 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
A tartományok és szervezeti egység szűrése | ● | ● | ● | ●  | ●
Objektumok attribútum értékek szűrése | ● | ● | ● | ●| ●
Engedélyezni kell attribútumok minimális számú szinkronizált (MinSync) | ● | ● |  ||
Másik szolgáltatáscsaládba sablonok attribútum flow alkalmazandó engedélyezése |●  | ● |  ||
Az Active Directory Azure Active Directory halad attribútumok eltávolítása engedélyezése | ● | ● |  |  |
Lehetővé teszi speciális attribútum flow testreszabása | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitások Azure Active Directoryval való integrálásának](active-directory-aadconnect.md).
