<properties
    pageTitle="Azure AD Connect állapot műveletek."
    description="Ez a cikk ismerteti, miután telepítette az Azure Active Directory csatlakozás állapot végezhető további műveletek."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD Connect állapot műveletek

A következő témakör ismerteti a különböző műveletek, amelyek használatával az Azure Active Directory csatlakozás állapot végezheti el.

## <a name="enable-email-notifications"></a>Értesítő e-mailek engedélyezése
Beállíthatja, hogy az Azure Active Directory csatlakozás állapot szolgáltatás értesítő e-mailek küldése, amikor értesítéseket jelző, az Identitáskezelés infrastruktúra nem megfelelő generált. Ez akkor fordul elő, egy figyelmeztetés létrehozásakor, valamint ha van megjelölve a rendszer. Kövesse az alábbi utasításokat követve állítsa be az értesítő e-mailek.

![Azure AD Connect állapot e-mailben értesítést felfedezése](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] Értesítő e-mailek alapértelmezés szerint le vannak tiltva.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure Active Directory csatlakozás állapot értesítő e-mailek engedélyezése

1. Nyissa meg a riasztások lap a szolgáltatás, amelyhez ki szeretne e-mailben értesítést kap.
2. Kattintson az "Értesítési beállítások" gomb a műveletsávon elérhető.
3. Kapcsolja be az értesítő e-mailt kapcsolót.
4. Jelölje ki a jelölőnégyzet bejelölésével állítsa be a globális rendszergazdák, e-mailben értesítést szeretne kapni.
5. Ha szeretne e-mailben értesítéseket a bármely más e-mail címét, a többi E-mail Címzett mezőben megadhatja őket. E-mail cím eltávolítása a listából, kattintson a bejegyzésre a jobb gombbal, és válassza a Törlés elemet.
6. Véglegesítéséhez a módosításokat a "Mentés" gombra. Az összes módosítások életbe effektusok, csak a "Mentés" kijelölése után.

## <a name="delete-a-server-or-service-instance"></a>A kiszolgáló vagy szolgáltatás példány törlése

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Azure Active Directory csatlakozás állapot szolgáltatásból származó kiszolgáló törlése
Egyes esetekben előfordulhat, hogy kívánt figyelni a kiszolgáló eltávolítása. Kövesse az alábbi útmutatást kiszolgáló eltávolítása az Azure Active Directory csatlakozás állapot szolgáltatás.

A kiszolgáló törlésekor tartsa szem előtt a következőket:

- Ez a művelet SZINKRONIZÁLÁSUK bármely további adatok gyűjtése, a kiszolgáló. A kiszolgáló felügyeleti a szolgáltatás törlődik. Ez a művelet után nem fogja tudni megtekinteni a új riasztások, kiszolgáló analytics adatainak ellenőrzése vagy használatát.
- Ez a művelet nem eltávolítása, vagy távolítsa el a kiszolgálóról, az állapot ügynök. Ha ez a lépés végrehajtása előtt nem eltávolította az állapot ügynök, megjelenhet hiba események a kiszolgálón, az állapot ügynök kapcsolatos.
- Ez a művelet nem törli a kiszolgálóról már gyűjtött adatok. Az adatokat a Microsoft Azure adatmegőrzési szerinti törlődnek.
- A művelet végrehajtása után ha ki szeretne figyelése ismét ugyanazt a kiszolgálót, kell távolítsa el és telepítse újra az állapot ügynök ezen a kiszolgálón.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Azure Active Directory csatlakozás állapot szolgáltatásból származó kiszolgáló törlése

Azure AD Connect állapot, az Active Directory összevonási szolgáltatások és Azure Active Directory csatlakoztatása (szinkronizálás):

1. Nyissa meg a kiszolgáló lap a kiszolgáló a lista lap jelöljön ki, hogy távolítsa el a kiszolgáló nevét.
2. A kiszolgáló a lap kattintson a "Delete" gomb a műveletsávon elérhető.
3. A kiszolgáló törlése a megerősítést kérő párbeszédpanelen írja be a kiszolgáló nevét a művelet megerősítéséhez.
4. Kattintson a "Delete" gombra.

Azure AD Connect állapota az Active Directory tartományi szolgáltatások:

