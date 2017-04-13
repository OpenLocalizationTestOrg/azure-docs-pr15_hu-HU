<properties
   pageTitle="Számlájának ismertetése |} Microsoft Azure"
   description="Megtudhatja, hogy miként áttekinthető és értelmezhető, a használati és Azure előfizetéshez tartozó számla"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/31/2016"
   ms.author="erihur;genli"/>


# <a name="understand-your-bill-for-microsoft-azure"></a>Microsoft Azure számlájának értelmezéséhez

> [AZURE.NOTE] Ha bármely pontján Ez a cikk további segítségre van szüksége, kérjük, [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a probléma megoldódott gyorsan.

A Microsoft Azure-előfizetésekhez költségek ráta terv típusától függően változnak. Például a Visual Studio vállalati (MPN) előfizetők néhány ráta-előfizetések tartalmazzák a havi credits bármely szükségletek Azure szolgáltatás használható.

Figyelje meg, hogy be és az előző számlázási időszak a rejtett használatát a 24 óra lehet jelenteni az aktuális számlaidőszakra a.

Felhasználás és ráta csomagok kapcsolatos további tudnivalókért olvassa el a a [Microsoft Azure vásárlás beállításlapon](https://azure.microsoft.com/pricing/purchase-options/)című témakört.

<!-- The below links cover a complete list of all Microsoft Azure services.

<!-- - [Service Details list (csv1)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
<!-- - [Service Details list (csv2)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)

<!-- *NOTE: The **csv1** link refers to the column header names for csv version 1 and **csv2** link refers to the new column header names for csv version 2.  These files are updated monthly.*-->

### <a name="view-or-download-a-bill-for-microsoft-azure"></a>Megjelenítése, és töltse le a számla készült Microsoft Azure:

1. Jelentkezzen be a [Fiók központban](https://account.windowsazure.com/subscriptions) , a Microsoft-Account vagy szervezeti azonosítójával.

2. Kattintson az előfizetés, amelyben kíváncsi rá részletek és a használatát.

3. Kattintson a **Számlázási előzmények**

    ![Összefoglaló - számlázási előzmények -1](./media/billing-understand-your-bill/ContentViewaBillforMA1.png)

4. A **Számlázási előzmények** szakasz a kimutatások előzetes számlázási időszakokat, valamint az aktuális teljesüléséig időszak sorolja fel. Az aktuális időszakra utasítás kezdve az időt, a becsült jött létre a költségek becsült értéke. Ezt az információt csak naponta frissített, és nem tartalmazhatnak dátum felmerült összes használatát. A havi számla a becsült eltérhetnek.  

    ![Összefoglalás – számlázási előzmények 2](./media/billing-understand-your-bill/ContentViewaBillforMA2.png)

5. Kattintson a **Nézet aktuális utasítás** kezdve az időt, a becsült jött létre a költségek becslése megtekintéséhez. Ezt az információt csak naponta frissített, és nem tartalmazhatnak dátum felmerült összes használatát. Havi számlájának a becsült eltérhetnek.

    ![Összefoglalás – számlázási előzmények 3](./media/billing-understand-your-bill/ContentViewaBillforMA3.png)

    ![Összefoglalás – számlázási előzmények 4](./media/billing-understand-your-bill/ContentViewaBillforMA4.png)

6. Kattintson a **Töltse le a számla** megtekintése az előző számla másolatát.

    ![Összefoglalás – számlázási előzmények 5](./media/billing-understand-your-bill/ContentViewaBillforMA5.png)


> [AZURE.NOTE] A kimutatások számlázás nemzetközi ügyfelek felsorolásában díjak képeken becslés célokra csak bankok számára átváltási különböző költségek van.

Az alábbiakban néhány példa-utasításait a két különböző ajánlatok érhető el a Microsoft Azure.

 Ajánlat típusa | Leírás | Letöltés |
 :--------- |:-------- | :-------|
Kirovó | Havonta fizet az utólag | [Mintafájl](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_ccinvoice_Sample.pdf)
Lekötési ajánlat | Az előre kifizetett lekötési a levont töltött | [Mintafájl](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_invoice_Sample.pdf)

## <a name="account-information"></a>A fiókadatok

A fiók adatok szakasz azonosítja a használatát, és a profilban lényegesek információt.

![Élőfej](./media/billing-understand-your-bill/Header.png)

| Kifejezés                | Leírás                                                                                         |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Számla száma         | A számla egyedi azonosító nyomon követése céljából                                                   |
| Számlázási ciklus       | Az időkeret, amely használatát megtörtént                                                       |
| Számla dátuma        | A számla létrehozott dátum                                                                 |
| Fizetési mód      | A fiók (számla ellenében vagy hitelkártyával) használt fizetési típus                                   |
| A számla             | Microsoft Azure kifizetések cím                                                                    |
| Előfizetés ajánlat  | Típusú előfizetés ajánlat vásárolta (kirovó, BizSpark plusz, Azure fázis stb.) |
| Fiók tulajdonosa e-mailben | A fiók e-mail címét, a Microsoft Azure-fiókjába a regisztrált                      |

## <a name="understand-the-invoice-summary"></a>A számla összesítése ismertetése

A számla **Számla összegzéséhez** szakasza tranzakciók óta az utolsó számla és az aktuális használati költségek foglalja össze.

![számla összegzése](./media/billing-understand-your-bill/InvoiceSummary.png)

Az előző egyenleg kifizetések és nyitott egyensúly a számla szakasza tranzakciók áttekintheti az utolsó számla óta.

| Kifejezés                                              | Leírás                                                                              |
|---------------------------------------------------|------------------------------------------------------------------------------------------|
| Előző egyenleg                                  | A teljes összeg az utolsó számla                                                 |
| Payments                                          | Az utolsó számla alkalmazott kifizetések összesen                                                 |
| Függőben lévő egyenleg (az előző számlázási ciklustól) | Számla beállításokat (credits vagy balances) a fiókjába az utolsó számla óta alkalmazott  |

## <a name="understand-the-current-charges"></a>A jelenlegi terhelések ismertetése
Az aktuális díjak szakaszának anyagjegyzék a havi költségek adatait tartalmazza. A hivatkozások a következő részek vannak tagolva.

| Kifejezés          | Leírás                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Használati költségek | Használati költségek olyan teljes havi költségek, az előfizetést. Vannak az elmúlt hónap használati utólag a számlát kapni.                                                                                                                                                                                                                                                                                                                                       |
| Kedvezményére     | Az aktuális számla alkalmazott szolgáltatás kedvezményére megjelennek a vonal elemre volna is.                                                                                                                                                                                                                                                                                                                                              |
| Korrekciók   | Vegyes korrekciók egyéb credits vagy az aktuális számla nyitott költségekről. Például a Visual Studio Enterprise MSDN ajánlatot, ha látni szeretné a vonal elem a havi hitelképesség. Ha az előfizetés lemondásához meghaladja a ajánlatot, az aktuális számlaidőszakra elején, hogy az előfizetés visszavonás dátumát tartalmazza, a havi hitel havi használati költségek szeretné látni.|

## <a name="footer-information"></a>Élőlábadatok
![élőláb](./media/billing-understand-your-bill/footerinformation.png)

## <a name="understand-the-additional-information"></a>További információk a ismertetése
A további információt a lapra lépve megtudhatja, hogy a számla, és a használat és egyéb fontos adatait a számla megtekintése mutató hivatkozások egyéb erőforrásokra mutató hivatkozásokat.

![További információk](./media/billing-understand-your-bill/AdditionalInformation.png)

### <a name="detailed-usage"></a>Részletes használatát
Hivatkozás a **Részletes használatát** a leírás a fiók központ, ahol megtekintheti a részletes használatát az előfizetéshez tartozó irányítja.  Most már létezik két verziója letölthető: **.csv 1-es verziójú** tartalmazza, a régi elnevezési konvenció és a mezők használatát és a **.csv 2-es verziójú** rövid ügyfélneveket az egyes kategóriákat tartalmaz, valamint a további mezők, amely segít megérteni, hogy milyen szolgáltatások, használja a Microsoft Azure. Ne feledje, hogy .csv 1-es verziójú, hogy nincsenek-e erőforrás-kezelő Azure részletek. Azure erőforrás-kezelő információk találhatók .csv 2-es verziójában.

### <a name="additional-information-and-useful-resources"></a>További információk és hasznos források
Ebben a szakaszban egyszerű kérdésekre számítási példány méretű, az SQL-adatbázis díjak és hasznos hivatkozások kérdések megválaszolásához további segítséget kapcsolatos hivatkozásokat tartalmaz.

| Kifejezés                 | Leírás                                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Eladott              | Ez a számlán a profil címet előre megadott                                                                           |
| Fizetési utasítások | Ez a szakasz küldési ellenőrzések, vezetékes átvitelek vagy egynapos ellenőrzések, ha a fizetési mód számla helyének fizetési utasításait |

## <a name="understand-detailed-usage-charges"></a>Részletes használati költségek ismertetése

A folyamatban lévő lekötési egyszerűen kezelése az Azure használatuk munkában részeként azt már továbbfejlesztett a letöltés használatát fájlt, amely a jelentések az Azure szolgáltatás használatát és a költségeket.  A letöltési hivatkozás két verziója a használatát fájlt tartalmazza:

- **1-es verziójú** a már meglévő formátum használata

- **2-es verziójú** tartalmaz további információkat, és a napi használata című szakaszában oszlopnevei frissíteni.  

Használati költségek kisebb bármely hitelkártya vagy leszámítolás előfizetés teljes **havi** költségek. Vannak az elmúlt hónap használati utólag a számlát kapni.  A fájl felső részén a részletek a szolgáltatásokat, amelyek számlázott az során az előző hónap számlázási ciklusának a jeleníti meg.  A fenti táblázat felsorolja az oszlop minden egyes verzió .csv fájlt.

1-es verzió |  2-es verzió  |  Leírás|
:---------------| :---------------- | --------|
Számlázási időszak | Számlázási időszak | Ha az erőforrás felhasznált számlázási időszakhoz.
név | Kategória állapotjelzője | Azonosítja a legfelső szintű szolgáltatás, amelynek a használatát tartozik.
Típus | Állapotjelzője alkategória | Azure szolgáltatás típusa az oszlopban, amely hatással lehetnek a ráta további definiálhatók.
Erőforrás | Kijelző neve | Az erőforrás-felhasználás alatt álló mértékegységre azonosítja.
Régió | Állapotjelzője terület | Hol található az egyes szolgáltatásokhoz, árú adatközponthoz helye alapján adatközponthoz azonosítja.
RAKTÁRI SZÁM | RAKTÁRI SZÁM | A rendszer egyedi azonosító az egyes Azure erőforrásokhoz azonosítja.
Egység | Egység | Az erőforrás alapdíjával a szolgáltatást a azonosítja. Ha például GB, óra, 10,000s.
Elfogyasztott mennyiség | Felhasznált mennyiség | Az erőforrás, amely a számlázási időszak alatt felhasznált időmennyiséget tartalmazza.
Beépített | Része a mennyiség | Az erőforrás, amely tartalmazza az aktuális számlaidőszakra a költség nélkül időmennyiséget tartalmazza.
Számlázható | Többlet mennyiség | A felhasznált meghaladja a része, ha ez az oszlop különbség jeleníti meg. Vannak, ezt az értéket a számlát kapni. Az ajánlatra vonatkozó részét képező nincs mennyiséggel ajánlatok kirovó a teljes ugyanaz, mint a felhasznált mennyiség lesz.
Lekötési belül | Lekötési belül | Az erőforrás díjakat, amelyek a lekötési összeg, a 6-os vagy 12-hónap ajánlat társított csökken tartalmazza. A erőforrás költségek időrendi sorrendben lekötési összeg csökken.
Pénznem | Pénznem | A pénznem megjelennek az aktuális számlaidőszakra azonosítja.
Hozzárendelések | Hozzárendelések | Az erőforrás díjakat, amelyekre a lekötési mennyiségének a 6-os vagy a 12 havi ajánlat társított tartalmazza.
Lekötési ráta | Lekötési ráta | A teljes lekötési mennyiségének a 6-os vagy 12-hónap ajánlat társított alapján lekötési gyakorisága tartalmazza.
Kamatláb | Kamatláb | Ráta jeleníti meg az előfizetést terhelő számlázható egységnyi mértékét.
Érték | Érték | A ráta oszlop szerint a számlázható oszlop szorzása eredményét jeleníti meg. Ha a felhasznált mennyiség nem haladja meg a mellékelve összeg, lesz ingyenesek ebben az oszlopban.

## <a name="analyze-daily-usage-data"></a>Napi használati adatok elemzése
Attól függően, hogy a használatát ezres napi használatát adatsort is lehet. Szeretne elemezni az adatokat, ha **Használatát letöltése** gombra, és válasszon egy vesszővel tagolt változó fájlhoz (.csv) verziót a napi használati adatok megtekintéséhez megfelelő számlázási időszakhoz.  A hivatkozásnak letöltheti a minta .csv fájl az alábbi tartozó.

 név | Letöltés |
 :----------:| :-------: |
  Részletes használatát .csv 1-es verzió|  [Mintafájl](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
  Részletes használatát .csv 2-es verzió | [Mintafájl](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)



![csv2screenshot](./media/billing-understand-your-bill/csv2screenshot.png)



A .csv fájlban az elemek bontani mennyi minden erőforrás felhasznált az aktuális számlaidőszakra eső listájának megjelenítéséhez.

![CSV-pillanatfelvétel](./media/billing-understand-your-bill/csvsnapshotportal.png)

Az alábbi oszlopokat a számlázási időszak kezdetén a mértékek befolyásoló részletek megjelenítése:

1-es verzió |   2-es verzió   |  Leírás |
:---------------| :----------------| -----|
Használatát dátum | Használatát dátum | A dátum, amikor az erőforrás kibocsátott volt.
név | Kategória állapotjelzője | Azonosítja a legfelső szintű szolgáltatás, amelynek a használatát tartozik.
Erőforrás globálisan egyedi azonosítója | Állapotjelzője azonosító | A számlázott állapotjelzője azonosítója.  A számlázási használatát ár azonosító szolgál.
Típus | Állapotjelzője alkategória | Azure szolgáltatás típusa az oszlopban, amely hatással lehetnek a ráta további definiálhatók.
Erőforrás | Kijelző neve | Az erőforrás-felhasználás alatt álló mértékegységre azonosítja.
Régió | Állapotjelzője terület | Hol található az egyes szolgáltatásokhoz, árú adatközponthoz helye alapján adatközponthoz azonosítja.
Egység | Egység | Az erőforrás alapdíjával a szolgáltatást a azonosítja. Ha például GB, óra, 10,000s.
Elfogyasztott mennyiség | Felhasznált mennyiség | Az erőforrás, amely az adott napra felhasznált időmennyiséget tartalmazza.
Sub terület | Erőforrások helye | Az erőforrás futtató adatközponthoz azonosítja.
Szolgáltatás | Felhasznált szolgáltatás | Ez az oszlop van használni az egyes Azure platform-szolgáltatás, előfordulhat, hogy nem lehet külön azonosítani a Név oszlopban nyomon követéséhez. Ez a szolgáltatás oszlop jelzi, hogy melyik adott szolgáltatás használatát vonatkozik.
A #HIÁNYZIK | Erőforráscsoport | _**Új oszlop hozzáadásával.**_ Az erőforráscsoport, amelyben a telepített erőforrás fut. A hivatkozott [Azure erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md) – áttekintés
Összetevő | Példány azonosítója | A futó erőforrás azonosítója. Az azonosító megadhatja az erőforrás létrehozásakor jött létre automatikusan nevét tartalmazza.
A #HIÁNYZIK | Címkék | _**Új oszlop hozzáadásával.**_ Új erőforrástípus Azure-ban lehetővé teszik címke erőforrásokat. Olvassa el [az Azure erőforrások címkékkel](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) rendszerezéséhez
További információ | További információ | A metaadatok a szolgáltatással kapcsolatos további.
Szolgáltatás adatai 1 | Szolgáltatás adatai 1 | Ebben az oszlopban a projekt nevét, amelyhez tartozik a szolgáltatást biztosít, az előfizetés a.
Szolgáltatás-információ 2 | Szolgáltatás-információ 2 | A mező értéke egy régebbi, amely jellemzi szolgáltatásspecifikus metaadat-alapú nem kötelező.

Mellett, néhány új mezőket és a CSV-fájlok 2-es verziójú névváltozások van fog kell szabványos a adatok formázása az alábbi mezőket:

- **Példány azonosító**: A példány azonosító mezőt jelöl, amely a felhasználó által megadott azonosítója, a kiépítéstől szolgáltatás. Jelenleg nincsenek két formátumokat, amelyben a példány azonosítója jeleníti meg: az erőforrás nevére, vagy teljesen minősített erőforrás-azonosító. Microsoft Azure-szolgáltatások vannak való az példány azonosítóját szabványos teljesen minősített erőforrás-azonosító formában _**(/subscriptions/<subscription id>/resourcegroups/<resourcegroupname>/providers/<providername>/<resourcename>)**_. Szolgáltatások való áttérés az új formátumra, látni fogja a példány azonosítója adatok mező az erőforrás-azonosító a csak az erőforrás nevének módosítása Az erőforrás-azonosító formátuma az előfizetést az erőforrások azonosítása az [Azure erőforrás-kezelő API](https://msdn.microsoft.com/library/azure/dn790567.aspx) segítségével.

![InstanceId](./media/billing-understand-your-bill/instanceid.png)

- **További információ**: A további információ a használatát .csv oszlop szolgáltatásspecifikus metaadat-alapú határozza meg. Ha például egy képtípust egy virtuális. Jelenleg a szolgáltatás bocsát szolgáltatásspecifikus metaadatok több oszlopban: További információ, szolgáltatás Info1 és információ SP2 mezőket. Microsoft Azure-szolgáltatások fog szabványalkotó, csak a további információ oszlopban szolgáltatásspecifikus metaadat-alapú vezérlés.  Lásd: az alatt a szabványos formátum pillanatképét:

![additionalinfo_csv2](./media/billing-understand-your-bill/AdditionaInfo_csv2.png)

- **Címkék**: Ez az oszlop tartalmazza a felhasználó által megadott erőforrás címkék. A címkék csoport számlázási rekordok használható. Címkék segítségével például költségek elosztása az szolgáltatással részleg. További tudnivalók az [Azure erőforrások rendszerezéséhez-címkék használata](../resource-group-using-tags.md). Szolgáltatások, amelyek támogatják a kibocsátás címkék a következők:  

    - Virtuális gépeken futó

    - Tárterület és

    - Hálózati szolgáltatásokkal kiépítve a [Azure erőforrás-kezelő API](https://msdn.microsoft.com/library/azure/dn790567.aspx)

![címkék](./media/billing-understand-your-bill/tags.png)


## <a name="next-steps"></a>Következő lépések

- [A számlázási értesítések beállítása](../billing-set-up-alerts.md)

- [A fizetési módokat kezelése](../billing-how-to-change-credit-card.md)

- [A Microsoft Azure piactéren költségek ismertetése](../billing-understand-your-azure-marketplace-charges.md)

- [Azure számlázási és előfizetési – gyakori kérdések](../billing-subscription-faq.md)

> [AZURE.NOTE] Ha továbbra is további kérdései vannak, kérjük, a [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a problémát megoldódott gyorsan.

<!--
OLD MSDN Articles
- [What do I do if my Azure subscription become disabled?](https://msdn.microsoft.com/library/azure/dn736049.aspx)
- [Edit payment information for an existing credit card](https://msdn.microsoft.com/library/azure/dn736053.aspx)
- [Add a new credit card to use as a payment method](https://msdn.microsoft.com/library/azure/dn736057.aspx)
- [Change the credit card on your Microsoft Azure account](https://msdn.microsoft.com/library/azure/dn736050.aspx)
- [Manage your payment method](https://msdn.microsoft.com/library/azure/dn736054.aspx)
-->



<!--Image references-->
