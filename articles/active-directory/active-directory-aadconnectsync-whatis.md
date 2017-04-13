<properties
    pageTitle="Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és testre szabhatja a szinkronizálás |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure AD Connect szinkronizálása dolgozik, és testreszabása."
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
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása
Az Azure Active Directory csatlakozás szinkronizálási szolgáltatások (Azure AD Connect sync) Azure AD Connect fő része. Gondoskodik, hogy a adatszinkronizálás identitás a helyszíni környezet és Azure Active Directory kapcsolódó műveletek. Azure AD Connect szinkronizálása a DirSync, Azure AD-szinkronizálás és az Azure Active Directory Connector konfigurálva a Forefront identitáskezelő követő.

Ez a témakör az otthoni a **szinkronizálási Azure AD Connect** (más néven **sync engine**), és hozzá kapcsolódó összes egyéb témakörökre mutató hivatkozások listája. Azure AD Connect hivatkozásokat [integrálása az Azure Active Directory címtárral a helyszíni identitások](active-directory-aadconnect.md)című témakör tartalmaz.

A szinkronizálási szolgáltatás két összetevők, a helyszíni **Azure AD Connect szinkronizálási** összetevő és áll a szolgáltatási oldalon az **Azure AD Connect szinkronizálási szolgáltatás**nevű Azure AD. A szolgáltatás nem közös DirSync, Azure AD-szinkronizálás és Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect szinkronizálási témakörök

A témakör | Mire vonatkozik, és mikor olvasása
----- | -----
**Azure AD Connect szinkronizálási alapjai** |
[A architektúra ismertetése](active-directory-aadconnectsync-understanding-architecture.md) | Azok, akik nem a szinkronizálási motor, és szeretné, ha többet szeretne tudni a architektúra és a kifejezések.
[Műszaki fogalmak](active-directory-aadconnectsync-technical-concepts.md) | Rövid architektúra témakör és röviden bemutatja használt kifejezések.
[Az Azure Active Directory topológiák csatlakoztatása](active-directory-aadconnect-topologies.md) | A különböző topológiák és -esetek támogatja a szinkronizálási motor ismerteti.
**Egyéni konfiguráció** |
[A telepítés varázsló ismételt futtatása](active-directory-aadconnectsync-installation-wizard.md) | Megtudhatja, milyen lehetőségeket van érhető el, ha ismét futtatja az Azure AD Connect telepítővarázslóban.
[Deklaráció kiépítési ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| A deklaráció kiépítési nevű konfigurációs modell ismerteti.
[Deklaráció kiépítési kifejezések ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | A kifejezés nyelvét deklaráció kiépítési szintaxisának ismerteti.
[Az alapértelmezett beállítások ismertetése](active-directory-aadconnectsync-understanding-default-configuration.md)| A kimenő kész szabályok és az alapértelmezett beállítások ismerteti. A szabályok összhatását a munkát a kimenő kész felhasználási területei is ismerteti.
[Tudnivalók a felhasználók és a partnerek](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Az előző témakör továbbra is fennáll, és ismerteti, hogyan a konfiguráció a felhasználók és a partnerek műveleteinek összhatását, különösen több erdőre környezetben.
[Hogyan lehet módosítani az alapértelmezett beállítás](active-directory-aadconnectsync-change-the-configuration.md) | Végigvezeti az általános beállítások megváltoztatását attribútum flow hogyan.
[Gyakorlati tanácsok az alapértelmezett beállítások módosítása](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Korlátozások támogatási és a változtatásokat a kimenő kész konfiguráción.
[Szűrés konfigurálása](active-directory-aadconnectsync-configure-filtering.md) | A különböző lehetőségek korlátozása mely objektumai a rendszer éppen szinkronizált az Azure Active Directory és részletes annak konfigurálásához, ezeket a beállításokat ismerteti.
**Szolgáltatások és -esetek** |
[Véletlen törlés tiltása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | A *véletlen törlés megakadályozása* szolgáltatás, és hogyan kell beállítani ismerteti.
[A Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md) | A beépített ütemezőt, importálása, szinkronizálását, és az adatok exportálásával kapcsolatos ismerteti.
[Jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md) | Hogyan működik a jelszó-szinkronizálás, megvalósításáról, és hogyan működik, és hibaelhárítása – mutatja be.
[Eszköz visszaírást](active-directory-aadconnect-feature-device-writeback.md) | Ismerteti, hogyan működik a eszköz visszaírást az Azure AD Connect.
[A címtár-bővítmények](active-directory-aadconnectsync-feature-directory-extensions.md) | Ha ki szeretné terjeszteni a saját egyéni attribútumokkal rendelkező az Azure Active Directory séma ismerteti.
**Szinkronizálási szolgáltatás** |
[Azure AD Connect szinkronizálási szolgáltatások](active-directory-aadconnectsyncservice-features.md) | A szinkronizálási szolgáltatás oldal és a szinkronizálási beállítások módosítása az Azure Active Directory ismerteti.
[Ismétlődő attribútum tűrőképessége](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Megtudhatja, hogy miként engedélyezheti és **userPrincipalName** és a **proxyAddresses** ismétlődő attribútum értékek tűrőképessége használhatja.
**Tevékenységek és a felhasználói felület** |
[Szinkronizálás Service Manager](active-directory-aadconnectsync-service-manager-ui.md) | A szinkronizálás szolgáltatáskezelő felhasználói felülete [Műveletek](active-directory-aadconnectsync-service-manager-ui-operations.md), [összekötők](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)és [Metaverz keresés](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) lapok ismerteti.
[Műveleti feladatok és szempontok](active-directory-aadconnectsync-operations.md) | Műveleti aggályokat, például vészhelyreállítás ismerteti.
**kézikönyv...** |
[Az Azure Active Directory-fiók visszaállítása](active-directory-aadconnectsync-howto-azureadaccount.md) | Hogyan lehet a szolgáltatásfiók, használja az Azure AD Connect szinkronizálási Azure AD a hitelesítő adatok alaphelyzetbe állítása.
**További információk és hivatkozások** |
[Portok](active-directory-aadconnect-ports.md) | Meg kell nyitnia a szinkronizálási motor és a helyszíni könyvtárak és Azure Active Directory között portok listája.
[Azure Active Directory szinkronizált attribútumok](active-directory-aadconnectsync-attributes-synchronized.md) | Listák közötti a helyszíni Active Directory és Azure Active Directory szinkronizált összes attribútum.
[Hivatkozás függvény](active-directory-aadconnectsync-functions-reference.md) | Az összes elérhető deklaráció kiépítési függvény listája.

## <a name="additional-resources"></a>További források

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