1. Nyissa meg a tartomány vezérlők irányítópult.
2. Jelölje ki a tartományvezérlőnek eltávolítjuk.
3. Kattintson a "Kijelöltek törlése" gomb a műveletsávon elérhető.
4. A kiszolgáló törlése a művelet megerősítéséhez.
5. Kattintson a "Delete" gombra.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>A szolgáltatás példány törlése szolgáltatásból Azure Active Directory csatlakozás állapota

Egyes esetekben előfordulhat, hogy kívánt távolítsa el a szolgáltatás. Kövesse az alábbi útmutatást távolítsa el a szolgáltatás szolgáltatásból Azure Active Directory csatlakozás állapota.

A szolgáltatás példányának törlésekor tartsa szem előtt a következőket:

- Ez a művelet az aktuális példány szolgáltatás eltávolítása a megfigyeléssel szolgáltatást.
- Ez a művelet nem távolítsa el vagy bármilyen a kiszolgálókat, azok ellenőrizni részeként ebben az esetben a szolgáltatás állapota ügynök eltávolítása. Eltávolította az állapot ügynök nem ez a lépés végrehajtása előtt, ha a kiszolgálókat, az állapot ügynök kapcsolatos hiba események is látható.
- Ez a szolgáltatás példány minden adatát a Microsoft Azure adatmegőrzési szerinti törlődnek.
- Ez a művelet végrehajtása után ha ki szeretne figyelése a szolgáltatást, kérjük, távolítsa el és telepítse újra az állapot ügynök ellenőrzik összes kiszolgálón. A művelet végrehajtása után ha ki szeretne figyelése ismét ugyanazt a kiszolgálót, kell távolítsa el és telepítse újra az állapot ügynök ezen a kiszolgálón.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>A szolgáltatás példány törlése szolgáltatásból Azure Active Directory csatlakozás állapota

1. Nyissa meg a szolgáltatás lap a szolgáltatást a lista lap jelöljön ki az eltávolítani kívánt szolgáltatás azonosítója (farm neve).
2. A kiszolgáló a lap kattintson a "Delete" gomb a műveletsávon elérhető.
3. Erősítse meg a megerősítést kérő párbeszédpanelen írja be a szolgáltatás neve. (például: sts.contoso.com)
4. Kattintson a "Delete" gombra.
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Szerepköralapú hozzáférés kezelése hozzáférés-vezérlés
### <a name="overview"></a>– Áttekintés
[Szerepkör alapú hozzáférés-vezérlés](role-based-access-control-configure.md) az Azure Active Directory csatlakozás egészség nyújt access Azure Active Directory csatlakozás állapot szolgáltatás felhasználók vagy csoportok globális rendszergazdák kívül. Ez a érik el a kívánt felhasználók vagy csoportok szerepkörök hozzárendelése, és lehetővé teszi a szeretné korlátozni a globális rendszergazdák a címtáron belül.

#### <a name="roles"></a>Szerepkörök
Azure Active Directory csatlakozás állapot támogatja a következő beépített szerepkörök.

| Szerepkör | Engedélyek |
| ----------- | ---------- |
| Tulajdonos | Tulajdonosok lehet ***hozzáférés kezelése*** (pl. hozzárendelése szerepkör egy felhasználó vagy csoport), ***minden információ megtekintése*** (pl. figyelmeztetések megtekintése) a portál és - ***Beállítások módosítása*** (pl. értesítő e-mailek) Azure Active Directory csatlakozás állapot belül. <br>Alapértelmezés szerint Azure AD a globális rendszergazdák a szerepkör vannak rendelve, és ezt nem módosíthatók.  |
|Közös munka|  Munkatársak is, hogy ***minden információ megtekintése*** (pl. figyelmeztetések megtekintése) a portál és - ***Beállítások módosítása*** (pl. értesítő e-mailek) Azure Active Directory csatlakozás állapot belül.|
|A képernyőolvasók| Olvasókat is, hogy ***minden információ megtekintése*** (pl. figyelmeztetések megtekintése) belül Azure Active Directory csatlakozás állapotát a portálon.|

Minden más szerepkörök (például "Felhasználói hozzáférés rendszergazdák" vagy "DevTest Labs felhasználók"), akkor is, ha elérhető a portál felület semmilyen hatást nem belül Azure Active Directory csatlakozás állapot eléréséhez.

#### <a name="access-scope"></a>Az Access hatóköre

Azure AD Connect támogatása két szinten hozzáférés kezelése:

