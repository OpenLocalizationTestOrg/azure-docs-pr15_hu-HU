<properties
    pageTitle="Analytics platformokon: Értékáram-elemzés Apache vihar összehasonlítás |} Microsoft Azure"
    description="Fussa kiválasztása egy felhőalapú analytics platform-és a megjelenítő Értékáram-elemzés Apache vihar összehasonlítása használatával hogyan. Ismerje meg a szolgáltatások és közötti különbségeket."
    keywords="Analytics platform, a analytics platformokon, a felhőben analytics platform, a vihar összehasonlítása"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Súgó adatfolyam analytics platformot kiválasztása: Azure Értékáram-elemzés Apache vihar összehasonlítás

Fussa egy felhőalapú analytics platform kiválasztása a Azure Értékáram-elemzés Apache vihar összehasonlítás segítségével hogyan. Értékáram-elemzés és Azure hdinsight szolgáltatáshoz, így megadhatja a megfelelő megoldást a vállalati felügyelt szolgáltatásként Apache vihar az az érték értékűeknek használjon esetekben megértése

Mindkét analytics platformokon előnyökkel PaaS megoldást, létezik néhány fő megkülönböztető funkciókat, például megkülönböztetheti egymástól őket. Funkciók, valamint az alábbi szolgáltatások korlátozások érdekében alatti listában, először a megoldással a célok eléréséhez szükséges.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Értékáram-elemzés és összehasonlítása vihar: általános szolgáltatások ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Értékáram-elemzés</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight Apache vihar</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Forrás megnyitása</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nem, a megjelenítő Értékáram-elemzés Azure a Microsoft védjegyzett felkínáló.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Igen, Apache vihar egy licencelt Apache technológia.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Támogatott Microsoft</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
igen </p>
            </td>
            <td width="246" valign="top">
                <p>
