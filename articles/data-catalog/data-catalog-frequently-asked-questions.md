<properties
   pageTitle="Gyakori kérdések a Azure adatkatalógus |} Microsoft Azure"
   description="Gyakori kérdések az Azure adatkatalógus, beleértve a forrás felhőbeli adatfeltárási, a széljegyzetek és kezelési lehetőségeit."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure adatkatalógus gyakran ismételt kérdések

Ez a cikk a Microsoft **Azure adatkatalógus** szolgáltatással kapcsolatos gyakori kérdésekre adott válaszok nyújt a.

## <a name="q-what-is-azure-data-catalog"></a>Kérdés: Mi az Azure adatkatalógus?

Megoldás: a Microsoft Azure adatkatalógus olyan egy regisztráció és a rendszer a vállalati adatforrások feltárás szolgál a Microsoft Azure felhőben tárolt teljes körű felügyelt szolgáltatás. Azure adatkatalógus lehetőségek, amelyekkel bármelyik felhasználó – az adatok tudósok fejlesztők – regisztrálni, hogy munkatársai felfedezése, dátumtáblázatok ismertetése és felhasználása adatforrásokat tartalmaz.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>Kérdés: milyen ügyfél lekérdezi jelent Azure adatkatalógus megoldani?

Azure adatkatalógus megszünteti a az adatok forrása feltárás és a "sötét-adatok" kérdés azáltal, hogy felhasználók Fedezze fel, és vállalati adatforrásokból értelmezhető.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>Kérdés: ki a célközönségek az Azure adatkatalógus?

Azure adatkatalógus szolgáltatásokat nyújt technikai és nem technikai jellegű számára, például:

- Adatok fejlesztők, BI és Analytics szakembereknek: akik mások számára felhasználása adatok és az elemzést tartalom előállító felelős
-   Adatok Tartalomfelelősök: Azok, akik az adatokat, azt jelenti, és hogyan szánt használandó és milyen célból ismerő
- Adatok fogyasztói: Azok, akik biztosítania szükséges, egyszerűen Fedezze fel, és csatlakozás adatokhoz szükséges munkaköre azok tetszés szerinti eszközzel
- A központi informatikai: azokat, akiknek szükségük van a tételére adatforrások több száz a vállalati verzió, és ki kell karbantartása rendszergazdákat, adatok használatának fölé, és ki által

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>Kérdés: Mi az Azure adatkatalógus régióban elérhető?

Jelenleg Azure adatkatalógus szolgáltatások érhetők el az alábbi adatközpontokban:

- Nyugati Amerikai Egyesült Államok
- Kelet-Amerikai Egyesült Államok
- Nyugati Európa
- Észak-Európa
- Ausztrália keleti
- Délkelet-ázsiai

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>Kérdés: milyen azok az adatok eszközök Azure adatkatalógusában a számával?

Az ingyenes Edition az Azure adatkatalógus 5 000 regisztrált adatok eszközök korlátozódik.

A Standard Edition az Azure adatkatalógus legfeljebb 100 000 regisztrált adatok eszközök támogatja.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>Kérdés: melyek a támogatott adatforrás és eszköz adattípusok?

Kérjük, olvassa el az [Adatok katalógus DSR](data-catalog-dsr.md) jelenleg támogatott adatforrások listája.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>Kérdés: hogyan végezze el a másik adatforrás támogatás kérése?

[Azure adatkatalógus fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)frissítéseiről és egyéb visszajelzés elküldhetők.

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>Kérdés: Hogyan kezdjek hozzá az Azure adatkatalógus?

Első lépések a legjobb helyen van az [Adatkatalógusban – első lépések](data-catalog-get-started.md)az utasításokat követve. Ez a témakör az végpontok közötti áttekintése az funkciók a szolgáltatásban.

## <a name="q-how-do-i-register-my-data"></a>Kérdés: hogyan lehet regisztrálni az adatok?

