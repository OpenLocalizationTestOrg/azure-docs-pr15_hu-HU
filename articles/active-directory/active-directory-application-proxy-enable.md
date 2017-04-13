<properties
    pageTitle="Azure AD-alkalmazás Proxy engedélyezése |} Microsoft Azure"
    description="Az Azure klasszikus portálon szolgáltatásalkalmazás-Proxy be- és az, fordított proxykiszolgáló az összekötő telepítése."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Szolgáltatásalkalmazás-Proxy engedélyezése az Azure-portálon

Ez a cikk végigvezeti a Microsoft Azure Active Directory szolgáltatásalkalmazás-Proxy a felhőben címtárban ahhoz, hogy az Azure Active Directory.

Ha nem tudja pontosan, milyen alkalmazás proxyval segíthetnek a végezze el, [hogyan biztonságos távoli hozzáférés nyújtása a helyszíni alkalmazások](active-directory-application-proxy-get-started.md)olvashat.

## <a name="application-proxy-prerequisites"></a>Alkalmazás Proxy vonatkozó követelmények
Mielőtt engedélyezése, és szolgáltatásalkalmazás-Proxy szolgáltatások használatára, van szükség:

- [Egyszerű vagy prémium előfizetés a Microsoft Azure Active Directory](active-directory-editions.md) és Azure Active Directory címtárhoz, amelynek Ön globális rendszergazda.
- A Windows Server 2012 R2 vagy Windows 8.1-es vagy újabb rendszert futtató kiszolgáló, amelyen is telepítheti az alkalmazást a proxykiszolgáló összekötő. A kiszolgáló kérést küld a szolgáltatásalkalmazás-Proxy szolgáltatások a felhőben, és szüksége van egy HTTP vagy HTTPS kapcsolat webkiszolgálón teszi közzé alkalmazásokat.

    - Az egyszeri bejelentkezési a közzétett alkalmazásokra ez a gép kell kell tartományhoz webkiszolgálón teszi közzé alkalmazásokat azonos AD a tartományban.

- Ha az elérési utat a tűzfalat, győződjön meg arról, hogy meg nyitva, hogy az összekötő HTTPS (TCP) kérések végezhet a szolgáltatásalkalmazás-Proxy. Az összekötő portokhoz használ, a magas szintű tartományokat msappproxy.net és servicebus.windows.net részét képező altartományokat együtt. Győződjön meg róla, hogy nyissa meg a következő portokat és a **kimenő** forgalom:

  	| Port száma | Leírás |
  	| --- | --- |
  	| 80 | Engedélyezze a kimenő HTTP-forgalmat biztonsági adatérvényesítéshez. |
  	| 443 | Azure AD (csak az összekötő regisztrációs folyamat szükséges) szemben a felhasználói hitelesítés engedélyezése |
  	| 10100 – 10120 | Vissza a proxyhoz küldött LOB HTTP válaszok engedélyezése |
  	| 9352, 5671 | Engedélyezze az összekötő felé bejövő felkérést az Azure szolgáltatás közötti kommunikációt. |
  	| 9350 | Nem kötelező, bejövő felkérést jobb teljesítményt engedélyezése |
  	| 8080 | Az összekötő betöltő sorrend és az összekötő automatikus frissítés engedélyezése |
  	| 9090 | Engedélyezze az összekötő regisztrációs (csak az összekötő regisztrációs folyamat szükséges) |
  	| 9091 | Összekötők megbízhatósági tanúsítványt az automatikus megújítás engedélyezése |

    A tűzfal kényszeríti forgalmának megfelelően felhasználók származó, nyissa meg a portokhoz, a hálózati szolgáltatás futtatott Windows-szolgáltatások érkező forgalmat. Győződjön meg róla, hogy port 8080 engedélyezése NT Authority\System.