igen </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hardverkövetelményei</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nincsenek hardver követelmények. Azure Értékáram-elemzés egy Azure Service szolgáltatást.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nincsenek hardver követelmények. Apache vihar egy Azure Service szolgáltatást.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Felső szintű egység</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure Értékáram-elemzés vevőkkel telepítése, és figyelemmel követheti a továbbított feladatokat.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
A HDInsight Apache vihar az ügyfelek üzembe helyezéséhez és figyelheti a teljes fürtre, és több vihar feladatok, valamint egyéb munkaterhelésekből (beleértve a köteg) üzemeltetheti.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ár</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Értékáram-elemzés van árú által feldolgozott adatok mennyisége, és továbbított mennyiségek (fut a feladat óránként) szükséges.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">További árinformációkat a itt találhatók.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
A HDInsight vihar Apache vásárlás módosítása fürt-alapú, és fel van töltve a fürt fut, a feladatok rendszerbe független időben.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">További árinformációkat a itt találhatók.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Minden egyes analytics platformon létrehozása##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Értékáram-elemzés</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight Apache vihar</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Lehetőségeit: SQL-DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Igen, egy könnyen használható SQL nyelv támogatását érhető el.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nem, felhasználók kell kódírás a Java C# vagy Trident API-k használata.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Lehetőségeit: Időbeli operátorok</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ablakok összegzések és időbeli illesztések beépített támogatottak.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Időbeli operátorok kell a felhasználó által végrehajtandó.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Fejlesztési élmény</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interaktív szerzői és hibakeresés a mintaadatok tapasztalható Azure portálon keresztül.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Fejlesztési hibakeresése során, és a figyeléshez élmény is biztosít a Visual Studio segítségével a .NET-felhasználóknak, miközben a Java- és más nyelvek fejlesztők élmény használhat az IDE tetszés szerinti.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Támogatás hibakeresése</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Értékáram-elemzés egyszerű feladat állapotát és hibakeresés módja műveletek naplók kínál, de jelenleg nem ajánlja fel a Mi a teendő, és hogyan nagy része a naplókat ie rugalmasság: részletes módot.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Részletes naplók hibakeresés céljából érhetők el. A felhasználói felület naplók kétféle módon, visual Studio vagy a felhasználói is RDP be a fürt naplók eléréséhez.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>UDF támogatása (felhasználó által definiált függvények)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Jelenleg nem támogatja a UDF.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
UDF írhatók a C#, Java vagy a a választott nyelven.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Bővíthető - egyéni kódot.</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Értékáram-elemzés bővíthető kód nem nyújt támogatást nem.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Igen, nem kódírás egyéni C#, Java vagy más támogatott nyelvek a vihar elérhetőségét.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Adatforrások és a kimeneti értékeket##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Értékáram-elemzés</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight Apache vihar</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Beviteli adatforrások</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>A támogatott beviteli forrásai Azure esemény hubok és Azure BLOB.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Összekötők áll rendelkezésre az esemény hubok, szolgáltatás Bus, Kafka, stb. Nem támogatott összekötők végrehajthatók keresztül egyéni kódot.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Adatbeviteli formátumok</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Támogatott beviteli formátumok Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Egyéni kódot keresztül bármilyen formátumú lehet végrehajtani.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Kimeneti értékeket</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Előfordulhat, hogy egy adatfolyam feladat több kimeneti értékeket. Támogatott kimeneti értékeket: Azure esemény hubok, Azure Blob-tárolóhoz, Azure táblázatok, Azure SQL-adatbázis és PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Támogatás sok kimeneti értékeket topológiát, minden kimeneti lehet későbbi feldolgozásra egyéni logika. Beépített vihar tartalmaz összekötők PowerBI, Azure esemény hubok, Azure Blob-tárolóban, Azure DocumentDB, SQL és HBase. Nem támogatott összekötők végrehajthatók keresztül egyéni kódot.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Adatformátumok kódolás</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Értékáram-elemzés szükséges UTF-8 adatformátum el kell végezni.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Bármely kódolási adatformátum végrehajthatók keresztül egyéni kódot.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Kezelési és műveleti##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Értékáram-elemzés</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight Apache vihar</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Feladat telepítési modell</strong>
                </p>
                <p>
                    - <strong>Azure portálon</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>A PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Telepítési Azure portálon, a PowerShell és a REST API-khoz áll rendelkezésre.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment Azure portálon, a PowerShell, a Visual Studio és a REST API-khoz áll rendelkezésre.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Figyelés (stat, számláló stb.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Figyelés keresztül Azure-portál és -REST API-khoz végrehajtani.
                </p>
                <p>
A felhasználó Azure riasztások is beállítható.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Figyelés vihar felhasználói felület és a REST API-khoz végrehajtani.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Méretezhetőség:</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Minden feladat Streaming egységek számát. Minden egyes Streaming egység feldolgozásával legfeljebb 1 MB/s. Alapértelmezés szerint 50 egységek maximális. Hívás korlát növeléséhez.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
A HDI vihar fürt található csomópontok számának. Nincs csomópontok (felső határ az Azure kvóta által meghatározott) száma korlátozott. Hívás korlát növeléséhez.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Adatkezelési korlátai</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Felhasználók méretezheti felfelé vagy lefelé adatfeldolgozás növelése vagy optimalizálása költségek folyamatos átvitelű egységek számát.
                </p>
                <p>
Legfeljebb 1 GB/s méretezése </p>
            </td>
            <td width="246" valign="top">
                <p>
Felhasználói méretezheti felfelé vagy lefelé fürt méret igényeknek megfelelően.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Megszakítása és folytatása</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Állítsa le, és folytathatja a legutóbbi helyről leállt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Állítsa le, és folytathatja a legutóbbi leállítva alapján a vízjelet helyezni.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Frissítés szolgáltatás és a keret</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Az automatikus javítási nincs legrövidebb leállás együtt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Az automatikus javítási nincs legrövidebb leállás együtt.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Üzleti folytonosságot garantált SLA a nagyon elérhető szolgáltatáson keresztül</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
A 99,9 % üzemidőt SLA </p>
                <p>
Az automatikus helyreállítás a hibák </p>
                <p>
Állapot-nyilvántartó időbeli operátorok helyreállítás az beépített.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
A vihar fürt 99,9 % felmérést, SLA. Apache vihar hibafa alternatív adatfolyam platform, azonban a vevők feladata annak érdekében, hogy a feladatokat a továbbított folyamatos futtatása.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Speciális szolgáltatásaira##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Értékáram-elemzés</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight Apache vihar</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Késői érkezési és a megfelelő sorrendben esemény kezelése</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Beépített konfigurálható házirendek sorrendjének módosításához húzza az események, vagy módosíthatja a időpontnál.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Felhasználói ebben az esetben kezelése logika kell végrehajtania.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hivatkozási adatok</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Hivatkozási adatok elérhető az Azure BLOB maximális mérete 100 MB a memóriában keresési gyorsítótárat. A hivatkozási adatok frissítése a szolgáltatás kezeli.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Adatok mérete nem korlátozza. Összekötők HBase, DocumentDB, SQL Server és Azure érhetők el. Nem támogatott összekötők végrehajthatók keresztül egyéni kódot.
                </p>
                <p>
A hivatkozási adatok frissítésének egyéni kóddal kell kezelni.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Gépi tanulási integrációja</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Konfigurálásával közzétett Azure gépi tanulási modellek függvények során ASA feladat létrehozása <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(a személyes előzetes verzió)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Vihar csapszegek keresztül érhető el.
                </p>
            </td>
        </tr>
    </tbody>
</table>