Az adatok regisztrálhatja Azure adatkatalógusában, indítsa el az Azure adatkatalógus regisztrációs eszköz az Azure adatkatalógus portált a "Közzététel" helyéről. Az Azure adatkatalógus közzétételi alkalmazásban jelentkezzen be a az Azure adatkatalógus portal eléréséhez használt hitelesítő adatait, és válassza az adatforrás és az adott eszközök szeretné rögzíteni.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>Kérdés: milyen tulajdonságok regisztrált adatok eszközök kibontotta?

Az adott tulajdonságok adatforrás adatforrásból függvényében eltérőek, de az Azure adatkatalógus közzététel szolgáltatás általános bontsa ki a következő információkat:

- Eszköz neve
- Eszköz típusa
- Leírás a digitális eszköz kiválasztása
- Attribútum és oszlopnevek
- Adattípusok attribútum/oszlop
- Attribútum/oszlop leírása

> [AZURE.IMPORTANT] Adatok eszközök regisztrálása Azure adatkatalógusban nem áthelyezése vagy másolása az adatok a felhőben. Eszközök adatforrás regisztrálása másolja a metaadat-alapú eszközök Azure, de az adatok megmaradnak az a meglévő adatforrás helyének. Ez a szabály alól kivételt, ha a felhasználó úgy dönt, hogy töltse fel a rekordjainak előnézete vagy adatok profil eszközök regisztrálásakor. Amikor legfeljebb 20 rekordok előnézetét, beleértve a program átmásolja minden eszközt, és Azure adatkatalógusában pillanatképként vannak tárolva. Adatok profil felvételekor összesít adatokat (például a táblázatok, a Százalék / oszlop null értékek és a minimum, a legnagyobb és a átlagos érték oszlopokban méretének) számított, és be a metaadat-alapú szerepel.

<br/>

