<properties
    pageTitle="Azure AD Connect szinkronizálása: műszaki fogalmak |} Microsoft Azure"
    description="A műszaki fogalmak az Azure AD Connect szinkronizálási ismerteti."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect szinkronizálása: műszaki fogalmak
Ez a témakör az [ismertetése architektúra](active-directory-aadconnectsync-technical-concepts.md)témakör összefoglalását.

Azure AD Connect szinkronizálási épít egyszínű metadirectory szinkronizálási platformot.
Az alábbi szakaszok a fogalmak metadirectory szinkronizálás vezet be.
MIIS ILM és FIM épít az Azure Active Directory szinkronizáló szolgáltatások a következő platform csatlakozás adatforrásokhoz, adatszinkronizálás adatforrásokhoz, valamint a kiépítési között, és identitások megszüntetés biztosít.

![Műszaki fogalmak](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

A következő szakaszokban további részleteket a FIM-szinkronizálási szolgáltatás az alábbi szempontokat:

- Összekötő
- Attribútum továbbításához
- Összekötő terület
- Metaverse
- Kiépítése

## <a name="connector"></a>Összekötő

A kód modulokat, amellyel kommunikálni egy csatlakoztatott Directoryval úgynevezett összekötők (korábbi nevén management ügynökök (m/m)).

Ezek telepítve van a számítógépen, amelyen az Azure AD Connect szinkronizálása.
Az összekötők adja meg a távoli rendszer protokollok használatával helyett a központi speciális ügynökök használna Serverhez agentless lehetősége. Ez azt jelenti, csökkent kockázatok és üzembe időpontok, különösen fontos alkalmazások és rendszerek kapcsolatban felmerülő.

A fenti képen az összekötő az összekötő térköz szinonimája, de magában foglalja az összes kommunikációt a külső rendszerhez.

Az összekötő összes importáláshoz felel funkciók exportálása a rendszer és felszabadítások fejlesztők az erre szolgáló miként rendszerhez való csatlakozáshoz minden natív módon használatakor deklaráció kiépítési adatok átalakítások testreszabásához.

Import és exportnak csak fordulhat elő, ha ütemezve, a módosításokat, a rendszer, mivel a módosítások automatikusan propagálja a csatlakoztatott adatforrás előforduló további szigetelés lehetővé tevő. Ezenkívül a fejlesztők számára is hozzon létre saját összekötők gyakorlatilag bármilyen adatforráshoz csatlakozhat.

## <a name="attribute-flow"></a>Attribútum továbbításához

A metaverse az összevont nézet összes illesztett azonosítók a szomszédos összekötő szóközöket. A fenti ábrán a olyan az attribútum folyamat téglalapként ábrázol a bejövő és kimenő továbbításhoz Nyílhegyek vonalak. Attribútum folyamat másolással vagy között egyik rendszerből adatok átalakítása folyamatán, és az összes attribútum flow (bejövő és kimenő).

Attribútum munkafolyamat esetén a összekötő szóköz és a metaverse között két irányban szinkronizálási (teljes vagy delta) műveletek mikorra van ütemezve.

Attribútum munkafolyamat csak akkor jelenik meg, ezeket a szinkronizálás futtatásakor. Attribútum flow szinkronizálási szabályok határozzák meg. Ezek a bejövő (ISR a fenti képen) és a kimenő (OSR a fenti képen) is lehet.

## <a name="connected-system"></a>A kapcsolódó rendszerre

Csatlakoztatott rendszer (más néven csatlakoztatott könyvtár) a távoli rendszer szinkronizálási csatlakozik Azure AD Connect hivatkozó és olvasása, és személyes adatok írása, az.

## <a name="connector-space"></a>Összekötő terület

Minden csatlakoztatott adatforrás megfelelője objektumok és attribútumok az összekötő helyen szűrt részét.
A szinkronizálási szolgáltatás nincs szükség esetén az objektumok szinkronizálása, lépjen kapcsolatba a távoli rendszer helyileg működtetéséhez így és korlátozza az import beavatkozásra és csak exportálja.

Esetén az adatforrás és az összekötő az azt jelenti, hogy a szükséges módosításokat (delta importálása) listája, majd a működési hatékonyság megnő, csak a módosításokat, mivel az utolsó lekérdezési ciklus továbbított. Az összekötő területről a csatlakoztatott adatforrás módosítható, hogy az összekötő ütemezés importálása és exportálása kötötten automatikusan propagálása insulates. A hozzáadott biztosítási ruházza fel a külön foglalkozni tesztelés, megtekintése vagy arról, hogy a következő frissítés közben.

## <a name="metaverse"></a>Metaverse

A metaverse az összevont nézet összes illesztett azonosítók a szomszédos összekötő szóközöket.

Egymáshoz kapcsolódó identitások és hitelesítésszolgáltató az importálási folyamat hozzárendelések keresztül különféle attribútumok van-e hozzárendelve, a közép metaverse objektum elkezdi összesített adatok több rendszerekből. Ebben az objektum attribútum adatfolyamban a hozzárendelések végrehajtása kimenő információt.

Objektumok egy mérvadó rendszer projektek őket a metaverse be jönnek létre. Amint a minden kapcsolat eltávolítása a metaverse objektum törlődik.

A metaverse objektumainak közvetlenül nem szerkeszthetők. Az objektum az összes adatot kell kell járult attribútum folyamat keresztül. A metaverse mindegyik összekötő szóközzel állandó összekötők kezeli. Ezek az összekötők minden-szinkronizálás futtatása nem szükséges sor. Ez azt jelenti, hogy Azure AD Connect szinkronizálási nem találja a megfelelő távoli objektum minden alkalommal, amikor. Ezzel elkerülhető módosításainak megakadályozására, amely általában felelős használatával történik az objektumok attribútumainak költséges ügynökök van szükség.

Lehetnek a korábban létrehozott olyan objektumok, amelyek kell kezelni az új adatforrások felfedezése, amikor a Azure AD Connect szinkronizálási neve illesztés szabály folyamatot használ, amelyhez kapcsolatot potenciális jelöltek ki szeretné számítani.
Miután a kapcsolat létrejött, a kiértékelés indítást, és a normál attribútum folyamat akkor fordulhat elő, a távoli csatlakoztatott adatforrás és a metaverse között.

## <a name="provisioning"></a>Kiépítése

Ha egy mérvadó forrás projektek az összekötő terület új objektum metaverse be egy új objektum lefelé irányuló csatlakoztatott adatforrás, amely egy másik Connector hozhat létre.

Eleve létesít egy hivatkozást, és attribútum adatfolyam két irányban folytatható.

Amikor a szabály azt határozza meg, hogy egy új összekötő terület objektum kell létrehozni, ennek neve kiépítési. Jó helyen jár mivel ez a művelet csak a történik az összekötő terület belül, azt nem kerülnek át az csatlakoztatott adatforrás mindaddig, amíg megtörténik az exportálás.

## <a name="additional-resources"></a>További források

* [Azure Active Directory csatlakozás szinkronizálása: A szinkronizálás beállításainak testreszabása](active-directory-aadconnectsync-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
