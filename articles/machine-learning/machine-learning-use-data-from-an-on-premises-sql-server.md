<properties
pageTitle="Egy helyszíni SQL Server-adatbázisból származó adatok használata a gépi tanulási |} Azure"
description="Az Azure gépi tanulási fejlett analitikai végrehajtásához használja egy helyszíni SQL Server-adatbázisból származó adatok."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Speciális elemzések végzése az Azure gépi tanulási egy helyszíni SQL Server-adatbázis adatainak használata


Gyakran a helyszíni adatokra nagyvállalatoknak szeretné venni a skála előnyeit és az illető gépi tanulási munkaterhelésekből felhőszolgáltatások agility. De azok nem szeretné, hogy az aktuális üzleti folyamatok és a munkafolyamatok megszakíthatja a helyszíni adatok áthelyezésével, a felhőbe. Azure gépi tanulási támogatja az adatok beolvasása egy helyszíni SQL Server-adatbázisból, majd képzés és ezekkel az adatokkal modell pontozási. Nincs már kézi másolja a vágólapra, és a felhő és a helyszíni kiszolgálói között az adatok szinkronizálása. Ehelyett az **Adatok importálása** a modul Azure gépi tanulási Studio most erről közvetlenül a helyszíni SQL Server-adatbázis a képzés és a feladatok pontozási. 

Ebből a cikkből megtudhatja, hogy miként szeretné bejövő adatok helyszíni SQL server-adatok Azure gépi tanulási be. Azt feltételezi, hogy már jól ismert Azure gépi tanulási fogalmak, például a *munkaterületek, modulok, adatkészleteket, kísérletek stb*.

