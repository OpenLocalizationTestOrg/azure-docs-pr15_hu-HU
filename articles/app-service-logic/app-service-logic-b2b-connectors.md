<properties 
    pageTitle="Üzleti vállalati verzió összekötők és az API-alkalmazások az összefüggés-alkalmazások |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre és állíthat szerkesztése, EDIFACT, AS2 és TPM összekötők; microservices architektúra" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Üzleti vállalati verzió összekötők és az API-alkalmazások

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logika alkalmazások számos BizTalk API alkalmazásokkal kapcsolatos beállításokhoz elengedhetetlen integrációs környezetekbe tartalmazza. Az alkalmazások API fogalmak és BizTalk kiszolgálón belül használható eszközök alapján, de érhető el logika alkalmazások részeként. 

Egy kategóriába tartozó API alkalmazások a vállalati verzió (B2B) API-alkalmazásokat. B2B API az alkalmazások használata esetén is egyszerűen partnerek hozzáadása, rendelkezést létrehozása, és végezze el minden közben szeretné a helyszíni szerkesztése, AS2 és EDIFACT.  

B2B API alkalmazások "Eseményindító" és "Művelet" lehetőségeket kínálnak. Az eseményindító elindítja egy új példányát, egy adott esemény, például egy X12 érkezését alapján partnertől üzenetet. Művelet az eredmény, például egy X12 fogadása után jelenik meg, majd küldje el az üzenetet AS2 használatával.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Mit nevezünk egy üzleti vállalati verzió összekötő és az API-alkalmazások
A vállalati verzió (B2B) szolgáltatás különböző vállalatok, részlegek, alkalmazásokat, és így tovább AS2, szerkesztése és EDIFACT kommunikáció engedélyezése a meglévő, beépített API alkalmazások tartalmazza. 

Az B2B API-alkalmazásokat tartalmazza: 

Összekötő és az API-alkalmazások | Leírás
--- | ---
BizTalk kereskedési partnerek kezelése | API alkalmazás, amely a vállalati verzió (B2B)-kapcsolatok partnerek és rendelkezést hoz létre. Ezek a kapcsolatok csatlakozást, a AS2 EDIFACT és X12 Protocol (protokoll).<br/><br/>A TPM API-alkalmazást az alap követelmény az AS2 összekötő és a X12 vagy EDIFACT API-alkalmazások. 
AS2 összekötő | Egy összekötőre, fogadása és a AS2 átviteli az üzenetek küldése. Az összekötő alapú átvitel esetében: adatok biztonságosan, és biztos, hogy az interneten keresztül.
BizTalk EDIFACT | Az API-alkalmazásokban, fogadása és EDIFACT az üzenetek küldése. EDIFACT UN/EDIFACT (ENSZ/elektronikus adatok Interchange For Administration, kereskedelmi integráció és átviteli) is gyakran nevezik, és különböző ágazatok sokan használják.
BizTalk X12 | Olyan API-alkalmazást, fogadása és a X12 az üzenetek küldése Protocol (protokoll). X12 ASC X 12 (akkreditált szabványok Bizottság X12) is gyakran nevezik és elterjedt ágazatok keresztül. 


API az alkalmazások használata esetén elvégezhető feladatok üzenetküldés különböző szerkesztése. Például a AS2 connector használatakor, akkor biztonságosan vezérlőn és különböző típusú üzenetek küldése (sima szerkesztése, XML-fájlt, és így tovább) egy ügyfél, a vállalaton belül egy hányados például emberi erőforrások vagy bárki AS2 használó. 

Annyi API-alkalmazások, amennyit kíván, és hozhat létre, hanem egyszerűen hozhat létre. Egyetlen API-alkalmazás belül több esetek vagy munkafolyamatok is felhasználhat.

Ezt megteheti a programozás nélkül. Lássunk hozzá. 


## <a name="requirements-to-get-started"></a>Első lépések vonatkozó követelmények
Ha B2B API-alkalmazások hoz létre, akkor néhány szükséges erőforrások. Ezeket az elemeket kell létrehoznia, mielőtt is használható más API-alkalmazásokból. Ezekről a követelményekről a következők: 

