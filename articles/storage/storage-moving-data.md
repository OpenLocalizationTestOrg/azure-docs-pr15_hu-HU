<properties
    pageTitle="Az adatok Azure tárhelyről mozgatása |} Microsoft Azure"
    description="Ez a cikk áttekintést nyújt a különböző módszerek az adatok Azure-tárhelyről mozgatása."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Az adatok Azure-tárhelyről mozgatása

A helyszíni adatok Azure tárolóhoz (vagy fordítva) áthelyezni kívánt, esetén számos lehetőség közül választhat. A megoldás, amely a legjobban az igényektől függ. Ez a cikk a különböző forgatókönyveket és a megfelelő szeretne rendelni, mindegyiknél gyors áttekintést nyújt.

## <a name="building-applications"></a>Építőelem-alkalmazások

Ha egy alkalmazást, szemben a REST API elkészítésének szeretne összeállítani, vagy a sok ügyfél-tárak egyik helyezze át az adatokat, hogy és Azure-tárhelyről kiválóan használhatók.

Azure tároló ügyfélprogramok tárak nyújt a .NET, iOS, Java, Android, univerzális Windows platformon (UWP), Xamarin, C++, Node.JS, PHP, fonetikus és Python. Az ügyfél-tárak például újrapróbálkozási logika, a naplózás és a párhuzamos feltöltések speciális funkciókat kínál. Közvetlenül a REST API-t, amely lehetővé teszi a HTTP-/ HTTPS-kérések bármely nyelvre által hívható szemben is készíthet.

Lásd: az [Első lépések az Azure Blob-tárolóhoz](storage-dotnet-how-to-use-blobs.md) további információt.

Ezeken kívül is kínálunk nagy teljesítményű adatok másolása, és az Azure tervezett tárban van [Azure tároló adatforrástárban mozgását](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) . További információt a mozgás adatforrástárban [dokumentációjában](https://github.com/Azure/azure-storage-net-data-movement) találhat. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Gyors megtekintése és használata az adatok

Ha azt szeretné, hogy az Azure tároló adatok megtekintése az azt jelenti, hogy feltöltése és letöltése az adatok mellett is ugyanígy, követően fontolja meg egy Azure tároló Intézőből.

Nézze meg a [Tárterület-kezelők Azure](storage-explorers.md) további listáját.

## <a name="system-administration"></a>Rendszer felügyelete

Ha szükséges, vagy annál kényelmesebbnek parancssori segédprogram (például a rendszergazdák), az alábbiakban a bemutatókkal lehetőségekből választhat:

### <a name="azcopy"></a>AzCopy

AzCopy egy nagy teljesítményű adatok másolása, és Azure-tárhelyről készült Windows parancssori segédprogramot. Is másolhatja az adatokat egy tárterület-fiókból vagy más-más tárolási fiókok között.

Lásd: az [adatok, mellettük a AzCopy parancssori segédprogram átadása](storage-use-azcopy.md) további információt.

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell egy modulra, amelybe parancsmagok Azure szolgáltatásainak kezelése. Érdemes egy feladatalapú parancssori rendszerhéj és a tervezett, különösen a rendszer felügyeleti parancsfájlok futtatásának nyelv.

[Azure PowerShell használatá Azure tároló](storage-powershell-guide-full.md) további témakörben találhat.

### <a name="azure-cli"></a>Azure CLI

Azure CLI biztosít a forrás megnyitása platformok parancsok az Azure szolgáltatások használatához. Azure CLI érhető el Windows, OSX vagy Linux rendszerhez.

Lásd: [az Azure CLI Azure adathordozós használatával](storage-azure-cli.md) című témakörben talál további.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Nagy mennyiségű adatot lassú hálózaton való áthelyezése

A legfontosabb problémáit társított nagy mennyiségű adatot áthelyezése egyik átviteli idő. Adatok beolvasása a/Azure tároló hálózatok költségek aggódnia vagy kódírás nélkül szeretne, majd Azure importálás/exportálás esetén egy megfelelő megoldást.

Lásd: [Azure importálás/exportálás](storage-import-export-service.md) további információt.

## <a name="backing-up-your-data"></a>Az adatok biztonsági mentése

Ha egyszerűen kell biztonsági másolatot a Azure tárolóhoz, Azure biztonsági módja a go. Ez a biztonsági másolat készítése a helyszíni adatok és Azure VMs hatékony megoldás.

Lásd: [Azure biztonsági másolat](../backup/backup-introduction-to-azure-backup.md) további információt.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Az adatok helyszíni hozzáférés és a felhőből

Ha olyan megoldást az adatok helyszíni eléréséhez és a felhőből, majd vegye figyelembe Azure a hibrid felhőalapú tárolási megoldás StorSimple használata. Ez a megoldás, hogy ezután tárolja a gyakran használt adatok SSD, alkalmanként használt adatokat a HDDs és az inaktív és biztonsági másolat/archiválás adatok Azure tárolón fizikai StorSimple eszköz áll.

Lásd: [StorSimple](../storsimple/storsimple-overview.md) további információt.

## <a name="recovering-your-data"></a>Az adatok helyreállítása

Amikor a helyszíni munkaterhelésekből és alkalmazások, szüksége van, amely lehetővé teszi a futó katasztrófa esetén továbbra is az üzleti megoldás. Azure webhely helyreállítási replikációs, feladatátadási és helyreállítási virtuális gépeken futó és fizikai kiszolgálók kezeli. Azure tárolására, amely lehetővé teszi, hogy egy másodlagos helyszíni adatközponthoz fölöslegessé replikált adatokat tárolja.

Lásd: [Azure webhely helyreállítási](../site-recovery/site-recovery-overview.md) további információt.