> [AZURE.NOTE] Ez a funkció nem érhető el az ingyenes munkaterületek. Gépi tanulási árak és rétegek kapcsolatos további tudnivalókért olvassa el az [Azure gépi tanulási árak](https://azure.microsoft.com/pricing/details/machine-learning/)című témakört.

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>A Microsoft adatkezelési átjáró telepítése

Az Azure gépi tanulási töltse le és telepítse a Microsoft adatkezelési átjáró kell egy helyszíni SQL Server-adatbázis eléréséhez. Az átjáró kapcsolat gépi tanulási Studio konfigurálásakor is töltse le és telepítse az átjárót, az alábbiakban ismertetett **letöltése és a külső.FÜGGV adatok átjáró** párbeszédpanelen lehetőséget.

Is telepítheti az adatkezelési átjáró időszakokra le, és az MSI-telepítés csomag fut a [Microsoft letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=39717). Válassza a legújabb verzióját, jelölje ki a 32 bites vagy 64 bites tetszés szerint a számítógépen. Az MSI is használható egy meglévő az adatkezelési átjáró a legújabb verzióra frissíteni megőrzi az összes beállításokkal.

Az átjáró előfeltételei a következők:

- A támogatott Windows operációs rendszer csak Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012-ben és a Windows Server 2012 R2.
- Az ajánlott konfiguráció a az átjárót futtató számítógépen legalább 2 GHz-es, 4 magmintákat, 8 GB RAM, de 80 GB szabad.
- Ha az állomásgép hibernálás az átjáró nem adatok kérelmekre válaszolni. Ezért állítsa be a megfelelő energiaséma azon a számítógépen az átjáró telepítése előtt. Az átjáró telepítési üzenetet jeleníti meg, ha be van állítva a gép hibernálás.
- Másolás tevékenység meghatározott gyakorisággal fordul elő, mert az Erőforrás kihasználtsága (Processzor, memória) a számítógépen is követi a csúcs és üresjárati időpontokkal azonos minta. Erőforrás-kihasználtság is függ erősen áthelyezése adatok mennyiségét. Ha több másolási feladatok előrehaladását erőforrásigények fogja csúcs időszakokban részletezésből erőforrás-kihasználtság. Noha a fent felsorolt minimális konfiguráció értelemben elegendő, érdemes az további erőforrások, mint a minimális konfiguráció attól függően, hogy az adatok mozgás az adott betöltése a konfiguráció van.

Vegye figyelembe az alábbiakat, ha beállításával és használatával az adatkezelési átjáró:

- Az adatkezelési átjáró csak egy példánya egyetlen számítógépre telepíthető.
- Több helyszíni adatforrások egyetlen átjáró is használhatja.
- Több átjáró a különböző számítógépeken ugyanazt a helyszíni adatforráshoz lehet csatlakozni.
- Egyszerre csak egy munkaterület átjárót konfigurálja. Átjárók nem lehet megosztani, munkaterületek keresztül adott időben.
- Beállíthatja, hogy egyetlen munkaterületre vonatkozó több átjáró. Például érdemes, amelyhez csatlakozik a fejlesztés során próba-adatforrások átjárók, és munkakörnyezeti az átjárók használhatók, amikor készen áll a üzemeltető.
- Az átjáró nem kell az adatforrás ugyanazon a gépen, de az adatforrás közelebb kapcsolat csökkenti az idő az átjáró csatlakozni az adatforráshoz. Azt javasoljuk, hogy az átjáró telepítette egy számítógépre, azzal, hogy az átjáró és az adatforrás nem versenyt az erőforrások a helyszíni adatforrás üzemeltető eltérő.
- Ha már telepítve van a számítógépén a Power BI vagy Azure Data Factory esetek felszolgálásához az átjárók, telepítse külön az átjárók Azure gépi tanulási egy másik számítógépen. 

    > [AZURE.NOTE] Ugyanazon a számítógépen az adatkezelési átjáró és a Power BI átjáró nem futtathatók.

- Akkor használja az adatkezelési átjáró az Azure gépi tanulási, még akkor is, ha a készült Azure ExpressRoute használja az egyéb adatok. Az adatforrás kezelje egy helyszíni adatforrás (tűzfal mögött van), még ha használja készült ExpressRoute, és gépi tanulási és az adatforrás közötti kapcsolatot létrehozni az adatkezelési átjáró segítségével. 

Részletes információt találhat telepítési előfeltételek, telepítés lépéseit és hibaelhárítási tippek az [adatok helyszíni adatforrások és az adatkezelési átjáró a felhőbeli közötti áthelyezése](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), a cikkben kezdve az [adatkezelési átjáró használatával kapcsolatos szempontok](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway)csoportban.

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Bejövő adatok adatainak az Azure gépi tanulási a helyszíni SQL Server-adatbázishoz


Az útmutató használni egy Azure gépi tanulási munkaterületen az adatkezelési átjáró beállítása, állítsa be, és majd olvassa el az adatok egy helyszíni SQL Server-adatbázisból.

> [AZURE.TIP] Mielőtt nekikezdene, tiltsa le a böngésző előugróablak- `studio.azureml.net`. A Google Chrome böngészőt használ, töltse le és telepítse a több bővítmény a Google Chrome webáruházba [Kattintson egyszer alkalmazás bővítmény](https://chrome.google.com/webstore/search/clickonce?_category=extensions)egyik.

### <a name="step-1-create-a-gateway"></a>Lépés: 1: Átjáró létrehozása

Első lépésként hozhat létre, és állítsa be az átjáró a helyszíni SQL-adatbázis eléréséhez.

1. Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/Home/) , és válassza ki a munkaterület, amely használni kívánt.

2. Kattintson a **Beállítások** lap, a bal oldalon, és kattintson az **Adatok ÁTJÁRÓK** lap tetején.

3. Kattintson az **Új adatok ÁTJÁRÓ** a képernyő alján.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. Az **új adatok átjáró** párbeszédpanelen írja be az **Átjáró neve** , és tetszés szerint adjon meg egy **leírást**. Kattintson a jobb oldali alsó nyissa meg a következő lépés a konfiguráció melletti nyílra.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. Töltse le és a külső.FÜGGV adatok átjáró párbeszédpanelen másolja a vágólapra az ÁTJÁRÓ regisztrációs BILLENTYŰT.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Ha még nincs letöltve, és telepítve van a Microsoft adatkezelési átjáró, majd kattintson a **Letöltés**gombra az adatkezelési átjáró. Ekkor megjelenik a Microsoft Download Center hol jelölje ki a átjáróverzió van szüksége, töltse le, és telepítse azt. Telepítési előfeltételek, telepítés lépéseit és hibaelhárítási tippeket részletes információt találhat az [adatok helyszíni adatforrások és az adatkezelési átjáró a felhőbeli közötti áthelyezése](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)cikk elején szakaszokban.

7. Az átjáró telepítése után az adatkezelési átjáró konfigurációkezelőjének nyílik meg, és az **átjáró regisztrációs** párbeszédpanel jelenik meg. Illessze be a vágólapra másolt **Átjáró regisztrációs kulcsa** , és kattintson a **Regisztrálás**gombra.

8. Ha már telepítve van az átjárók, futtassa az adatkezelési átjáró konfigurációkezelőjének kattintson **módosítása hivatkozásra**, illessze be a vágólapra másolt  **Átjáró regisztrációs kulcsa** , és kattintson az **OK gombra**.

9. A telepítés befejeződött, a Microsoft adatkezelési átjáró konfigurációkezelőjének **regisztrálni az átjárót** párbeszédpanel jelenik meg. Beillesztése a másolt a vágólapra a fenti ÁTJÁRÓ regisztrációs BILLENTYŰT, és kattintson a **Regisztrálás**gombra.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Az átjáró konfiguráció befejeződött, ha az alábbi értékekre vannak beállítva a **Kezdőlap** lapon a Microsoft adatkezelési átjáró konfigurációkezelőjének:

    - **Átjáró neve** és a **példány nevét** az átjáró neve vannak beállítva.

    - **Regisztráció** **regisztrálva**állapotérték szerepel.

    - **Állapota** mező állapotértéke **Elindítva**.

    - A képernyő alján az állapotsor **csatlakoztatva az adatokat adatkezelési átjáró felhőalapú szolgáltatás** egy zöld pipa együtt jeleníti meg.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure gépi tanulási Studio is frissítik a regisztráció sikeres esetén.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. **Töltse le és adatok átjáró regisztrálni** párbeszédpanelen kattintson a jelölőnégyzet be van jelölve a beállítási folyamat befejezéséhez. A **Beállítások** lap az átjáró állapota "Online" jeleníti meg. A jobb oldali ablaktáblában állapotát és egyéb hasznos információkat talál.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. A Microsoft adatkezelési átjáró konfigurációkezelőjének Váltás a **tanúsítvány** fülre. A tanúsítvány megadott ezen a lapon a helyszíni adatok tárolóhoz a portálon megadott hitelesítő adatok titkosítás/visszafejtés szolgál. Ez az alapértelmezett generált tanúsítvány. A Microsoft javasolja módosítás saját tanúsítvány, amely a biztonsági másolatot készíteni a tanúsítvány kezelési rendszerben. Kattintson a **Módosítás** saját tanúsítványt használja helyette.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (nem kötelező) Ha azt szeretné, annak érdekében, hogy az átjáró problémáinak részletes naplózás engedélyezése, a a Microsoft adatkezelési átjáró konfigurációkezelőjének kattintson a **Diagnosztika** fülre, és a **hibaelhárítási célból részletes naplózás engedélyezése** választógombot. A naplóadatokat a Windows eseménynaplójának az **alkalmazások és szolgáltatásnaplók** csoportban található - &gt; **Az adatkezelési átjáró** csomópontot. A **diagnosztikai** lap segítségével ellenőrizze az átjáró a helyszíni adatforrás-kapcsolatot.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Ez akkor egészíti ki az átjáró beállítása az Azure gépi tanulási folyamat.
Most már készen áll a helyszíni adatok használatára.

Hozzon létre, és állítsa be az egyes munkaterületeken Studio több átjáró. Például az átjárók, amelyet a próba-adatforrások csatlakoztatása fejlesztés alatt, és egy másik átjáró előfordulhat, hogy van a termelési adatforrások. Azure gépi tanulási függően a vállalati környezetben, több átjáró beállítása a rugalmasságot biztosít. Jelenleg nem oszthatnak meg átjárókat munkaterületek között, és csak egy átjáró egyetlen számítógépre telepíthető. Az átjáró telepítésekor további kapcsolatos szempontok lásd: [az adatkezelési átjáró használatával kapcsolatos szempontok](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) [adatok helyszíni adatforrások és az adatkezelési átjáró a felhőbeli közötti áthelyezése](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)a következő cikket.

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Lépés: 2: Olvassa el az adatok helyszíni adatforrásból listában az átjáró segítségével

Az átjáró beállítása után az **Adatok importálása** modul vehet egy kísérlet, amely a bemeneti az adatokat a helyszíni SQL Server-adatbázisból.

1.  A gépi tanulási Studióban a **kísérletek** lapon jelölje ki, válassza a **+ Új** bal alsó sarokban, és jelölje ki az **Üres kísérlet** (vagy válasszon egyet a rendelkezésre álló több minta kísérletek).

2.  Keresse meg és húzza az **Adatok importálása** a modul a kísérlet vásznat.

3.  A vászonra alatt kattintson a **Mentés másként** gombra. Írja be a "Azure gépi tanulási helyszíni SQL Server oktatóprogram" a kísérlet jelölőnégyzetét, jelölje ki a munkaterület, és kattintson az **OK gombra** .

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Kattintson az **Adatok importálása** modulra, jelölje ki, majd a **Tulajdonságok** ablaktábla jobb oldalán a vászonra, válassza az **adatforrás** legördülő lista "Helyszíni SQL-adatbázis".

5.  Jelölje ki az **adatok átjáró** telepítve, és regisztrálva. A telepítő egy másik átjáró "(Hozzáadás új adatok átjáró...)" kiválasztásával.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Írja be az SQL- **adatbázis-kiszolgáló neve** és **az adatbázis neve**, az SQL- **adatbázis-lekérdezés** végrehajtani kívánt együtt.

7.  Kattintson a **Enter értékeket** a **felhasználónevet és jelszót** , és írja be az adatbázis hitelesítő adatait. Windows-hitelesítés és az SQL Server-hitelesítés attól függően, hogy a helyszíni SQL Server-beállításaitól is használhatja.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Zöld pipa az "szükséges értékek" üzenet "értékek megadása" változik. Csak egyszer kell megadniuk a hitelesítő adatok kivéve, ha az adatbázis adatai vagy a jelszó módosítása. Azure gépi tanulási azt a tanúsítványt, feltéve, hogy az átjáró titkosítsa a hitelesítő adatokat a felhőben való telepítésekor használja. Azure soha ne tárolja a helyszíni hitelesítő adatok titkosítás nélkül.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  A kísérlet futtatásához a **Futtatás** gombra.

Miután befejeződött a kísérlet fut, akkor is ábrázolása, kattintson az **Adatok importálása** modul a kimeneti port és válassza a **Megjelenítés**az adatbázisból importálta az adatokat.

Miután végzett a kísérlet fejlesztése, telepítése, és a modell üzemeltető. A köteg végrehajtás szolgáltatást használja, az az **Adatok importálása** modul konfigurált helyszíni SQL Server-adatbázisból származó adatok kell beolvassa, és pontozási használható. A helyszíni adatok pontozási használhatja a visszajelzés kérése szolgáltatás, miközben javasoljuk az [Excel programhoz bővítmény](machine-learning-excel-add-in-for-web-services.md) használatával. Jelenleg írása egy helyszíni SQL Server adatbázis **Adatainak exportálása** keresztül nem támogatott a kísérletek vagy a közzétett webes szolgáltatások.

