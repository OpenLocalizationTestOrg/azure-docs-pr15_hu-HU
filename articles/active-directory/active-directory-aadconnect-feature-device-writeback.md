<properties
    pageTitle="Azure AD Connect: Segítségével, így az eszköz visszaírást |} Microsoft Azure"
    description="A dokumentum adatai Azure AD Connect használatával eszköz visszaírást engedélyezése"
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
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Eszköz visszaírást engedélyezése

>[AZURE.NOTE] Azure Active Directory prémium verzióra való előfizetés szükség az eszköz visszaírást.

A következő dokumentáció ismerteti ahhoz, hogy az eszköz visszaírást Azure AD Connect szolgáltatás. Eszköz visszaírást alapul, az alábbi esetekben:

- ADFS eszközök alapuló feltételes hozzáférés engedélyezése (2012 R2 vagy újabb) védett alkalmazások (megbízó fél meghatalmazások).

Ez nyújt további biztonsági és garantálja, hogy az alkalmazások hozzáférés csak a megbízható eszközök. Feltételes hozzáféréssel kapcsolatos további tudnivalókért lásd: [Feltételes hozzáférés kezelése a kockázatok](active-directory-conditional-access.md) és [feltételes elérésének helyszíni Azure Active Directory eszköz regisztrációs beállítását](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Eszközök ugyanabban a tartományban, mint a felhasználók kell lennie. Eszközök erdőn vissza kell írni, mivel ez a funkció jelenleg nem támogatja a környezet, amelyben a több felhasználó erdők.</li>
<li>A helyszíni Active Directory-erdő csak egy eszköz regisztrációs konfigurációs objektum bővíthető. Ez a funkció nem kompatibilis a topológia a helyszíni Active Directory több Azure AD-könyvtárak szinkronizált hol.</li>

## <a name="part-1-install-azure-ad-connect"></a>Rész 1: Azure Active Directory telepítése kapcsolódni
1. Telepítse az Azure AD Connect használatával egyéni, vagy Express beállítások. A Microsoft összes felhasználók és csoportok sikeres szinkronizálásának eszköz visszaírást engedélyezése előtt induljon javasolja.

## <a name="part-2-prepare-active-directory"></a>2 rész: Az Active Directory előkészítése
Felkészülés az eszköz visszaírást használatával kövesse az alábbi lépéseket.

1.  A számítógépről, amelyen telepítve van-e az Azure AD Connect emelt módban indítsa el PowerShell.

2.  Ha nincs telepítve a Active Directory PowerShell-modult, telepítse a következő parancsot:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Ha nincs telepítve a Powershellhez Azure Active Directory modul, majd töltse le, és telepítse az [Azure Active Directory modul a Windows Powershellhez (64 bites verzió)](http://go.microsoft.com/fwlink/p/?linkid=236297). Ez az összetevő függőség a – bejelentkezési segéd, amelyen telepítve van az Azure AD Connect van.

4.  Vállalati rendszergazdai hitelesítő adatokkal a következő parancsokat, és zárja be a PowerShell.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Vállalati rendszergazdai hitelesítő adatok szükségesek, mivel a konfigurációs névtér módosításai van szükség. A domain admin nem lesz a megfelelő engedélyekkel.

![PowerShell használata az eszközön visszaírást engedélyezése](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Leírás:

- Ha már nem léteznek, hoz létre, és konfigurálja az új tárolók és objektumok CN = eszköz regisztrációs Configuration, CN = Services, CN = Configuration, [erdő-dn].
- Ha már nem léteznek, hoz létre, és konfigurálja az új tárolók és objektumok CN = RegisteredDevices, [tartomány-dn]. Eszköz objektumok létrejön a tárolóban.
- Az Azure Active Directory-összekötő ügyfél, az Active Directory-eszközök kezeléséhez szükséges engedélyeket állít be.
- Csak futtatásához szükséges rendszerkövetelmények egy erdőkben, még akkor is, ha több erdőket Azure AD Connect telepíti.

Paraméterek:

- Tartománynév: Az Active Directory tartomány eszköz objektumok létrehozásának helyét. Megjegyzés: Az adott Active Directory-erdő összes eszközön is létrejön egy tartományban.
- AdConnectorAccount: Active Directory-fiókját a címtár-objektumok kezelése Azure AD Connect által használt. Az Active Directory csatlakozhat az Azure AD Connect szinkronizálás használt fiók. Express beállításokkal telepítette, érdemes a fiókot, ha MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>3 rész: Engedélyezése eszköz visszaírást az Azure AD Connect
Az alábbi eljárással ahhoz, hogy az Azure AD Connect eszköz visszaírást.

1.  Futtassa újra a telepítés varázslót. Jelölje ki a **szinkronizálási beállítások testreszabása** a további tevékenységek lapról, és kattintson a **Tovább**gombra.
![Egyéni telepítési szinkronizálási beállításainak testreszabása](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  A választható funkció lapon eszköz visszaírást lesz már nem kell szürkén jelenik meg. Kérjük, hogy ha az Azure AD Connect előkészületre lépéseket nincsenek bejegyzett eszköz visszaírást Megjegyzés fog szürkítve jelennek kicsinyítés a választható szolgáltatások lapon. Jelölje be a jelölőnégyzetet, az eszköz visszaírást, és kattintson a **Tovább**gombra. Ha a jelölőnégyzet továbbra is le van tiltva, nézze meg a [hibaelhárítási szakaszában](#the-writeback-checkbox-is-still-disabled).
![Egyéni eszköz visszaírást választható funkció telepítése](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  A visszaírást lapon, az alapértelmezett eszköz visszaírást erdő jelenik meg a megadott tartomány.
![Egyéni telepítés eszköz visszaírást célerdő](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  A varázsló további beállítási változtatás nélkül a telepítés befejezéséhez. Ha szükséges, [Azure AD Connect az egyéni telepítési.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Feltételes elérésének engedélyezése
Ahhoz, hogy ebben az esetben részletes útmutatást [a helyszíni feltételes elérésének Azure Active Directory eszköz regisztrációs beállításának](https://msdn.microsoft.com/library/azure/dn788908.aspx)belül érhetők el.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Ellenőrizze az eszköz a rendszer szinkronizálja az Active Directory
Célszerű az eszköz visszaírást most megfelelően működik. Ne feledje, hogy az eszköz objektumok írt vissza ad legfeljebb 3 óráig is eltarthat.  Ha ellenőrizni szeretné, hogy az eszközöket megfelelően a rendszer alatt szinkronizálja, tegye a következőket a szinkronizálási szabályok befejezése után:

1.  Indítsa el az Active Directory felügyeleti központ.
2.  Bontsa ki a RegisteredDevices identitásszolgáltatóval összevont alatt tartományhoz belül.
![Az Active Directory felügyeleti központ bejegyzett eszközök](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Aktuális bejegyzett eszközök nem lesz látható.
![Az Active Directory felügyeleti központ bejegyzett eszközök listája](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="the-writeback-checkbox-is-still-disabled"></a>A visszaírást jelölőnégyzet továbbra is le van tiltva.
Ha az eszköz visszaírást a jelölőnégyzet nincs engedélyezve, akkor is, ha a fenti lépések követte, az alábbi lépésekkel végigvezeti Önt Mi az telepítővarázslóban ellenőrzése előtt a be van jelölve.

Első dolog, amit első:

- Ellenőrizze, hogy legalább egy erdőnek van Windows Server 2012R2. Az eszköz objektumtípus szerepelnie kell.
- Ha már fut a telepítés varázslót, majd módosításokat észlelése nem történik. Ebben az esetben a telepítés varázsló, és futtassa újra.
- Ellenőrizze, hogy a fiók, adja meg, ha a inicializálni parancsfájl ténylegesen a helyes-e az Active Directory Connector által használt felhasználói. Ennek ellenőrzéséhez kövesse az alábbi lépéseket:
    - Nyissa meg a start menü **Szinkronizálási szolgáltatás**.
    - Nyissa meg az **összekötők** fülre.
    - Keresse meg az összekötő típusú Active Directory tartományi szolgáltatások, és jelölje ki.
    - A **Műveletek**csoportban válassza a **Tulajdonságok parancsot**.
    - Nyissa meg az **Active Directory-erdőben csatlakozni**. Győződjön meg arról, hogy a tartomány és a felhasználó neve megadott a képernyő megfelelés a fiókot, a parancsfájl rendelkezésére.
![Összekötő-fiók szinkronizálása Service Manager](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Az Active Directory konfigurációjának ellenőrzése:
- Ellenőrizze, hogy az eszköz regisztrációs szolgáltatás az alábbi helyen található (CN DeviceRegistrationService, CN = eszköz regisztrációs Services, CN = eszköz regisztrációs Configuration, CN = Services, CN = = Configuration) konfigurációs elnevezési környezetében.

![A hiba elhárításához a konfigurációs névtér DeviceRegistrationService](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Ellenőrizze, hogy csak egy konfigurációs objektum van a konfigurációs névtér kereséssel. Ha egynél több, az Ismétlődés törlése

![Hibaelhárítás, az ismétlődő objektumok keresése](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Az eszköz regisztrációs szolgáltatása objektumra ellenőrizze, hogy, az attribútum msDS DeviceLocation nem tartalmaz adatokat, és egy értéket tartalmazza. Keresés a helyet, és győződjön meg arról, hogy megtalálható az objektumtípus msDS DeviceContainer együtt.

![A hiba elhárításához msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![A hiba elhárításához RegisteredDevices objektum osztály](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Ellenőrizze, az Active Directory Connector által használt fiók szükséges engedélyekkel rendelkezik-e meg a bejegyzett eszközök tároló az előző lépésben által talált. Ez a tároló várható engedélyeinek:

![Hibaelhárítás, tároló engedélyeinek ellenőrzése](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Ellenőrizze az Active Directory-fiókját rendelkezik engedélyekkel a CN eszköz regisztrációs Configuration, CN = Services, CN = = konfigurációs objektumot.

![Hibaelhárítás, eszköz regisztrációs konfigurálásról engedélyek ellenőrzése](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>További információk
- [A kockázatok feltételes hozzáférés kezelése](active-directory-conditional-access.md)
- [A helyszíni feltételes elérésének Azure Active Directory eszköz regisztrációs beállítása](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
