<properties 
   pageTitle="Egy B2B folyamat létrehozása az Azure alkalmazás szolgáltatás |} Microsoft Azure" 
   description="Hogyan hozhat létre az üzleti-üzleti folyamat áttekintése" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>Egy B2B folyamat létrehozása

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Üzleti eset 
A Contoso és a Northwind két üzleti partnereket. A Contoso (viszonteladótól) megrendelések küld protokollal egy üzleti szintű AS2 például a Northwind (szállító). Adatnézet, ahol a Felhőbeli tárhelyről összes bejövő rendelések tárol. A vételi megrendelések XML-üzenetek e két partnerek között. Amikor az üzenet tárolja a Northwind a felhőbeli tárhelyről majd meg a Northwind belső folyamatok kezelni ettől kezdve a sorrend.
 
Ebben az oktatóanyagban célja, hogyan hozhat létre Northwind keresztül, amely azt is megkapják az üzeneteket (megrendelések az XML-ben) Contoso partnerétől AS2 fölé és majd áll fenn a Felhőbeli tárhelyről üzleti folyamatok létrehozására.


## <a name="capabilities-demonstrated"></a>Igazolni funkciók 
Ebben az oktatóanyagban segít, megjelenítve a következő funkciók: 

- **Üzenet pihenőhelyek**: A viszonteladótól és a szállítók a más platformokon lehet, de a kettő közötti kommunikáció eredmény. Ebben az oktatóanyagban AS2 azokat is kommunikáció (alkalmazási terület utasítás 2). AS2 módja a népszerű átviteli az adatokat az üzleti vállalati verzió kommunikáció kereskedelmi partnerek között.
- **Adatok állandó**: Ha az üzenet megérkezett AS2 fölé, majd a Northwind szeretne további feldolgozása előtt továbbra is fennáll. Összekötő a Felhőbeli tárhelyről üzenetek is használhatja. Ebben az oktatóanyagban Azure BLOB van folyamatban kapcsolatos károkozásra, a felhőbeli tárhelyről a Northwind.
- **Üzleti folyamatok létrehozása**: egy folyamat, több API-alkalmazások is lehet stitched együtt egy üzleti eredmény eléréséhez, ahogyan itt.


## <a name="before-you-begin"></a>Első lépések
Ebben az oktatóanyagban feltételezi Azure alkalmazás Services egyszerű megértése, tudni, hogy miként API-alkalmazások létrehozása és a folyamat összeilleszteni.


## <a name="steps-to-achieve-the-business-scenario"></a>A lépéseket a vállalati forgatókönyv elérése
**Létrehozása és konfigurálása a szükséges API-alkalmazások**

1. Hozzon létre egy példánya az **Azure tároló Blob-összekötő**. Ehhez a hitelesítő adatokat tároló Azure-fiókjába. Győződjön meg arról, hogy készen áll a létrehozásának megkezdése előtt.
2. Hozzon létre egy példányának **BizTalk kereskedési Partner kezelése**. Ehhez a függvény üres SQL-adatbázishoz. Győződjön meg arról, hogy készen áll a létrehozásának megkezdése előtt.
3. Hozzon létre egy példánya az **AS2 összekötő**. Ez a függvény üres SQL-adatbázishoz is szükséges. Győződjön meg arról, hogy készen áll a létrehozásának megkezdése előtt. Ezenkívül szeretné az üzenetek archiválása részeként AS2 feldolgozása, ha, nyújthat hitelesítő adatok az Azure Blob saját létrehozása során.
4. A létrehozott (a partnerek felügyeleti kereskedési) TPM-szolgáltatás beállítása:  
    1. Tallózással keresse meg a TPM szolgáltatás, a fenti lépések részeként létrehozott példányát.
    2. A **partnerek** lehetőséget az *összetevők* és a saját profil **hozzáadása** egy **kontraktor** nevű új partner hozzáadása szükséges AS2 azonosítója.
    3. A **partnerek** lehetőséget az *összetevők* saját profil és a **Northwind** nevű új partner **hozzáadása** a szükséges AS2 identitás hozzáadása.
    4. Használja a Northwind és a Contoso **hozzáadása** egy új AS2 megállapodást a *összetevők* **rendelkezést** parancsot. Northwind lesz a megosztott partner, és a Contoso a vendégként való bekapcsolódáshoz partner. Szükség szerint állítsa be a bejelentkezés, a titkosítást, a tömörítés és a nyugták ezt a szerződést létrehozása során. Abban az esetben, ha tanúsítványokat kell használni, azok tölthetők keresztül a **tanúsítványok** lehetőséget a létrehozott TPM szolgáltatás megnyitásához.


## <a name="create-a-flow--business-process"></a>Hozzon létre egy folyamat / üzleti folyamat
1. Hozzon létre egy új folyamat, amelyben első lépésként AS2. Húzása az **Összekötő AS2** , és válassza a már létrehozott példányt. Válassza ki a kiváltó ok mező a funkcióinak:  
    ![][1]  
2. Ezután húzása **Azure tároló Blob-összekötő** , és válassza a már létrehozott példányt. Válassza ki a műveletet a funkcióinak és belül, amely, jelölje be a kívánt funkciókat **Feltöltése Blob** . Állítsa be megfelelően.
3. Most hozzon létre és üzembe a folyamat.


## <a name="message-processing--troubleshooting"></a>Üzenet feldolgozási és hibaelhárítás
1. A folyamat, hogy telepítette, tesztelésre. XML-(szerint a AS2 megállapodás előbb létrehozott) a tördelt AS2 üzenetek küldése az AS2 végpontot, hogy megjelentek által létrehozott AS2Connector-példányt. Előfordulhat, hogy a hitelesítés a végpont beállítása, úgy, hogy nyilvánosan elérhető.
2. A folyamat végrehajtás információt tallózással megkeresi a folyamat, és be az áramlás-példányt, amelyeket végrehajtva használ majd verziószámú van jelenik meg
3. AS2 feldolgozás információt tallózással keresse meg az érintett AS2Connector példányt, utána pedig kövesse a nyomon követés részére történő verziószámú szerint. Az érintett van szükség információk megtekintésének korlátozása szűrőket is használhatja.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
