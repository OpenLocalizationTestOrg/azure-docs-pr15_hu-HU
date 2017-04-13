<properties
   pageTitle="A Power BI beágyazott sor szintű biztonság"
   description="Részletek a Power BI beágyazott sor szintű biztonsága"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>A Power BI beágyazott sor biztonság

Felhasználói hozzáférés korlátozása adott adatait egy jelentés vagy adatkészlet, ugyanazt a jelentést, miközben minden megnéz eltérő adatot használata több különböző felhasználóknak lehetővé tevő sor szintű biztonsági (RLS) használható. A Power BI beágyazott támogatja RLS konfigurált adatkészleteket.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Annak érdekében, hogy RLS előnyeit, fontos, hogy megismeri három fő fogalmak; Felhasználók, a szerepkörök és a szabályokat. Íme a fentiek mindegyike részletesebben:

**Felhasználók** – ezek a tényleges végfelhasználók jelentések megtekintése. A Power BI beágyazott felhasználók azonosított-alkalmazás jogkivonat felhasználónév tulajdonságban.

**Szerepkörök** – felhasználók szerepkörök tartoznak. Szerepkör az szabályok tároló és nevet adhat valamit, amit például "Értékesítés Manager" vagy "Értékesítő". A Power BI beágyazott felhasználók a szerepkörök tulajdonság-alkalmazás jogkivonat különbözteti.

**Szabályok** – szerepkörök szabályok rendelkezik, és ezeket a szabályokat a tényleges szűrőkkel, hogy az adatok vonatkozni. Ez lehet egyszerűen "ország = USA" vagy valami sokkal több dinamikus.

### <a name="example"></a>Példa

Ez a cikk a többi biztosítjuk RLS létrehozása és használata, majd egy beágyazott alkalmazásból más, például fogja. A példában a [Kiskereskedelmi Analysis](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX mintafájl.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

A kiskereskedelmi Analysis minta az összes értékesítés egy adott kiskereskedelmi lánc jeleníti meg. Nélkül RLS, függetlenül attól, hogy melyik körzet felettes bejelentkezik, és megjeleníti a jelentést, hogy megjelenik a ugyanazokat az adatokat. Vezetés meghatározta körzet menedzserek csak kell jelennie a tárolók is kezelheti őket a értékesítéseket, és ehhez RLS használhatja azt.

A Power BI Desktop RLS hozta létre. Amikor megnyitják az adatkészlet és a jelentés, azt lehet váltani a diagram nézetben a séma megtekintéséhez:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Íme néhány dolog, amit érdemes figyelje meg a séma használata:

-   Az összes mértékek, például a **Teljes értékesítés**, a **Sales** ténytábla tárolja.
-   Négy további kapcsolódó dimenzió táblát: **elem**, az **idő**, a **tár**és a **körzet**.
-   A kapcsolati vonalak lévő nyilakra azt jelzik, hogy milyen módon szűrőket is flow egyik táblából a másikba. Például szűrő idő **[dátum]**kerül, ha az aktuális sémában azt volna csak szűrése lefelé az **Értékesítés** táblában található értékeket. Nincs más táblák volna befolyásolhatja a szűrő, mivel az összes a kapcsolati vonalak lévő nyilakra mutasson az értékesítési táblában és nem vagyok a gépnél nem.
-   A **körzet** táblázat jelöli ki a felügyelője minden körzet:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Ez a séma alapján, ha azt szűrő alkalmazása a **Körzet Manager** oszlopban a körzet táblázatban, és a felhasználó a jelentés megtekintése, szűrése egyezéseket, ha a a szűrő is szűrése az **Értékesítés** és **tárolása** a táblák csak jeleníthetők meg, hogy az adott körzet manager lefelé.

Az alábbiakban módját:

1.  A modellezési lapon kattintson a **Szerepkörök kezelése**gombra.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Hozzon létre egy új szerepkört, **Manager**neve.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  Írja be a következő DAX-kifejezés a **körzet** tábla: **[körzet Manager] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Ellenőrizze, hogy a szabályok dolgozik, a **modellezése** lapon kattintson a **nézet, szerepkörök**, és írja be a következő:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    A jelentések letöltése adatait jeleníti meg, ha **Andrew Ma**, volt bejelentkezve.

A szűrő alkalmazása ebben az esetben azt did módja a rendszer kiszűri lefelé a **körzet**, a **tár**és az **Értékesítés** táblában található összes rekordot. Azonban miatt szűrő irányának vonatkozó **értékesítések** és az **idő**közötti kapcsolatok, **Értékesítés** és a **elemre**, és az **elem** és a **idő** táblák program nem szűri le.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Lehetséges, hogy ez a követelmény az ok, azonban ha nem szeretné, hogy menedzserek, amelynek minden értékesítés nincs is elemek megtekintéséhez, azt is kapcsolja be a kétirányú keresztszűrés a kapcsolat, és haladnak a biztonsági szűrő mindkét irányba. Ez az **értékesítési** és viszonya **elem**, jelennek meg szerkesztésével lehet végrehajtani:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Most szűrők is is flow a Sales táblában **elem** táblához:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Megjegyzés:** Ha DirectQuery módban a-adatok használata esetén szüksége lesz a kétirányú-vektoriális az alábbi két lehetőség kiválasztásával szűrés engedélyezése:

1.  **Fájl** -> **Beállítások** -> **Előzetes verzió szolgáltatások** -> **tartományok mindkét irányba DirectQuery-szűrés engedélyezése**.
2.  **Fájl** -> **Beállítások** -> **DirectQuery** -> **korlátlan mérték DirectQuery módban engedélyezi**.


Többet szeretne tudni a kétirányú keresztszűrésről, töltse le a [kétirányú keresztszűrésről az SQL Server Analysis Services 2016-ban és a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) szeretne.

Ez lehet végrehajtani a Power BI Desktop kell, hogy az összes munkáját fussa, de egy további munkákat végezhető el, hogy a RLS kell, hogy a szabályok a Power BI beágyazott definiált munka. Felhasználók hitelesíti és engedélyezi az alkalmazás és a alkalmazás tokenek való hozzáférést, hogy egy adott Power BI beágyazott jelentés felhasználói szolgálnak. A Power BI beágyazott információk ki a felhasználó még nincs. A RLS a munkát kell néhány további környezet átadása az alkalmazás jogkivonat részeként:
-   **felhasználónév** (nem kötelező) – RLS használt: az olyan karakterlánc, amely a felhasználó RLS szabályok alkalmazásakor azonosításához használható. Lásd: a Power BI szintű biztonsági beágyazott sor használata
-   **szerepkörök** – a szerepkörök, jelölje ki a sorban a biztonsági szint szabályok alkalmazásakor tartalmazó karakterlánc. Ha egynél több szerepkör átadása, át kell őket karakterlánc tömbként.

A username tulajdonság esetén szerepkörök, meg kell felelnie legalább egy értéket.

A teljes alkalmazás jogkivonat jelenik meg az alábbihoz hasonló:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Most az összes a csatolt együtt, amikor valaki a jelentés megtekintéséhez az alkalmazásba bejelentkezik azok csak ezentúl tárolhatják szeretné látni, a sor szintű biztonsági által meghatározott adatok megjelenítéséhez.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Lásd még:
[Kiemelt sor szintű biztonsági (RLS)](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
