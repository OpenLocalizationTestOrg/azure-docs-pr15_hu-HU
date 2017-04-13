<properties
    pageTitle="Azure AD Connect: Az első lépések express beállításokkal |} Microsoft Azure"
    description="Útmutató letöltése, telepítése, és futtassa a beállítási varázsló az Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Azure AD Connect express beállításokkal – első lépések
Azure AD Connect **Express beállításai** használatos, amikor egy egyetlen erdőn topológiája és a [Jelszó-szinkronizálás](../active-directory-aadconnectsync-implement-password-synchronization.md) hitelesítéshez. **Express beállításai** az alapértelmezett beállítás, és használja a leggyakrabban telepített forgatókönyvet. Csak néhány rövid kattintással nem vagyok a gépnél, hogy a helyszíni címtárában a felhőbe kiterjesztése áll.

Azure AD Connect telepítés megkezdése előtt győződjön meg arról, hogy [Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) töltheti le, és a teljes a előzetesen szükséges lépések leírását [Azure AD Connect: hardver- és előfeltételek](../active-directory-aadconnect-prerequisites.md).

Ha a express beállítások nem egyezik meg a topológia, olvassa el a [kapcsolódó dokumentáció](#related-documentation) más esetben.

## <a name="express-installation-of-azure-ad-connect"></a>Express telepítésével Azure AD Connect
Megtekintheti a művelet a [Videók](#videos) szakaszban az alábbi lépéseket.

1. Azure AD Connect telepíti a kiszolgálóra helyi rendszergazdaként jelentkezzen be. A kiszolgálón, hogy a szinkronizálási kiszolgáló kíván tegye.
2. Nyissa meg azt, és kattintson duplán a **AzureADConnect.msi**.
3. Az üdvözlőképernyő jelölje be a jelölőnégyzetet, amelyben a licencfeltételeket, és kattintson a **Tovább**gombra.  
4. Express beállításai képernyőn jelölje be a **sürgős beállítások használata**választógombot.  
![Üdvözli a Azure Active Directory csatlakoztatása](./media/active-directory-aadconnect-get-started-express/express.png)
5. A képernyőre Azure AD Connect írja be a felhasználónevét és jelszavát a globális rendszergazda az Azure Active Directory. Kattintson a **Tovább**gombra.  
![Azure Active Directory csatlakoztatása](./media/active-directory-aadconnect-get-started-express/connectaad.png) Ha hiba jelenik meg, és a csatlakozási problémák lépnek fel, majd ellenőrizze [kapcsolódási problémák elhárítása](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. A csatlakozás Active Directory tartományi szolgáltatások képernyő írja be a felhasználónevét és jelszavát egy vállalati rendszergazdai fiókjával. A tartomány rész NetBios vagy a teljes Tartománynevet formátumban, azaz FABRIKAM\administrator vagy fabrikam.com\administrator adhat meg. Kattintson a **Tovább**gombra.  
![Active Directory tartományi Szolgáltatásokban összekapcsolása](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Az [**Azure Active Directory bejelentkezési beállítása**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) lapon csak jeleníti meg, ha Ön nem fejeződött be [a tartomány ellenőrzése](../active-directory-add-domain.md) a [Előfeltételek](../active-directory-aadconnect-prerequisites.md).
![A tartományok nem ellenőrzött](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Ha megjelenik a lapon, majd tekintse át a minden tartomány **Nem adja hozzá** , és **Nincs ellenőrizve**. Ellenőrizze, hogy használja ezeket a tartományokat az Azure Active Directory már ellenőrizte. Kattintson az frissítési a tartomány ellenőrzését.
8. A felkészült beállítása képernyő kattintson a **telepítés**gombra.
    - Tetszés szerint a lap beállítása kész, megszüntetheti a **Indítsa el a szinkronizálást, amint konfigurációs befejezése** jelölőnégyzetet. Érdemes kijelölésének megszüntetése ezt a jelölőnégyzetet, ha további konfigurációja, például a [szűrési](../active-directory-aadconnectsync-configure-filtering.md)műveletek. Akkor a kijelölés megszüntetése ezt a lehetőséget, ha a varázsló konfigurálása szinkronizálása, de az ütemező letiltva marad. Csak akkor kapcsolhatja be manuálisan [a telepítés varázslót](../active-directory-aadconnectsync-installation-wizard.md)újrafuttatásával nem működik.
    - A helyszíni Active Directory Exchange is van, majd is esetén [**az Exchange hibrid telepítésének**](https://technet.microsoft.com/library/jj200581.aspx)engedélyezése beállítást. Akkor engedélyezze ezt a beállítást, ha azt tervezi, hogy az Exchange-postaládák, mind a felhőben, és a helyszíni egyszerre.
![Készen áll a Azure AD Connect konfigurálása](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. A telepítés befejezése után kattintson a **Kilépés**gombra.
10. A telepítés befejezése után jelentkezzen, és jelentkezzen be újra a szinkronizálás szolgáltatáskezelő vagy szinkronizálás szabály szerkesztő használata előtt.

## <a name="videos"></a>Videók

Egy videót az express telepítésével a következő cikkekben talál:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Következő lépések
Most, hogy van-e telepítve Azure AD Connect lehet [ellenőrzi a telepítést, és a licencek hozzárendelése](../active-directory-aadconnect-whats-next.md).

További tudnivalók a következő funkciók, melyek engedélyezett a telepítés: [Automatikus frissítés](../active-directory-aadconnect-feature-automatic-upgrade.md), [Törlés tiltása véletlen](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)és [Azure Active Directory csatlakozás állapota](../active-directory-aadconnect-health-sync.md).

További tudnivalók az alábbi általános témakörök: [ütemezőt, és hogy miként indíthatja el a szinkronizálási](../active-directory-aadconnectsync-feature-scheduler.md).

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Kapcsolódó dokumentáció

A témakör |  
--------- | ---------
Azure AD Connect – áttekintés | [A helyszíni identitások integrálása az Azure Active Directory](../active-directory-aadconnect.md)
Telepítse a testre szabott beállítások használata | [Azure AD Connect egyéni telepítése](active-directory-aadconnect-get-started-custom.md)
A DirSync frissítés | [Frissítés az Azure Active Directory-szinkronizáló (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
A fiókok telepítéshez | [További információ: Azure AD Connect fiókok és engedélyek](active-directory-aadconnect-accounts-permissions.md)
