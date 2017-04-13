<properties
    pageTitle="Azure AD Connect szinkronizálása: gyakorlati tanácsok az alapértelmezett beállítások módosításával |} Microsoft Azure"
    description="Gyakorlati tanácsokat a Azure AD Connect szinkronizálási alapértelmezett beállítás módosításához."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect szinkronizálása: gyakorlati tanácsok az alapértelmezett beállítások módosítása
Ez a témakör célja Azure AD Connect szinkronizálási támogatott és a nem támogatott módosításai ismertetik.

A konfiguráció Azure AD Connect által létrehozott "," a helyszíni Active Directory szinkronizálása az Azure AD a legtöbb környezetek működik. Jó helyen jár egyes esetekben szükség néhány módosításainak alkalmazása az egy adott segítségre van szüksége, vagy a követelmény kielégítéséhez konfigurációt.

## <a name="changes-to-the-service-account"></a>A fiók módosítása
Azure AD Connect szinkronizálás csoportban a telepítés varázsló által létrehozott szolgáltatásfiók fut. Ez a szolgáltatás-fiók a titkosítási kulcs rendelkezik, a szinkronizálási által használt adatbázishoz. Létrehozás 127 karakter hosszú jelszóval, és a jelszó úgy van állítva, hogy ne járjon le.

- Még **nem támogatott** módosítása vagy a szolgáltatás fiók jelszavának alaphelyzetbe állítása. A titkosítási kulcs így megsemmisít és a szolgáltatást, akkor nem képes a hozzáférést az adatbázishoz, és nem tud elindulni.

## <a name="changes-to-the-scheduler"></a>Változások az ütemező
A kiadásairól az összeállítás 1.1 kezdődő (február 2016) beállíthatja, hogy az alapértelmezettől eltérő szinkronizálási ciklust 30 percig az [ütemezőt](active-directory-aadconnectsync-feature-scheduler.md) .

## <a name="changes-to-synchronization-rules"></a>Változások szinkronizálása szabályok
A telepítés varázslót biztosít a leggyakoribb forgatókönyvet személyre fájlját konfiguráció. Abban az esetben, ha szeretne változtatásokat végezni a konfiguráción, majd kövesse ezeket a szabályokat a támogatott konfiguráció továbbra is.

- Módosítható [attribútum flow](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) , ha az alapértelmezett közvetlen attribútum flow nem alkalmasak a szervezet számára.
- Ha [folyamat tulajdonság](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) szeretne, és Azure Active Directory távolítson el minden meglévő attribútum-értéket, majd meg kell ebben az esetben a szabály létrehozása.
- [Tiltsa le egy nem kívánt szinkronizálási szabály](#disable-an-unwanted-sync-rule) törlés helyett. A rendszer a törölt szabályok a frissítés során újra.
- [Egy kész a szabály módosítása](#change-an-out-of-box-rule)kell az eredeti szabály másolatot készíteni, és tiltsa le a kimenő kész szabály. A szinkronizálási szabály szerkesztő kéri, és segítségével.
- Exportálja az egyéni szinkronizálási szabályokat, a szinkronizálási szabályok szerkesztőben. A szerkesztő biztosít egy PowerShell-parancsprogramot is használhatja egyszerűen hozza létre őket egy visszaállításához összeomlást követő helyreállítás során.

>[AZURE.WARNING] A kimenő kész szinkronizálási szabályokat egy ujjlenyomat van. Ha egy módosítást szabályok az ujjlenyomatot már nem egyező van. Lehet, hogy problémákat későbbi használatakor alkalmazhat egy új kiadását Azure AD Connect. Csak módosításokat leírt módon ebben a cikkben.

### <a name="disable-an-unwanted-sync-rule"></a>Egy nem kívánt szinkronizálási szabály letiltása
Ne töröljön egy kimenő kész szinkronizálási szabályt. A rendszer következő frissítés során újra.

Bizonyos esetekben a telepítés varázslót, amely nem működik a topológia konfiguráció van mutatni. Például ha egy fiókot az erőforrás erdő topológia van, de meg az Exchange séma használata a fiók erdőben a séma van bővített, akkor az Exchange szabályok jönnek létre a fiókerdők és erőforráserdőben. Ebben az esetben meg kell tiltsa le a szinkronizálási szabály Exchange.

![Letiltott szinkronizálási szabály](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

A fenti képen az telepítővarázslóban talált egy régi Exchange 2003-sémát az fiókerdők. A séma bővítményhez hozzá lett adva, mielőtt erőforráserdőben Fabrikam környezetben jelent meg. Annak érdekében, hogy nincs régi Exchange végrehajtása attribútumok vannak szinkronizálva, a szinkronizálási szabály kell tiltható le látható módon.

### <a name="change-an-out-of-box-rule"></a>Egy kész a szabály módosítása
Ha módosítania kell egy kimenő kész szabályt, ezután kell készítsen másolatot a kimenő kész szabály, és tiltsa le az eredeti szabály. Végezze el a módosításokat a klónozott szabályhoz. A szinkronizálási szabály szerkesztő segít ezeket a lépéseket. Amikor megnyit egy kimenő kész szabályt, ez a párbeszédpanel jelenik meg:  
![Figyelmeztetés ki a szabály párbeszédpanel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Válassza az **Igen gombra** a szabály másolatot szeretne készíteni. A klónozott szabály nyitja meg.  
![Klónozott szabály](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

A klónozott szabályt végezze el szükség módosításokat hatókörét, join és átalakítások.

## <a name="next-steps"></a>Következő lépések

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