> [AZURE.NOTE] Az adatforrásokhoz, például SQL Server Analysis Services egy osztályú **Leírás** tulajdonságának az Azure adatkatalógus közzétételi alkalmazás fog bontsa ki a tulajdonság értékét. Az SQL Server rendszerű relációs adatbázisok, amelynek hiányzik egy osztályú **Leírás** tulajdonság, az Azure adatkatalógus közzétételi alkalmazás az érték fog kinyerése a ms_description kiterjesztett tulajdonságként a objektumok és az oszlopokban. További tudnivalókért lásd: TechNet [Bővített tulajdonságainak használata az adatbázis-objektumok](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>Kérdés: mennyi ideig kell tartani az imént bejegyzett eszközök Azure adatkatalógus jelennek meg?

Miután eszközök regisztrálása Azure adatkatalógus 5-10 másodperc csak akkor jelennek meg az Azure adatkatalógus portál időtartama lehet.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Kérdés: hogyan jegyzetelés és a metaadatokat kiegészítése saját regisztrált adatok eszközök?

A metaadat-alapú nyújtsanak bejegyzett eszközök legegyszerűbben az Azure adatkatalógus portálon jelölje ki az eszközt, és írja be a metaadatok értékét a Tulajdonságok vagy séma ablaktáblán a kijelölt objektum.

Akkor is megadhatja néhányuk szakértők és a címkék, például a regisztrációs folyamat során. Az összes eszköz adott időpontban regisztrálás alatt az Azure adatkatalógus közzétételi szolgáltatás megadott értékek érvényesek lesznek. A legutóbb regisztrált objektumok megtekintése a portálon további jegyzet, jelölje be az az Azure adatkatalógus közzétételi alkalmazás az utolsó képernyő **Megtekintése a portálon** gombjára.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>Kérdés: hogyan törölhetők a regisztrált adatobjektumok?

Az Azure adatkatalógus objektum törölheti, jelölje ki az objektumot a portálon, és válassza a **Törlés** gombra. Ez az objektum a metaadatok eltávolítása Azure adatkatalógus, de nem befolyásolja a tényleges mögöttes adatforrásban.

## <a name="q-what-is-an-expert"></a>Kérdés: Mi az, hogy egy szakértővel?

Egy szakértővel a személy, aki rendelkezik egy tájékoztatni perspektíva adatobjektumok kapcsolatban. Objektum beállíthatja, hogy több jártas szakértőktől. Egy szakértővel, nem szükséges objektum; "tulajdonosi" a szakértő egy kérdésben egyszerűen olyannak, aki tudja, hogy az adatokat is és kell használni.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>Kérdés: Hogyan oszthatok meg információkat az Azure adatkatalógus csapattal Ha problémák lépnek fel lehet?

Használja a problémák bejelentésére adatkatalógus Azure-fórum, megoszthatja, és tegyen fel kérdéseket. A fórum a következő helyen található http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>Kérdés: Azure adatkatalógus működik ez más adatforrás vagyok kíváncsi?
Aktívan dolgozunk a probléma Azure adatkatalógus megadásáról további adatforrásokhoz. Ha a kívánt támogatott, kérjük, akkor javasolt (vagy a támogatási hangpostára, ha már javasolt) [Azure adatkatalógus fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)adatforrás.

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Kérdés: hogyan Azure adatkatalógus kapcsolódik az adatkatalógusban, a Power BI az Office 365-höz?

Érdemes elképzelnie adatkatalógus Azure-adatkatalógus alakulása szerint. Azure adatkatalógus biztosítja az adatok forrása közzétételi és feltárás hasonló funkciókat, de szélesebb esetek a fókuszban lévő és az Office 365-ben nem függ. Az Azure adatkatalógus általánosan elérhetővé válik után hamarosan a két katalógusok egyetlen szolgáltatás fog egyesítheti.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>K: milyen engedélyekkel kell a felhasználó eszközök regisztrálhatja Azure adatkatalógusban?

A felhasználó az Azure adatkatalógus regisztrációs eszköz futtatása szüksége van az adatforrásban, amely lehetővé teszi, hogy olvassa el a metaadatok a forrásból származó engedélyeinek. Ha a felhasználó kijelöli előnézetét felvenni, majd a felhasználó is vele, olvassa el az objektumokat, hogy regisztrált adatainak engedélyezése engedélyekkel kell rendelkeznie.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Kérdés: Azure adatkatalógus lesz elérhető, valamint a helyszíni telepítés?

Azure adatkatalógus egy felhőalapú szolgáltatásba, mind a felhőben, és a helyszíni adatforrások kezelnek előadásához hibrid adat forrás feltárás megoldást. Jelenleg nem szerepel a tervek az Azure adatkatalógus szolgáltatás helyszíni által futtatott változata.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>K: azt kinyerése további / sokoldalúbb metaadat-alapú az adatforrások azt regisztrálni?

Bontsa ki a Azure adatkatalógus funkcióinak aktívan, már dolgozunk. Ha szeretné látni kibontott az adatforrásból regisztráció során további metaadatokkal, kérjük, javaslat (vagy a szavazás, ha már javasolt) [Azure adatkatalógus fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). A jövőben azt lehetővé teszi a harmadik felek hozzáadása új keresztül egy bővítési API-ja adatforrástípus esetében.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Kérdés: hogyan korlátozása meg bejegyzett adatok eszközök, láthatóságát, úgy, hogy csak bizonyos személyeknek, észleljék őket?

A: az adatok eszközök válassza az Azure adatkatalógus, és kattintson a "Készítése tulajdonjogot" gombra. Az adatok eszközök Azure adatkatalógusában tulajdonosai a láthatóság beállításainak módosításával, vagy felhasználóimnak összes katalógus felfedezése a saját tulajdonú eszközök, illetve a láthatóság korlátozása adott felhasználókhoz.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>K: Hogyan módosíthatom a regisztrációs adatok eszköz, amely az adatforrás módosítása a katalógus jelennek meg?

A: Ha frissíteni szeretné a metaadat-alapú adatok eszközök már regisztrált a katalógus, egyszerűen újraregisztrálása az adatforrásban, amely tartalmazza az eszközöket. Az adatforrás módosításokat, például oszlopokat hozzáadni, illetve eltávolítja a táblák vagy nézetek, frissítése a katalógus, de minden felhasználó által megadott széljegyzetek maradnak.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>Probléma: a kérdés nem érkezett válasz itt – Mit tegyek?

Központi a fölé a [Azure adatkatalógus fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Kérdések van azok módon itt találja.
