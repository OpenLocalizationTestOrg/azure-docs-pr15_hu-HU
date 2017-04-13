<properties
    pageTitle="Azure AD Connect szinkronizálása: szinkronizálás szolgáltatáskezelő felhasználói felület |} Microsoft Azure"
    description="Azure AD Connect az összekötők lapon a szinkronizálás szolgáltatáskezelő megértése"
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
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect szinkronizálása: szinkronizálás Service Manager

[Műveletek](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Összekötők](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer alkalmazásban](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverz keresés](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Az összekötők lapon a szinkronizálási motor csatlakozik az összes rendszerek kezelésére szolgál.

## <a name="connector-actions"></a>Összekötő műveletek

Művelet | Megjegyzés
--- | ---
Hozzon létre | Ne használja. További AD erdők csatlakozik, használja a telepítés varázslót.
Tulajdonságok | Tartomány és -szűrés OU használható.
[Törlése](#delete) | Használja, vagy az összekötő térköz az adatok törlése vagy erdőhöz kapcsolat törléséhez.
[Állítsa be a Futtatás profilok](#configure-run-profiles) | Tartomány kivételével szűrése el az alábbi konfigurálása. Ez a művelet segítségével már beállított futtatása profilok című témakör tartalmaz.
Futtatása | Indítsa el a profil egy egyszeri futtatás szolgál.
állj | Futó profil összekötő leáll.
Összekötő exportálása | Ne használja.
Összekötő importálása | Ne használja.
Összekötő frissítése | Ne használja.
Séma frissítése | A gyorsítótárban tárolt séma frissült. Célszerű használni kívánt helyett használja a lehetőséget a telepítővarázslóban óta is frissítések szabályok szinkronizálása.
[Keresés összekötő lemezterület](#search-connector-space) | Objektumok megkeresése használt, és [hajtsa végre az objektum tartalmával együtt – a rendszer](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Törlése
A törlési művelet a két különböző dolgot szolgál.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

A beállítás **csak az összekötő tárhely törlése** eltávolítja az összes adatot, de a konfigurációs megtartása.

Az **összekötő törlése és összekötő hely** eltávolítása az adatok és a konfigurációs. Ez a beállítás használható, ha nem szeretné, hogy a továbbiakban Csatlakozás erdőhöz.

Mindkét beállítások szinkronizálása az összes objektumot, és frissítse az metaverse objektumokat. Ez a művelet egy olyan időigényes művelet.

### <a name="configure-run-profiles"></a>Állítsa be a Futtatás profilok
Ezzel a beállítással a konfigurált összekötők futtatása profilok című témakör tartalmaz.

![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Keresés összekötő terület
A keresés összekötő terület művelet akkor hasznos, objektumok megkeresése és adatok kapcsolatos problémák megoldása.

![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Első lépésként **hatókörét**. Kereshet az adatok alapján (relatív megkülönböztető neve, DN, horgony, alszint fa), vagy az objektum (minden más beállítások) állapota.  
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Ha például egy alszint fa keresést, az összes objektum kap egy szervezeti.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) a rács megadhatja az-objektum **Tulajdonságok**kijelölése és az [azt követő](#follow-an-object-and-its-data-through-the-system) a forrás összekötő hely a metaverse keresztül, és a cél összekötő sorköz eltávolítása.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Hajtsa végre az objektum tartalmával együtt a rendszer keresztül
Referenciaként célszerű adatok problémája, a forrás összekötő szóköz objektum követve a metaverse, és a célba összekötő terület fő eljárás megértéséhez, hogy miért adatokat nem a várt értéket tartalmaz.

### <a name="connector-space-object-properties"></a>Összekötő terület objektum tulajdonságai
**Importálás**  
Amikor megnyit egy cs-objektum, vannak több tabulátorok a képernyő tetején. A **importálása** lapon az importálás után elő van készítve adatokat jeleníti meg.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) a **Régi értéket** jeleníti meg, mi jelenleg tárolja a rendszer, és mi az adatforrás rendszer érkezett, és nem telepítették az **Új értéket** . Ebben az esetben mivel a szinkronizálási hiba, a módosítás nem alkalmazható.

**Hibaüzenet**  
A hibalap csak akkor látható, ha a probléma az objektum. A részletek kapcsolatban további információt a [szinkronizálási hibák](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab)elhárítása a Műveletek lapra.

**Leszármazással**  
A leszármazással lap jeleníti meg, hogyan viszonyul az összekötő terület objektum az metaverse objektumra. Az összekötő módosítás az utolsó importálásakor a csatlakoztatott rendszer, és milyen szabályokat alkalmazza a metaverse adatok kitöltéséhez az láthatja.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) tartozó **művelet** oszlopban, akkor láthatja van egy **bejövő** szinkronizálási szabály művelethez **rendelkezni**. Ez azt jelzi, hogy mindaddig, amíg az összekötő terület objektum, metaverse objektum marad. Ha szinkronizálási a szabályok listáját inkább szinkronizálási szabály irányba **kimenő** és **rendelkezni**, azt jelzi, hogy az objektum a metaverse objektumok törlésekor a program törli.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) is megjelenik a **PasswordSync** oszlopban, hogy a bejövő összekötője szóköz hozzájárulhat a jelszó módosítása, mivel az egy szinkronizálási szabályt az **Igaz**értéket tartalmazza. Erre a jelszóra kattintson a rendszer elküldi a kimenő szabály keresztül Azure AD.

A leszármazással lapon érheti el a metaverse [Metaverse objektum tulajdonságai](#metaverse-object-properties)gombra kattintva.

Az összes lap alján van két gomb: **előnézeti** , és **Jelentkezzen be**.

**Előzetes verzió**  
A kép lap szinkronizálása egy egyetlen objektum szolgál. Ez akkor hasznos, ha bizonyos ügyfél szinkronizálási szabályok üzenetátvitel, és szeretné, hogy a változás hatása egyetlen objektum. **Teljes szinkronizálása** és **Delta szinkronizálási**közül választhat. Emellett bejelölheti **Kép készítése**, amely csak a memóriában továbbra is a módosítása, és **Véglegesítse előzetes**, mely az összes szakaszokat módosításaira cél összekötő szóközöket között.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) az objektumot, és milyen szabályt alkalmazza, egy adott attribútum továbbításhoz vizsgálatára használható.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Napló**  
A napló lap olvassa el a jelszó-szinkronizálás állapotának és a [Jelszó-szinkronizálás elhárítása](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) további információt talál az előzmények szolgál.

### <a name="metaverse-object-properties"></a>Metaverse objektum tulajdonságai
**Attribútumok**  
Az attribútumok lapon megtekintheti, hogy az értékek, és mely összekötő befizetett azt.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**összekötők**  
Az összekötők lapon minden összekötő szóközt, amelyek a megadott az objektum látható.
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) ezen a lapon is lehetővé teszi az [összekötő terület objektum](#connector-space-object-properties)megkeresése.

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
