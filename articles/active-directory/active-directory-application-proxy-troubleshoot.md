<properties
    pageTitle="Szolgáltatásalkalmazás-Proxy elhárítása |} Microsoft Azure"
    description="Az Azure Active Directory szolgáltatásalkalmazás-Proxy hibák foglalkozik."
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
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Szolgáltatásalkalmazás-Proxy – problémamegoldás

Ha hiba történik a közzétett alkalmazás elérése vagy közzétételi alkalmazásban, jelölje be az alábbi lehetőségek megjelenítéséhez, ha a Microsoft Azure Active Directory szolgáltatásalkalmazás-Proxy megfelelően működik-e:

- Nyissa meg a Windows-szolgáltatások konzolt, és ellenőrizze, hogy engedélyezve van-e a **Microsoft AAD alkalmazás Proxy összekötő** szolgáltatás és futtatása. Is érdemes szolgáltatásalkalmazás-Proxy szolgáltatás tulajdonságai lapon tekintse meg az alábbi képen látható módon:  
  ![A Microsoft AAD alkalmazás Proxy összekötő tulajdonságai ablak képernyőképe](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Nyissa meg az Eseménynapló nevet, és keresse meg a szolgáltatásalkalmazás-Proxy összekötő események az **alkalmazások és szolgáltatásnaplók** > **Microsoft** > **AadApplicationProxy** > **összekötő** > **rendszergazda**.
- Ha szükséges, részletesebb naplók érhetők el elemző naplók hibakeresése során, és a szolgáltatásalkalmazás-Proxy összekötő munkamenet napló bekapcsolása bekapcsolásával.

## <a name="the-page-is-not-rendered-correctly"></a>A lapon nem jelenik meg megfelelően

Ha nem hallatszik egy adott hibaüzenetet, előfordulhat, hogy továbbra is problémák merültek leképezési, vagy nem megfelelően működik-e az alkalmazással. Ez akkor fordulhat elő, ha a cikk elérési közzétett, de az alkalmazás igényel, hogy az elérési utat kívüli tartalom.

Például ha teszi közzé, hogy az elérési út https://yourapp/app, de az alkalmazás https://yourapp/media felhívja a képeket, hogy nem jeleníthető meg. Győződjön meg arról, hogy az alkalmazást a legmagasabb szintű elérési összes kapcsolódó tartalmat is használni kell közzé. Ebben a példában http://yourapp/ lenne.

Ha módosítja a hivatkozott tartalom, de továbbra is a felhasználóknak, hogy az elérési út mélyebb hivatkozásra land szükséges elérési útja, tekintse meg az közzé [a megfelelő hivatkozásra az Azure Active Directory access panel és az Office 365-alkalmazásindító szolgáltatásalkalmazás-Proxy alkalmazások beállítása](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Általános hibák

Hibaüzenet | Leírás | Megoldás
--- | --- | ---
Vállalati alkalmazás nem érhető el. Nincs engedélye ennek az alkalmazásnak eléréséhez. Nem sikerült engedélyezése. Győződjön meg arról, hogy a felhasználó, aki hozzáfér hozzárendelése az alkalmazás. | Előfordulhat, hogy nem rendelt ehhez az alkalmazáshoz a felhasználó. | Nyissa meg az **alkalmazás** lapon, és a **felhasználók és csoportok**, a felhasználó vagy felhasználócsoport hozzárendelése az alkalmazást.
Vállalati alkalmazás nem érhető el. Nincs engedélye ennek az alkalmazásnak eléréséhez. Nem sikerült engedélyezése. Győződjön meg arról, hogy a felhasználónak licencet az Azure Active Directory Premium vagy Basic. | A felhasználók előfordulhat, hogy a következő hibaüzenet meg az alkalmazást, akkor közzé, ha az azok nem kifejezetten prémium/Basic licenccel rendelkező rendszergazda által megadott az előfizetői hozzáférés. | Nyissa meg az előfizetői az Active Directory- **licencek** lapot, és győződjön meg arról, hogy a felhasználó vagy felhasználócsoport van rendelve egy Premium vagy az egyszerű licenccel.


## <a name="connector-troubleshooting"></a>Connector hibaelhárítása
Ha a regisztrációs varázsló összekötő telepítése során nem sikerül, kétféleképpen megtekintése a hiba okát. Nézze meg az Eseménynapló az **alkalmazások és szolgáltatások Logs\Microsoft\AadApplicationProxy\Connector\Admin**, vagy futtassa az alábbi Windows PowerShell-parancsot.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Hibaüzenet | Leírás | Megoldás |
| --- | --- | --- |
| Nem sikerült összekötő regisztráció: Ellenőrizze, hogy engedélyezve van a szolgáltatásalkalmazás-Proxy az adatkezelési portálon Azure és a megadott megfelelően az Active Directory-felhasználónevét és jelszavát. Hiba: "egy vagy több hiba történt." | A regisztráció ablak zárni, jelentkezzen be az Azure Active Directory végrehajtása nélkül. | Futtassa újra az összekötő varázslót, és regisztráljon az összekötőt. |
| Nem sikerült összekötő regisztráció: Ellenőrizze, hogy engedélyezve van a szolgáltatásalkalmazás-Proxy az adatkezelési portálon Azure és a megadott megfelelően az Active Directory-felhasználónevét és jelszavát. Hiba: "AADSTS50001: az erőforrás `https://proxy.cloudwebappproxy.net /registerapp` le van tiltva." | Szolgáltatásalkalmazás-Proxy le van tiltva.  | Ellenőrizze, hogy a szolgáltatásalkalmazás-Proxy engedélyezése előtt történő regisztrálásakor az összekötő az Azure klasszikus portálon. Szolgáltatásalkalmazás-Proxy engedélyezésével kapcsolatos további tudnivalókért olvassa el a [szolgáltatások szolgáltatásalkalmazás-Proxy engedélyezése](active-directory-application-proxy-enable.md)című témakört. |
| Nem sikerült összekötő regisztráció: Ellenőrizze, hogy engedélyezve van a szolgáltatásalkalmazás-Proxy az adatkezelési portálon Azure és a megadott megfelelően az Active Directory-felhasználónevét és jelszavát. Hiba: "egy vagy több hiba történt." | Ha a regisztrációs ablak megnyílik, és azonnal bezárul, nélkül próbál bejelentkezni, akkor valószínűleg kapja ezt a hibát. A hiba oka egy hálózati hiba esetén a rendszer. | Győződjön meg arról, hogy az lehetséges csatlakozhat egy böngészőből a nyilvános webhelyet, és, hogy a nyitva legyen a [szolgáltatásalkalmazás-Proxy Előfeltételek](active-directory-application-proxy-enable.md)meghatározott. |
| Nem sikerült összekötő regisztráció: Győződjön meg arról, hogy a számítógép csatlakozik az internethez. Hiba: "nem figyel végpont volt `https://connector.msappproxy.net :9090/register/RegisterConnector` , amely sikerült fogadja el az üzenetet. Ez gyakran nem a megfelelő vagy SOAP művelet. Ha jelen van, további részleteket lásd: a InnerException,. " | Ha a Azure AD-felhasználónevével és jelszavával jelentkezzen, de ezután a következő hibaüzenet jelenik, lehet, hogy az összes portok feletti 8081 blokkolt. | Győződjön meg arról, hogy a szükség legyen nyitva. További tudnivalókért olvassa el a [szolgáltatásalkalmazás-Proxy Előfeltételek](active-directory-application-proxy-enable.md)című témakört. |
| A regisztráció ablak törlése hiba jelenik meg. Nem folytatható – csak a zárja be az ablakot. | A megfelelő felhasználónevet és jelszót adott meg. | próbáld újra. |
| Nem sikerült összekötő regisztráció: Ellenőrizze, hogy engedélyezve van a szolgáltatásalkalmazás-Proxy az adatkezelési portálon Azure és a megadott megfelelően az Active Directory-felhasználónevét és jelszavát. Hiba: "AADSTS50059: bérlői azonosító információ nem található vagy a kérelmet, vagy implicit által megadott hitelesítő adatokat, és a keresési szolgáltatás elve URI által sikertelen volt. | Jelentkezzen be Microsoft-Account és nem a szervezet azonosítója, a könyvtár elérni kívánt részét képező tartomány próbál. | Győződjön meg arról, hogy a rendszergazda tartomány neve megegyezik a bérlői tartomány része, például ha az Azure Active Directory tartományi contoso.com, a rendszergazdának kell lennie admin@contoso.com. |
| Nem sikerült beolvasni a futó PowerShell-parancsfájlokat az aktuális végrehajtási házirend. | Ha az összekötő telepítése sikertelen, ellenőrizze, hogy győződjön meg arról, hogy nincsenek letiltva a PowerShell végrehajtási házirend. | Nyissa meg a csoportházirend-szerkesztőben. Nyissa meg a **Számítógép konfigurációja** > **Felügyeleti sablonok** > **Windows-összetevők** > **A Windows PowerShell** kattintson duplán a **bekapcsolása parancsfájl végrehajtása**. E beállítás **Nincs beállítva** vagy **engedélyezve**. Ha **engedélyezi**a beállítást, győződjön meg arról, hogy a beállítások csoportban az adatvégrehajtás-házirend értéke vagy **Engedélyezés helyi és távoli aláírt parancsfájlok** vagy **az összes parancsfájl engedélyezése**. |
| Összekötő nem sikerült letölteni a konfiguráción. | Az összekötő ügyfél-tanúsítvány, amely hitelesítéshez használt lejárt. Ez akkor is előfordulhat, ha az összekötőt a proxy mögött van telepítve van. Az összekötő ebben az esetben nem tud hozzáférni az interneten, és nem tudják az alkalmazások távoli felhasználók számára. | Adatvédelmi manuálisan megújítani az `Register-AppProxyConnector` a Windows PowerShell parancsmaggal. Ha az összekötő rendszere proxy mögött működik, szükség adja meg az Internet-hozzáféréssel a Connector-fiókok "hálózati szolgáltatások" és "helyi rendszer." Ez lehet elvégezni megadása az access a Proxy vagy beállításával, megkerüli a proxykiszolgálót. |
| Nem sikerült összekötő regisztráció: Ellenőrizze, hogy a globális rendszergazdája, az Active Directoryval, hogy regisztráljon az összekötő. Hiba: "a regisztrálási kérés el lett utasítva." | Az alias szeretné, hogy jelentkezzen be a rendszergazda a tartomány nem. Az összekötő mindig telepítve van a könyvtár, amely a felhasználó tartomány tulajdonosa. | Győződjön meg arról, hogy a rendszergazda próbál bejelentkezni, a Azure AD-bérlő globális engedélyekkel rendelkezik.|


## <a name="kerberos-errors"></a>Kerberos-hibák

| Hibaüzenet | Leírás | Megoldás |
| --- | --- | --- |
| Nem sikerült beolvasni a futó PowerShell-parancsfájlokat az aktuális végrehajtási házirend. | Ha az összekötő telepítése sikertelen, ellenőrizze, hogy győződjön meg arról, hogy nincsenek letiltva a PowerShell végrehajtási házirend. | Nyissa meg a csoportházirend-szerkesztőben. Nyissa meg a **Számítógép konfigurációja** > **Felügyeleti sablonok** > **Windows-összetevők** > **A Windows PowerShell** kattintson duplán a **kapcsolja be a parancsfájl végrehajtása**. E beállítás **Nincs beállítva** vagy **engedélyezve**. Ha **engedélyezi**a beállítást, győződjön meg arról, hogy a beállítások csoportban az adatvégrehajtás-házirend értéke vagy **Engedélyezés helyi és távoli aláírt parancsfájlok** vagy **az összes parancsfájl engedélyezése**. |
| 12008 - azure Active Directory meghaladja a megengedett Kerberos hitelesítési kísérletek kódmentes kiszolgálóra maximális száma. | Ebben az esetben jelezheti helytelen konfigurációja közötti Azure Active Directory és a kódmentes application server, vagy a dátum és időpont konfigurációban mindkét gépen probléma. | A kódmentes server Azure Active Directory által létrehozott Kerberos-jegy elutasítva. Ellenőrizze, hogy Azure Active Directory és a kódmentes alkalmazáskiszolgáló megfelelően van-e beállítva. Győződjön meg arról, hogy a dátum és időpont konfigurációjának az Azure Active Directory és a kódmentes application server vannak szinkronizálva. |
| 13016 - azure AD nem lehet beolvasni a felhasználó nevében Kerberos-jegy, mivel nincsenek (UPN), az access cookie-ban vagy a biztonsági jogkivonat. | A STS konfigurációs probléma van. | Az egyszerű Felhasználónévi igények konfigurálása javítsa a STS. |
| 13019 - azure AD az alábbi általános API-hiba miatt nem lehet beolvasni a Kerberos-jegy a felhasználó nevében. | Ez az esemény jelezheti helytelen konfigurációja közötti Azure Active Directory és a tartomány vezérlő kiszolgálón vagy a dátum és időpont konfigurációs mindkét gépen probléma. | A tartományvezérlőnek a Kerberos jegy Azure Active Directory által létrehozott elutasítva. Ellenőrizze, hogy Azure Active Directory és a kódmentes application server helyesen beállítva, különösen a SPN konfiguráció. Ellenőrizze, hogy az Azure AD a tartományhoz ugyanazt a tartományvezérlőnek annak érdekében, hogy a tartományvezérlőnek létesít adatvédelmi az Azure Active Directory tartományi. Győződjön meg arról, hogy a dátum és időpont konfigurációjának az Azure Active Directory és a tartományvezérlőnek vannak szinkronizálva. |
| 13020 - azure Active Directory nem lehet beolvasni a felhasználó nevében Kerberos-jegy, mivel nincs megadva a kódmentes kiszolgáló SPN. | Ez az esemény jelezheti helytelen konfigurációja közötti Azure Active Directory és a tartomány vezérlő kiszolgálón vagy a dátum és időpont konfigurációs mindkét gépen probléma. | A tartományvezérlőnek a Kerberos jegy Azure Active Directory által létrehozott elutasítva. Ellenőrizze, hogy Azure Active Directory és a kódmentes application server helyesen beállítva, különösen a SPN konfiguráció. Ellenőrizze, hogy az Azure AD a tartományhoz ugyanazt a tartományvezérlőnek annak érdekében, hogy a tartományvezérlőnek létesít adatvédelmi az Azure Active Directory tartományi. Győződjön meg arról, hogy a dátum és időpont konfigurációjának az Azure Active Directory és a tartományvezérlőnek vannak szinkronizálva. |
| 13022 - azure Active Directory nem csatlakozó felhasználók hitelesítését, mert a kódmentes kiszolgáló válaszol Kerberos hitelesítési kísérletek HTTP 401 hibát. | Ebben az esetben jelezheti helytelen konfigurációja közötti Azure Active Directory és a kódmentes application server, vagy a dátum és időpont konfigurációban mindkét gépen probléma. | A kódmentes server Azure Active Directory által létrehozott Kerberos-jegy elutasítva. Ellenőrizze, hogy Azure Active Directory és a kódmentes application server megfelelően van-e beállítva. Győződjön meg arról, hogy a dátum és időpont konfigurációjának az Azure Active Directory és a kódmentes alkalmazáskiszolgáló vannak szinkronizálva. |
| A webhely nem tudja megjeleníteni a lapon. | A felhasználó előfordulhat, hogy a következő hibaüzenet meg az alkalmazást, ha az alkalmazás IWA alkalmazás közzétett eléréséhez. Lehet, hogy az alkalmazás definiált SPN helytelen. | Az alkalmazások IWA: Győződjön meg arról, hogy helyesen-e konfigurálva ennek az alkalmazásnak a SPN. |
| A webhely nem tudja megjeleníteni a lapon. | A felhasználó előfordulhat, hogy a következő hibaüzenet meg az alkalmazást, ha az alkalmazás az Outlook Web App alkalmazás közzétett elérni. Ez az alábbiak egyike okozhatja: <br> -Az alkalmazás a definiált SPN nem megfelelő. <br> -Felhasználó elérni az alkalmazás segítségével a nagy vállalati fiók, hanem egy Microsoft-fiókkal jelentkezzen be, vagy egy Vendég felhasználó. <br> -A felhasználó, aki elérni az alkalmazás megfelelően nem tartalmazza a helyszíni oldalon ehhez az alkalmazáshoz. | Ennek megfelelően csökkentésében lépéseket: <br> -Győződjön meg arról, hogy helyesek-e konfigurálva ennek az alkalmazásnak a SPN. <br> – Ellenőrizze, hogy a felhasználó bejelentkezik a vállalati fiókkal, amely megfelel a tartományát a közzétett alkalmazást. Microsoft-Account-felhasználók és a vendégként való bekapcsolódáshoz IWA alkalmazások nem tud hozzáférni. <br> -Győződjön meg arról, hogy a felhasználó rendelkezik-e a megfelelő engedélyekkel, kódmentes alkalmazás a helyszíni számítógépen meghatározott. |
| Vállalati alkalmazás nem érhető el. Nincs engedélye ennek az alkalmazásnak eléréséhez. Nem sikerült engedélyezése. Győződjön meg arról, hogy a felhasználó, aki hozzáfér hozzárendelése az alkalmazás. | A felhasználók előfordulhat, hogy a következő hibaüzenet meg az alkalmazást, ha az azok Microsoft-fiók helyett a vállalati fiók való bejelentkezéshez használt közzétett eléréséhez. A vendégként való bekapcsolódáshoz felhasználók is jelenhetnek meg ezt a hibát. | Microsoft-Account felhasználók és a vendégek IWA alkalmazások nem tud hozzáférni. Győződjön meg arról, hogy a felhasználó bejelentkezik a vállalati fiók, amely megfelel a tartományát a közzétett alkalmazást használ. |
| Vállalati alkalmazás jelenleg nem érhető el. Próbálja meg később... Az összekötő időtúllépési. | A felhasználók előfordulhat, hogy a következő hibaüzenet meg az alkalmazást, ha nem megfelelően határozza a helyszíni oldalon ehhez az alkalmazáshoz közzétett eléréséhez. | Győződjön meg arról, hogy a felhasználó rendelkezik-e a megfelelő engedélyekkel, kódmentes alkalmazás a helyszíni számítógépen meghatározott. |


## <a name="see-also"></a>Lásd még:

- [Azure Active Directory szolgáltatásalkalmazás-Proxy engedélyezése](active-directory-application-proxy-enable.md)
- [Szolgáltatásalkalmazás-Proxy-alkalmazások közzététele](active-directory-application-proxy-publish.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Feltételes elérésének engedélyezése](active-directory-application-proxy-conditional-access.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
