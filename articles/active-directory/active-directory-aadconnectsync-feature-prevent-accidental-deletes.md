<properties
   pageTitle="Azure AD Connect szinkronizálása: véletlen törlés tiltása |} Microsoft Azure"
   description="Ez a témakör ismerteti az Azure AD Connect megakadályozása (megakadályozása véletlen törlését) véletlen törlése szolgáltatás."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect szinkronizálása: véletlen törlés tiltása
Ez a témakör ismerteti az Azure AD Connect megakadályozása (megakadályozása véletlen törlését) véletlen törlése szolgáltatás.

Ha telepíti az Azure AD Connect megakadályozása véletlen törlése alapértelmezés szerint engedélyezve, és nem engedélyezte a több mint 500 törli az exportálás. Ez a funkció akkor véletlen konfigurálása és a változásokat szembeni védekezésben, amely sok felhasználó- és egyéb objektumok befolyásolhatja a helyszíni címtárában lett tervezve.

Ha olyan sok törlése esetei:

- Megváltozik, [Szűrés](active-directory-aadconnectsync-configure-filtering.md) ahol egy teljes [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) vagy [tartomány](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) nincs bejelölve.
- A szervezeti egység összes objektumát törlődnek.
- Szervezeti egység átnevezése, a benne lévő összes objektumot számítanak ki a hatókör-szinkronizálás.

Az alapértelmezett érték 500 objektumok megváltoztathatja a PowerShell használatával `Enable-ADSyncExportDeletionThreshold`. Konfigurálnia kell a szervezet méretéhez ezt az értéket. Mivel a szinkronizálási ütemező 30 percenként fut, az érték törlése 30 percen belüli látható számát.

Azure Active Directory exportálandó előkészített túl sok törlése esetén, majd az Exportálás leállítja, és kap egy e-mailben jelennek meg:

![Megakadályozhatja, hogy e-mailek véletlen törlése](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Helló (technikai kapcsolattartó). A (idő) az identitás-szinkronizálási szolgáltatás előforduló, hogy a törölt elemek számát túllépte a beállított törlés küszöbértékét (a szervezet neve). Egy teljes (szám) objektumok az identitás-szinkronizálás futtatása a törlésre küldték. Ez a teljesülnie kell, vagy túllépte a beállított törlés küszöb értéket (szám) objektumok. Meg kell adnia a megerősítést kérő szükséges, hogy a törlés célszerű lehet feldolgozni lesz a folytatás előtt. Olvassa el a további információt a hiba-e az e-mailben felsorolt megakadályozó véletlen törlését.*

Is megjelenik az állapot `stopped-deletion-threshold-exceeded` mikor, keresse meg a felhasználói felület **Szinkronizálási szolgáltatáskezelő** az Exportálás profil.
![Szinkronizálási Service Manager felhasználói felület véletlen törlés tiltása](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Ha nem várt, vizsgálja meg, és helyesbítő műveleteket. Törlés objektumai megtekintéséhez tegye a következőket:

1. Indítsa el a **szinkronizálási szolgáltatás** a Start menüből.
2. Nyissa meg **az összekötők**.
3. **Azure Active Directory**-típus, jelölje ki az összekötőt.
4. A **Műveletek** jobbra jelölje ki a **Keresési összekötő területet**.
5. Az előugró ablakban **hatókör**csoportban jelölje ki a **Megszakad a kapcsolata óta** , és kiválasztja az elmúlt. Kattintson a **Keresés**gombra. Ez az oldal ismerteti az összes objektumot törölni fog látni. Egyes elemekre kattintva kaphat további információt az objektumot. **Oszlop beállítása** a rács látható legyen a jellemzőket hozzáadása is kattinthat.

![Keresés összekötő terület](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Az összes törlése van szükség, majd tegye a következőket:

1. Az ideiglenesen tiltsa le a védelmet, és mondja el folyamatát, futtassa a PowerShell-parancsmag adott törlése: `Disable-ADSyncExportDeletionThreshold`. Adja meg az Azure Active Directory globális rendszergazdai fiókkal és a jelszavát.
![Hitelesítő adatok](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Az Azure Active Directory Connector legyen kijelölve válassza a **Futtatás** műveletet, és válassza az **Exportálás**.
3. Ahhoz, hogy újra a védelmet, futtassa a PowerShell-parancsmagot: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Következő lépések

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