- ***Az összes szolgáltatás példányainak***: Ez a legtöbb ügyfelek ajánlott elérési útját és szabályozza a hozzáférést az összes szolgáltatás példány (pl. ADFS-farm) keresztül szerepkör diagramtípusokat, amely az Azure Active Directory csatlakozás állapot felügyeli vannak.

- ***Szolgáltatás-példány***: bizonyos esetekben szüksége lehet access szerepköröket vagy egy szolgáltatást példány által alapuló újból. Ebben az esetben a szolgáltatás példány szintjén access kezelheti.  

Ha egy végfelhasználói van elérhető vagy Directory vagy a szolgáltatás példányának szintre van rendelkezik engedéllyel.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Hogyan Azure Active Directory csatlakozás állapot felhasználók vagy csoportok elérésének engedélyezése
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Lépések 1: Válassza ki a megfelelő hozzáférési hatókör
Szeretné, hogy a felhasználói hozzáférés Azure Active Directory csatlakozás állapot belül *az összes szolgáltatás példányainak* szintre, nyissa meg a fő lap Azure Active Directory csatlakozás állapota.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Lépés: 2: Felhasználók hozzáadása, csoportok és a szerepköröket rendelhet hozzájuk
1. Kattintson a "Felhasználók" rész a Configure szakaszából.<br>
![Azure AD Connect állapot RBAC fő lap](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Jelölje ki azokat "Hozzáadás"
3. Jelölje ki a "szerepkört" például "Tulajdonosa"<br>
![Azure AD Connect állapot RBAC felhasználó hozzáadása](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Írja be a nevét vagy a célként megadott felhasználó vagy csoport azonosítója. Kijelölhet egy vagy több felhasználót vagy csoportot egy időben. Kattintson a "select".
![Azure AD Connect állapot RBAC választó felhasználói](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Jelölje be a "Ok".<br>

6. Miután a szerepkör-hozzárendelés befejeződött, a felhasználók és csoportok jelenik a listában.<br>
![Azure AD Connect állapot RBAC felhasználói listája](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Ezeket a lépéseket lehetővé teszi, a felsorolt felhasználók és a csoport hozzáférési a kiosztott szerepkör szerint.
>[AZURE.NOTE]
- Globális rendszergazda a teljes hozzáférés az összes művelet bármikor van, de a globális rendszergazdai fiókkal nem szerepel a fenti listában.
- Azure Active Directory csatlakozás állapot belül nem támogatott szolgáltatás "Felhasználók meghívása".

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>3 lépés: Felhasználók vagy csoportok a lap hely megosztása
1. Engedélyek hozzárendelése után a felhasználó hozzáférhet Azure Active Directory csatlakozás állapot ismételt megjelenítéséhez [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Egyszer kattintson a lap, a felhasználó rögzítheti a lap vagy a különböző részei az irányítópult kattintva egyszerűen "PIN-kódot az irányítópult"<br>
![Azure Active Directory-állapot RBAC csatlakoztatása a PIN-kód lap](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Az "Olvasó" szerepkört rendelkező felhasználók nem tudnak a "létrehozása" művelet Azure Active Directory csatlakozás állapotjelző mellék lekérése az Azure Marketplace szolgáltatásból. A felhasználó akkor is elérjék a lap nyissa meg a fenti hivatkozást. Későbbi használatra a felhasználó rögzítheti a az irányítópult lap.

### <a name="remove-users-andor-groups"></a>Felhasználók vagy csoportok eltávolítása
Egy felhasználó vagy csoport jobb gombbal, és válassza az Eltávolítás Azure Active Directory csatlakozás állapot szerepkör alapú hozzáférés-vezérlés rész hozzá eltávolíthatja.<br>
![Azure AD Connect állapot RBAC eltávolítása felhasználói](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Kapcsolódó hivatkozások

* [Azure AD Connect állapota](active-directory-aadconnect-health.md)
* [Azure AD Connect állapot ügynök telepítésekor](active-directory-aadconnect-health-agent-install.md)
* [Azure Active Directory használatával kapcsolatba állapot Active Directory összevonási szolgáltatások](active-directory-aadconnect-health-adfs.md)
* [Azure Active Directory csatlakozás állapota szinkronizálás használata](active-directory-aadconnect-health-sync.md)
* [Azure Active Directory segítségével kapcsolatba állapot Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect állapot – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect állapot korábbi verziók](active-directory-aadconnect-health-version-history.md)