- Ha szervezete használja a proxykiszolgálók csatlakozhat az internethez, kérjük, nézze meg a blogbejegyzés [meglévő helyszíni proxykiszolgálók használata](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) részletekért hogyan kell beállítani őket.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Lépés: 1: Szolgáltatásalkalmazás-Proxy engedélyezése az Azure Active Directory
1. Jelentkezzen be rendszergazdaként az [Azure klasszikus portálon](https://manage.windowsazure.com/).
2. Nyissa meg az Active Directory, és jelölje ki a könyvtárat, amelyben a szolgáltatásalkalmazás-Proxy engedélyezni szeretné.

    ![Az Active Directory - ikon](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Jelölje be a **Configure** címtár lapra, és görgessen le a **Szolgáltatásalkalmazás-Proxy**.
4. Váltás **engedélyezze ezt a címtárat Proxy alkalmazásszolgáltatások** **engedélyezve van**.

    ![Szolgáltatásalkalmazás-Proxy engedélyezése](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Jelölje ki **a Letöltés gombra**. Ekkor megjelenik az **Azure Active Directory alkalmazás Proxy összekötő letöltése**. Olvassa el és fogadja el a licencszerződést, és **Töltse le** a Windows Installer fájl (.exe) az összekötő mentéséhez kattintson.

## <a name="step-2-install-and-register-the-connector"></a>Lépés: 2: Telepítése, és regisztráljon az összekötő
1. **AADApplicationProxyConnectorInstaller.exe** meg megfelelően a Előfeltételek készített kiszolgálón futó.
2. Kövesse az utasításokat követve telepítse a varázslóban.
3. A telepítés során a rendszer rákérdez, hogy az összekötő regisztrálhatja a az Azure AD-bérlő alkalmazás proxyval.

  - Adja meg az Azure Active Directory globális rendszergazdai hitelesítő adataival. A globális rendszergazda bérlői eltérhetnek a Microsoft Azure hitelesítő adatait.
  - Ellenőrizze, hogy a rendszergazda, aki regisztrál az összekötő ugyanabban a mappában Ha engedélyezte a szolgáltatásalkalmazás-Proxy szolgáltatást. Például: contoso.com bérlői tartomány esetén a rendszergazda kell admin@contoso.com vagy bármely más alias meg azt a tartományt.
  - Ha **Internet Explorer fokozott biztonság beállításai** van beállítva **a** a kiszolgálón Ha telepíti az összekötő, a regisztrációs képernyője blokkolhatja. Kövesse a hozzáférés engedélyezése a hibaüzenetet. Győződjön meg arról, hogy ki van kapcsolva, hogy az Internet Explorer fokozott biztonság.
  - Ha nem sikerül összekötő regisztrációs, olvassa el a [Szolgáltatásalkalmazás-Proxy kapcsolatos hibák elhárítása](active-directory-application-proxy-troubleshoot.md).  

4. A telepítés befejezésekor két új szolgáltatások kerülnek a kiszolgáló:

    - **A Microsoft AAD alkalmazás Proxy Connector** lehetővé teszi, hogy a kapcsolat
    - **A Microsoft AAD alkalmazás Proxy összekötő Updater** egy automatikus frissítése szolgáltatása, amelyeket rendszeresen ellenőrzi, hogy az összekötő új verziója, és frissíti az összekötőt, szükség szerint.

    ![Alkalmazás Proxy összekötő szolgáltatás – kép](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. A telepítés ablakban a **Befejezés** gombra.

Magas elérhetősége céljából telepítenie kell legalább két összekötők. További összekötők üzembe helyezéséhez ismételje 2 és 3 felett. Mindegyik összekötő regisztrálva külön-külön kell lennie.

Ha azt szeretné, az összekötő eltávolítása, távolítsa el a csatlakozás és a Updater szolgáltatást is. Indítsa újra a számítógépet a szolgáltatás teljes körű eltávolítása.


## <a name="next-steps"></a>Következő lépések

Most már készen áll [a szolgáltatásalkalmazás-Proxy közzététel](active-directory-application-proxy-publish.md)alkalmazások.

Ha alkalmazást, ami külön hálózatok vagy más helyre, a különböző összekötők rendszerezéséhez logikai egységek összekötő csoportok is használhatja. További tudnivalók a [munka szolgáltatásalkalmazás-Proxy összekötőkkel](active-directory-application-proxy-connectors.md).
