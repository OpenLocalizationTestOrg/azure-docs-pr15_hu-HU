<properties
    pageTitle="Azure AD Connect szinkronizálása: varázslóval telepítési másodszori |} Microsoft Azure"
    description="Megtudhatja, hogy az telepítővarázslóban hogyan működik a második futtatásakor."
    keywords="Karbantartási beállítások konfigurálása a második alkalommal futtatja az Azure AD Connect telepítővarázslóban segítségével"
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect szinkronizálása: a telepítés varázslót második alkalommal fut
Az első alkalommal futtatja az Azure AD Connect telepítési varázslója azt végigvezeti a telepítés konfigurálása. Ha ismét a telepítés varázslót, is kínál a karbantartás lehetőségeit.

A telepítés varázslót az **Azure AD Connect**nevű start menüjében talál.

![Start menü](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

A telepítés varázsló indításakor megjelenik egy ezeket a beállításokat tartalmazó lapra:

![És a feladatok lap](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Ha az Azure AD Connect ADFS telepítve van, még több lehetőség közül választhat. [ADFS-kezelés](active-directory-aadconnect-federation-management.md#ad-fs-management)ADFS telepítve van, a további lehetőségeket ismerteti.

Jelöljön ki egy feladatot, és kattintson a **Tovább gombra** a folytatáshoz.

> [AZURE.IMPORTANT] Miközben a telepítés varázslót, nyissa meg, a szinkronizálási motor az összes művelet felfüggeszti a vannak. Győződjön meg arról, amint a konfigurációs módosítások befejezése bezárja a telepítés varázslót.

## <a name="view-current-configuration"></a>Jelenlegi konfigurációjának megtekintése
Ez a beállítás a jelenleg beállított lehetőségek gyors áttekintést nyújt.

![Az összes beállítást, és állapotuk lap](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Kattintson az **előző** térhet vissza. Válassza a **Kilépés**, ha bezárja a telepítés varázslót.

## <a name="customize-synchronization-options"></a>A szinkronizálás beállításainak testreszabása
Ez a beállítás módosításához a szinkronizálási konfiguráció használják. Megjelenik a konfigurációs egyéni telepítési elérési beállítások egy részét. Ez a beállítás akkor is, ha az eredetileg használt express telepítésével látni.

- [További könyvtárak hozzáadásával](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Eltávolíthat könyvtárában, olvassa el a [összekötő törlése](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete)című témakört.
- A [tartomány módosítása és -szűrés OU](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Csoport törlése a szűrést.
- [Módosítás választható funkció](active-directory-aadconnect-get-started-custom.md#optional-features).

Az egyéb beállításokat, az a kezdeti telepítés nem áll rendelkezésre, és nem módosíthatók. Ezek a következők:

- Módosítsa az attribútum userPrincipalName és sourceAnchor használható.
- Objektumok csatlakozó módjának módosítása a különböző erdőből.
- A csoport alapú szűrés engedélyezése.

## <a name="refresh-directory-schema"></a>A címtár-séma frissítése
Ez a beállítás használható, ha megváltoztatta a séma az egyik a helyszíni Active Directory tartományi szolgáltatások erdők. Például akkor lehet, hogy Exchange telepített vagy frissített a Windows Server 2012 séma eszköz objektumokkal. Ebben az esetben meg kell arra utasítja az Active Directory tartományi szolgáltatásokból újra olvassa el a séma, és a gyorsítótár frissítése Azure AD Connect. Ez a művelet is újragenerálása a szinkronizálási szabályokat. Ha szerepel példaként, Exchange-séma ad hozzá, a szinkronizálási szabályokat, az Exchange kerülnek, a konfiguráción.

Ha ezt a beállítást választja, a konfigurációban összes könyvtárak sorolja fel. Az alapértelmezett megőrzése és frissítése az összes erdők vagy a kijelölés megszüntetése néhány oldalt.

![A környezet összes könyvtárak és a lapon](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Fejlesztői üzemmód beállítása
Ezzel a beállítással engedélyezése és letiltása a kiszolgáló átmeneti tárolásra szolgáló módban. További információt a módot, és hogyan használják átmeneti tárolására [Műveletek](active-directory-aadconnectsync-operations.md#staging-mode)találhatók.

A beállítás látható átmeneti jelenleg engedélyezhető vagy tiltható le:  
![Fejlesztői üzemmód aktuális állapotát is megjelenítő beállítás](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Az állapot módosításához válassza ezt a lehetőséget, és jelölje ki vagy a kijelölés megszüntetése a jelölőnégyzetet.  
![Fejlesztői üzemmód aktuális állapotát is megjelenítő beállítás](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Felhasználónak bejelentkezés módosítása
Ezzel a beállítással módosítása a jelszó-szinkronizálás összevonási vagy fordítva. **Nem**beállítása nem módosítható.

Ezt a beállítást a további tudnivalókért lásd: a [felhasználónak bejelentkezés](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Következő lépések

- További tudnivalók az Azure AD Connect szinkronizálási [Ismertetése deklaráció kiépítési](active-directory-aadconnectsync-understanding-declarative-provisioning.md)által használt modell.

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