Követelmények | Leírás
--- | ---
Azure SQL-adatbázis | B2B elemekhez, beleértve a partnerek, a sémák, tanúsítványok és agreeements tárolja. A saját Azure SQL-adatbázis szükséges az egyes B2B API alkalmazások. <br/><br/>**Megjegyzés:** Másolja a vágólapra a kapcsolati karakterláncot az adatbázishoz.<br/><br/>[Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md)
Azure Blob-tárolóhoz tároló | Tárolók tulajdonságok jelenik meg, amikor AS2 archiválása engedélyezve van. Ha már nincs szükség az archiválás AS2 üzenet, tároló tároló nincs szükség. <br/><br/>**Megjegyzés:** Ha engedélyezi az archiválás, másolja a kapcsolati karakterlánc a Blob-tárolóhoz.<br/><br/>[Azure tároló fiókokról](../storage/storage-create-storage-account.md)
Szolgáltatás Bus Namespace és az kulcs értékeit. | X12 és EDIFACT kötegelés adatokat tárolja. Ha már nincs szükség kötegelés, szolgáltatás Bus névteret nincs szükség.<br/><br/>**Megjegyzés:** Ha engedélyezi a kötegelés, másolja a ezeket az értékeket.<br/><br/>[A szolgáltatás Bus Namespace létrehozása](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM példány | Hozzon létre egy AS2 összekötő és X12 vagy EDIFACT API-alkalmazás egy BizTalk kereskedési Partner Management (TPM) példány szükséges. A TPM API-alkalmazás létrehozásakor a TPM példány készít. <br/><br/>**Megjegyzés:** Tudnivalók a TPM API-alkalmazás nevére. 


## <a name="create-the-api-apps"></a>Az API-alkalmazások létrehozása
Az Azure portálon vagy a REST API-k használata B2B API-alkalmazások hozhat létre. 


### <a name="create-the-api-apps-using-rest-apis"></a>A REST API-k használata API-alkalmazás létrehozása
[Nézze meg a dokumentáció a REST API-k használata.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>A B2B API-alkalmazások létrehozása az Azure-portálon
Az Azure-portálon B2B API-alkalmazások logika alkalmazások, a Web Apps alkalmazások és a Mobile-alkalmazások létrehozásakor is létrehozhat. Vagy egy saját lap használatával hozhat létre. Mindkét módon is egyszerűen, így ez attól függ, az igényeinek, vagy a beállítások. Néhány felhasználó előnyben részesíti az adott tulajdonságaik a B2B API alkalmazások létrehozásához először. Ezután a logika alkalmazások/alkalmazások/Mobile webalkalmazások létrehozására, és a létrehozott B2B API-alkalmazások felvétele.  

Az alábbi lépéseket a B2B API alkalmazások használata az API-alkalmazások lap létrehozása


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>A partnerek felügyeleti (TPM) API-alkalmazások kereskedési BizTalk létrehozása

> [AZURE.NOTE] Hozzon létre egy AS2 összekötő és X12 vagy EDIFACT API-alkalmazás egy BizTalk kereskedési Partner Management (TPM) példány szükséges. A TPM API-alkalmazás létrehozásakor a TPM példány készít.

Az alábbi lépéseket az TPM hozza létre:

1. Az Azure-portálon Startboard (kezdőlap) jelölje ki a **piactérről**. A meglévő API-alkalmazások és összekötők **API-alkalmazások** listája Is **keresési** -B2B API az adott alkalmazást.
2. Válassza a **partnerek felügyeleti kereskedési BizTalk**. Válassza az új lap **létrehozása**lehetőséget. 
3. Adja meg az a tulajdonságokat: 

    A tulajdonság | Leírás
--- | ---
név | Írja be a TPM példány egy tetszőleges nevet. Ha például tetszés szerinti nevet adhat meg *AccountsPayableTPM*.
Csomagbeállítási | Írja be a ADO.NET- **Adatbázis kapcsolat-karakterlánc** létrehozott Azure SQL-adatbázishoz. <br/><br/>A kapcsolati karakterlánc másolásakor a jelszó nem bekerül a kapcsolati karakterlánc. Ügyeljen arra, hogy a kapcsolati karakterlánc írja be a jelszót.
Alkalmazás szolgáltatáscsomagja | A fizetési csomag listája. Ha több vagy kevesebb erőforrások módosíthatja.
Réteg árak | Írásvédett tulajdonságot, amely felsorolja a belül az Azure előfizetés árak kategóriára. 
Erőforráscsoport | Hozzon létre egy új vagy meglévő csoport használata. Az összes API-alkalmazások és a logika alkalmazások, a Web Apps alkalmazások és a Mobile-alkalmazások összekötők az azonos erőforráscsoport kell lennie. <br/><br/>[Erőforrás-csoportok használatával](../azure-resource-manager/resource-group-overview.md) bemutatja ezt a tulajdonságot. 
Előfizetés | Írásvédett tulajdonságot, amely felsorolja a jelenlegi előfizetését.
Hely | Az Azure szolgáltatás üzemelteti földrajzi helyét. 
Startboard hozzáadása | Akkor válassza ezt a B2B API-alkalmazás hozzáadása a hajó (kezdőlap).

4. Jelölje ki a **létrehozása**. 

Miután létrehozta a TPM API-alkalmazás (TPM példány), AS2 kapcsolatra, illetve a X12 vagy EDIFACT API-alkalmazások majd létrehozhat. 


#### <a name="create-the-as2-connector"></a>Az AS2 összekötő létrehozása

1. Az Azure-portálon Startboard (kezdőlap) jelölje ki a **piactérről**. A meglévő API-alkalmazások és összekötők **API-alkalmazások** listája Is **keresési** -B2B API az adott alkalmazást.
2. Jelölje ki a **AS2 összekötőt**. Válassza az új lap **létrehozása**lehetőséget. 
3. Adja meg az a tulajdonságokat: 

    A tulajdonság | Leírás
--- | ---
név | Írja be a AS2 összekötő egy tetszőleges nevet. Ha például tetszés szerinti nevet adhat meg *AS2Connector*.
Csomagbeállítási | Adja meg a megadott beállításokat, hogy API-alkalmazásra, például a TPM példány nevét. <br/><br/>Ez a témakör az adott tulajdonságok [Hozzáadása AS2 Csomagbeállítási](#AddAS2Conn) megtekintése 
Alkalmazás szolgáltatáscsomagja | A fizetési csomag listája. Ha több vagy kevesebb erőforrások módosíthatja.
Réteg árak | Írásvédett tulajdonságot, amely felsorolja a belül az Azure előfizetés árak kategóriára. 
Erőforráscsoport | Hozzon létre egy új vagy meglévő csoport használata. [Erőforrás-csoportok használatával](../azure-resource-manager/resource-group-overview.md) bemutatja ezt a tulajdonságot. 
Előfizetés | Írásvédett tulajdonságot, amely felsorolja a jelenlegi előfizetését.
Hely | Az Azure szolgáltatás üzemelteti földrajzi helyét. 
Startboard hozzáadása | Akkor válassza ezt a B2B API-alkalmazás hozzáadása a hajó (kezdőlap).

    **<a name="AddAS2Conn"></a>AS2 összekötő Csomagbeállítási**

    A tulajdonság | Leírás
--- | --- 
Adatbázis kapcsolat-karakterlánc | Írja be a ADO.NET-kapcsolati karakterláncot az Azure SQL-adatbázis létrehozott. A kapcsolati karakterlánc másolásakor a jelszó nem bekerül a kapcsolati karakterlánc. Ügyeljen arra, hogy a kapcsolati karakterlánc adja meg a jelszót, mielőtt beillesztené.
Archiválás engedélyezése a beérkező üzenetekhez | Nem kötelező. Engedélyezze ezt a tulajdonságot kapott egy partnernek bejövő AS2 üzenet üzenet tulajdonságainak tárolásához. 
Azure Blob-tároló kapcsolat-karakterlánc  | Írja be a létrehozott Azure Blob-tárolóhoz tároló a kapcsolati karakterlánc. Ha engedélyezve van az archiválás, a kódolt és dekódolt üzenetek tároló tároló tárolja.
TPM példányának neve | Adja meg a **BizTalk kereskedési partnerek kezelése** a korábban létrehozott API-alkalmazás nevét. Az AS2 összekötő hoz létre, ha ez az összekötő végrehajtja a csak az adott TPM példány belül AS2 rendelkezést.

4. Jelölje ki a **létrehozása**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>A X12 és a EDIFACT API-alkalmazás létrehozása

1. Az Azure-portálon Startboard (kezdőlap) jelölje ki a **piactérről**. A meglévő API-alkalmazások és összekötők **API-alkalmazások** listája Is **keresési** -B2B API az adott alkalmazást.
2. Jelölje ki **A BizTalk X 12** vagy **BizTalk EDIFACT**. Válassza az új lap **létrehozása**lehetőséget. 
3. Adja meg az a tulajdonságokat: 

    A tulajdonság | Leírás
--- | ---
név | Írja be a B2B API-alkalmazást egy tetszőleges nevet. Ha például tetszés szerinti nevet adhat meg *EDI850APIApp*.
Csomagbeállítási | Adja meg a megadott beállításokat, hogy API-alkalmazásra, például a TPM példány nevét. <br/><br/>Ez a témakör az adott tulajdonságok [X12 vagy EDIFACT Csomagbeállítási](#AddX12) megtekintése 
Alkalmazás szolgáltatáscsomagja | A fizetési csomag listája. Ha több vagy kevesebb erőforrások módosíthatja.
Réteg árak | Írásvédett tulajdonságot, amely felsorolja a belül az Azure előfizetés árak kategóriára. 
Erőforráscsoport | Hozzon létre egy új vagy meglévő csoport használata. [Erőforrás-csoportok használatával](../azure-resource-manager/resource-group-overview.md) bemutatja ezt a tulajdonságot. 
Előfizetés | Írásvédett tulajdonságot, amely felsorolja a jelenlegi előfizetését.
Hely | Az Azure szolgáltatás üzemelteti földrajzi helyét. 
Startboard hozzáadása | Akkor válassza ezt a B2B API-alkalmazás hozzáadása a hajó (kezdőlap).

    **<a name="AddX12"></a>X12 és EDIFACT API alkalmazások Csomagbeállítási**  

    A tulajdonság | Leírás
--- | --- 
Adatbázis kapcsolat-karakterlánc | Írja be a ADO.NET-kapcsolati karakterláncot az Azure SQL-adatbázis létrehozott. A kapcsolati karakterlánc másolásakor a jelszó nem bekerül a kapcsolati karakterlánc. Ügyeljen arra, hogy a kapcsolati karakterlánc adja meg a jelszót, mielőtt beillesztené.
Szolgáltatás Bus Namespace | Írja be a szolgáltatás Bus névtér hozott létre. Kötelező, csak ha kötegelés engedélyezve van-e. 
Szolgáltatás Bus Namespace megosztott hívóbetű neve | Írja be a szolgáltatás Bus névtér hívóbetű hozott létre. Kötelező, csak ha kötegelés engedélyezve van-e. 
Szolgáltatás Bus Namespace megosztott hívóbetű érték | Írja be a szolgáltatás Bus névtér létrehozott hívóbetű értéket. Kötelező, csak ha kötegelés engedélyezve van-e. 
TPM példányának neve | Adja meg a **BizTalk kereskedési partnerek kezelése** a korábban létrehozott API-alkalmazás nevét. A X12 vagy EDIFACT API-alkalmazás létrehozásakor a API az alkalmazás csak az adott TPM példány belül X12/EDFIACT rendelkezést hajt végre.

4. Jelölje ki a **létrehozása**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>A partnerek, a rendelkezést, a tanúsítványok és a sémák hozzáadása 
Az Azure-portálon nyissa meg az TPM API-alkalmazást. Az **alkatrészek** csoportban adja meg a partnerek, a rendelkezést, a tanúsítványok és a sémák. 

Rendelkezést adja hozzá a AS2 összekötőkhöz, X12 API-alkalmazások és az API-alkalmazások EDIFACT. 


## <a name="monitor-your-api-apps"></a>Az API-alkalmazások figyelése
Az Azure-portálon nyissa meg az TPM API-alkalmazást. A **Műveletek** csoportban megtekintheti a különböző kezelési műveletek. Ha például közül választhat:

- Nézet tájékoztatások és hiba események
- A memória használatát és szál számát a munkafolyamat (w3wp) megtekintése
- Az alkalmazás és a webhely kiszolgálói naplók megtekintése

További a [Monitor a összefüggés-alkalmazásokat](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>Az API-alkalmazások hozzáadása az alkalmazáshoz 
A Microsoft Azure alkalmazás szolgáltatás, amely a következő B2B API-alkalmazások használata másik alkalmazás típusú közzététele. A meglévő B2B API alkalmazások felvétele logika alkalmazások, Mobile-alkalmazások és a Web Apps, vagy hozzon létre újat. 

Belül az alkalmazást egyszerűen elemre kattintva a B2B API-alkalmazások automatikusan hozzáadja őket az alkalmazás.  

> [AZURE.IMPORTANT] Összekötők és a korábban létrehozott API-alkalmazások hozzáadásához a logika alkalmazások, Mobile-alkalmazások vagy létrehozása Web Apps alkalmazások ugyanaz az erőforrás csoportjában található. 

Az alábbi lépéseket a B2B API-alkalmazások felvétele a logika alkalmazások, a Mobile-alkalmazások és a Web Apps alkalmazások: 

1. Az Azure-portálon Startboard (kezdőlap) megnyitásához az **Office áruházat**, és a logika, a mobil vagy az Web Apps alkalmazások keresése. 

    Ha hoz létre egy új alkalmazást, keresse meg a logika alkalmazások, a Mobile-alkalmazások és a Web Apps alkalmazások. Jelölje ki az alkalmazást, és válassza az új lap **létrehozása**. [Egy logikai-alkalmazás létrehozása](app-service-logic-create-a-logic-app.md) felsorolja azokat a lépéseket. 

2. Nyissa meg az alkalmazást, és válassza a **Eseményindítók és a műveletek**. 

3. A **gyűjteményben**jelölje be a B2B API alkalmazást, amely automatikusan hozzáadja az alkalmazását. Új B2B API alkalmazás is létrehozhat.

    > [AZURE.IMPORTANT] Az AS2 összekötő és X12, a EDIFACT API-alkalmazások szükség egy TPM példányt. Ezért ha készít új B2B API-alkalmazásokat, a TPM API-alkalmazás létrehozása első, majd hozza létre az AS2 összekötő X12 API-alkalmazásban, illetve EDIFACT API-alkalmazást. 

4. Válassza az **OK** gombra a módosítások mentéséhez. 

>[AZURE.NOTE] Veheti használatba Azure logika alkalmazások Mielőtt feliratkozna az Azure fiók, [Próbálja meg logika alkalmazások](https://tryappservice.azure.com/?appservice=logic). Létrehozhat egy rövid életű starter logika alkalmazás azonnal. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="more-b2b-resources"></a>További B2B források

[Egy B2B folyamat létrehozása](app-service-logic-create-a-b2b-process.md)<br/>
[A kereskedelmi Partner szerződés létrehozása](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Mik azok az összekötők és BizTalk API-alkalmazások](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>További információ: összefüggés-alkalmazások és Web Apps alkalmazások
[Mik azok a logika alkalmazások?](app-service-logic-what-are-logic-apps.md)<br/>
[Webhelyek és Web Apps alkalmazások Azure App szolgáltatásban](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>További összekötők

[Összekötők és az API-alkalmazások listájának](app-service-logic-connectors-list.md)<br/><br/>
[Mik azok az összekötők és BizTalk API-alkalmazások](app-service-logic-what-are-biztalk-api-apps.md) 
