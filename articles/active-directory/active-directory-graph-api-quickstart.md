<properties
   pageTitle="Quickstart útmutató az Azure Active Directory-grafikonhoz API |} A Microsoft Aure"
   description="Az Azure Active Directory Graph API Azure Active Directory – OData REST API-végpontok programozott hozzáférést biztosít. Alkalmazások a Graph API segítségével végezhet el létrehozása, olvassa el, frissítése és törlése a címtár-adatok és objektumok (CRUD) műveleteket."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Quickstart útmutató az Azure Active Directory-grafikonhoz API

Az Azure Active Directory (AD) Graph API Azure Active Directory – OData REST API-végpontok programozott hozzáférést biztosít. Alkalmazások a Graph API segítségével végezhet el létrehozása, olvassa el, frissítése és törlése a címtár-adatok és objektumok (CRUD) műveleteket. A diagram API segítségével például hozzon létre egy új felhasználót, megtekintése, illetve felhasználó tulajdonságainak módosítása, módosíthatja a jelszavát, jelölje be a csoport tagságát szerepköralapú hozzáférés-, letiltása és a felhasználó törlése. A diagram API-szolgáltatások és -alkalmazás esetek a további tudnivalókért lásd: [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) és [Azure Active Directory Graph API Előfeltételek](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Azure Active Directory Graph API-funkciók [A Microsoft Graph](https://graph.microsoft.io/), egy egyesített API-val, amely tartalmazza az API-khoz más Microsoft szolgáltatásokból, például Outlook, a OneDrive, OneNote, a Csapattervező és az Office Graph elérhető keresztül egy végpontot, és egy egyetlen jogkivonat keresztül is érhető el.

## <a name="how-to-construct-a-graph-api-url"></a>Hogyan Graph API URL-cím létrehozása

Az Graph API annak érdekében, hogy a címtár-adatok és objektumok (más szóval, erőforrások vagy entitás) szemben, amely a CRUD műveleteket szeretne elérni használhat URL-címeit, a Megnyitás adatok (OData-) protokoll alapján. Az URL-címeit, grafikon API-val használt négy fő részből állnak: szolgáltatás a legfelső szintű, a bérlői azonosítóját, a erőforrás elérési utat és a lekérdezési karakterlánc beállítások: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. A példában az alábbi URL-cím vennie: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Szolgáltatás legfelső szintű**: Azure AD-grafikon API, a szolgáltatás legfelső szintű értéke mindig https://graph.windows.net.
- **Bérlői azonosító**: Ez lehet egy igazolt (regisztrált) tartomány neve, akkor a contoso.com a fenti példában. Azt is lehet egy bérlői Objektumazonosító vagy a "myorganiztion" vagy "me" aliast. További tudnivalókért lásd: [megcímezheti személyek és a diagram API műveletek](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Erőforrás elérési útja**: Ez a szakasz egy URL-cím azonosítja az erőforrás kezelni (felhasználók, csoportok, egy felhasználó, vagy egy adott csoport stb.) A fenti példában a legfelső szintű "csoportok" beállítása erőforrás-címre. Egy adott személy is például cím "felhasználók / {objektumazonosító}" vagy "felhasználók/userPrincipalName".
- **A lekérdezés paraméterei**:? a lekérdezés paraméterei szakaszából az erőforrás elérési út részben elválasztja. A "api-verzió" lekérdezési paraméter összes a Graph API-összehívások van szükség. A diagram API is támogatja a következő OData lekérdezési beállítások: **$filter**, **$orderby**, **bontsa ki a $**, **$top**és **$format**. Az alábbi lekérdezés beállítások jelenleg nem támogatottak: **$count** **$inlinecount**és **$skip**. További tudnivalókért lásd: [támogatott lekérdezéseket, a szűrők, és a személyhívó beállításai az Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Diagram API-verziókkal

A verzió Graph API-összehívás esetén a "api-verzió" lekérdezési paraméter ad meg. 1.5-ös vagy újabb verzió egy numerikus érték; használata API-verzió 1,6 =. Korábbi verzióihoz dátum karakterlánc, amely nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik a formátum YYYY-MM-DD; használata Ha például api-verzió 2013-11-08 =. A betekintő szolgáltatásokhoz használja a karakterláncot "béta"; Ha például api-verzió béta =. Graph API-verziók közötti különbségek kapcsolatos további tudnivalókért olvassa el az [Azure Active Directory Graph API verziószámozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning)című témakört.

## <a name="graph-api-metadata"></a>Graph API-metaadat

A "$metadata" szegmens hozzáadása után az URL-CÍMÉT például a bérlői webhely-azonosító visszatérési Graph API metaadat-fájlt, az alábbi URL-cím visszatér metaadat-alapú a bemutató vállalat a Graph Explorer által használt: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Ez az URL a címsorban a webböngészőben tekintheti meg a metaadat-alapú adhat meg. A visszaadott CSDL metaadat-dokumentum a személyek és összetett típusú, tulajdonságaik, és a függvények és a kért Graph API verziója által elérhetővé tett műveletek ismerteti. Az api-verzió paraméter kihagyása, a legújabb verzió metaadatait ad eredményül.

## <a name="common-queries"></a>Általános lekérdezések

[Azure Active Directory Graph API általános lekérdezések](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) kínál az Azure Active Directory Graph, többek között a legfelső szintű forrásokat címtárában használt lekérdezések és a műveleteket a címtárában lekérdezések közös lekérdezések sorolja fel.

Ha például `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` eredménye vállalati címtár contoso.com adatait.

Vagy `https://graph.windows.net/contoso.com/users?api-version=1.6` megjeleníti a címtár-contoso.com minden felhasználó objektumokat.

## <a name="using-the-graph-explorer"></a>A diagram Intézővel

A diagram Intéző az Azure Active Directory Graph API segítségével a címtár lekérdezést, ahogy az alkalmazást.

> [AZURE.IMPORTANT] A diagram Intéző írni, és az adatok törlése egy könyvtár nem támogatja. Csak a diagram Intézővel Azure Active Directory címtárában olvasási műveleteket végezheti el.

A következő beállítás szeretné látni, hogy ha voltak nyissa meg azt a diagram Intézőt, jelölje be a használatát bemutató vállalat, és írja be a kimeneti `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` kattintva jelenítse meg a felhasználók a bemutató címtárban:

![Azure Active Directory graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**A diagram Intéző betöltése**: Ha szeretné tölteni az eszközt, nyissa meg azt a [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Kattintson a **Használatát bemutató vállalat** a Graph Intéző futtassanak adatok a minta bérlői webhelyre. Szükségtelen hitelesítő adatait, és a bemutató vállalat használja. Másik lehetőségként kattintson a **Bejelentkezés** gombra, és az Azure Active Directory fiók hitelesítő adataival jelentkezzen be a diagram Intéző futtassanak a bérlő. Graph Explorer futtatja a saját bérlői, ha Ön vagy a rendszergazdához kell beleegyezés bejelentkezés során. Ha Office 365-előfizetéssel, automatikusan rendelkeznek egy Azure AD-bérlő. A hitelesítő adatokat, jelentkezzen be az Office 365 használatával, valójában Azure Active Directory-fiókok, és használhatja ezeket a hitelesítő adatokat Graph Intézővel.

**Lekérdezések futtatása**: lekérdezések futtatásához a kérelem szövegmezőbe írja be a lekérdezést, és kattintson a **első** vagy kattintson a kulcs **beírása** . Az eredmények megjelennek a válasz mezőbe. Ha például `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` a bemutató címtárban összes objektumok csoportosítása a felsorolásban.

Vegye figyelembe az alábbi szolgáltatások és a diagram Intéző korlátozások:
- Az automatikus kiegészítés szolgáltatás erőforráson állítja be. Jelenik meg, **Használatát bemutató vállalat** gombra, és kattintson a kérelem szövegdobozt (ahol a vállalat URL-címe megjelenik) gombjára. Kijelölhet egy erőforrás a legördülő listából.

- Támogatja a "me" és "futrinka nevű" megcímezheti aliasok. Használhatja például `https://graph.windows.net/me?api-version=1.6` való visszatéréshez a user objektumban, a bejelentkezett felhasználó vagy `https://graph.windows.net/myorganization/users?api-version=1.6` minden felhasználó vissza az aktuális könyvtár. Figyelje meg, hogy a "me" alias használatával hibát ad vissza a bemutató vállalat mert aláírt a felhasználó nem a kérés.

- Válasz fejlécek szakasz. Ez a lekérdezések futtatásakor jelentkező problémák elhárítására használható.

- A JSON megjelenítő, a válasz kibontás és összecsukás funkciókhoz.

- Nem támogatja a miniatűrök fénykép megjelenítése.

## <a name="using-fiddler-to-write-to-the-directory"></a>Írja be a címtárhoz Fiddler használatával

Quickstart útmutató útmutatóban alkalmazásában használhatja a Fiddler webes Debugger annak érdekében, hogy – a gyakorlás írása"művelet ellen, Azure Active directory. További információ vagy Fiddler telepítse olvassa el a [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Az alábbi példában de kell használni Fiddler webes Debugger "MyTestGroup" új biztonsági csoport létrehozása az Azure Active directory.

**Egy hozzáférési jogkivonat beszerzése**: Azure Active Directory Graph elérését ügyfelek sikerült hitelesíteni először az Azure Active Directory van szükség. További tudnivalókért lásd: [Az Azure Active Directory Authentication esetek](active-directory-authentication-scenarios.md).

**Írása és a lekérdezés futtatása**: hajtsa végre az alábbi lépéseket.

1. Nyissa meg a Fiddler webes Debugger, és kattintson a **Zeneszerző** fülre.
2. Szeretne új biztonsági csoport létrehozása, mivel a HTTP-módszer a legördülő menüből válassza a **bejegyzés gombot** . Csoport objektum műveletek és engedélyek kapcsolatos további tudnivalókért olvassa el a [csoport](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) belül az [Azure Active Directory Graph REST API-hivatkozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)című témakört.
3. A **bejegyzés**melletti mezőbe írja be a kérelem URL-címe a következők: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] A tartomány nevét a saját Azure Active Directory mytenantdomain kell le.

4. A közvetlenül a bejegyzés legördülő alatti mezőbe írja be a következőt:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] Helyette a &lt;a hozzáférési jogkivonat&gt; és a hozzáférési jogkivonat az Azure Active Directory.

5. A **szervezet kérése** mezőbe írja be a következőt:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Csoportok létrehozásával kapcsolatos további tudnivalókért olvassa el a [Csoport létrehozása](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup)című témakört.

További információt a szervezetek Azure Active Directory és grafikon által adattípusa és a rajtuk Graph elvégezhető műveletek információt című témakörben talál [Azure Active Directory Graph REST API-hivatkozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Következő lépések

- További tudnivalók az [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- További tudnivalók az [Azure Active Directory Graph API jogosultsági hatókörök](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
