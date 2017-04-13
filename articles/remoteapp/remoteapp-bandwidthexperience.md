<properties 
    pageTitle="Azure RemoteApp – hogyan hálózat sávszélessége és minőségét tapasztalja munka együtt? | Microsoft Azure"
    description="Megtudhatja, hogyan Azure RemoteApp a hálózat sávszélessége is hatással lehet a felhasználói élmény minősége."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp – hogyan hálózat sávszélessége és minőségét tapasztalja munka együtt?

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Az [Általános hálózati sávszélesség](remoteapp-bandwidth.md) az Azure RemoteApp szükséges készíteni, tartsa szem előtt az alábbi tényezőket - ezek a dinamikus rendszert, amely hatással van a teljes felhasználói felület minden részét képezik. 

- **Aktuális hálózati feltételek és a rendelkezésre álló hálózati sávszélesség** - paraméterek (veszteség, időtartam, szolgáltatás) ugyanazon a hálózaton, egy adott időben hatással lehet a folyamatos átvitelű élmény, ami azt jelenti, egy leeresztett teljes felhasználói felület az alkalmazás. Érhető el a hálózati sávszélesség túlzsúfolt, a véletlen veszteség, a késés függvény, mert ezek a paraméterek hatással a túlzsúfolt vezérlő mechanizmusa, amely az viszont a vezérlők sebessége ütközések elkerülése érdekében.  Például a veszteséges hálózathoz vagy magas válaszidejű láthatóvá válik a felhasználói felület hibás még egy 1000 MB sávszélesség-hálózaton. Az adatvesztés és a késés függ, amelyek ugyanabba a hálózatba, és mi azoknak a felhasználóknak mivel foglalkoznak (például megnézi a videók, letöltésének vagy feltöltésének nagy fájlok nyomtatása) felhasználók számát.
- **Használati eset** – a felület függ, hogy mi a felhasználók teljesítenek a személyek és a hálózaton csoportként. Például csak egyetlen keret frissíteni kell; olvasási egy dia igényel Ha a felhasználó skims, és a szöveg dokumentum tartalomban görgetés, keretek másodpercenként frissíteni kell nagyobb számú szükségük van. Kapcsolatba lépni a másikba ebben az esetben a kiszolgáló ahányat felhasználja több sávszélességet. Is vegye figyelembe a rendkívüli példa: több felhasználó van (például a felbontás 4K) a nagy felbontású videók megtekintése, tartsa HD konferenciahívások, 3D videó játékok vagy CAD rendszeren működik. Ezek közül az összes használhatatlan lehet akkor is nagyon magas sávszélesség hálózati gyakorlatilag.
- Teljes frissítés nagyobb képernyőkön-nél kisebb képernyőkön szükség a **képernyőfelbontás és a lapok számát** – további hálózati sávszélesség. A alaptechnológiát kódolást, és csak a képernyőn megjelenő utasításokat a frissített a régiók továbbítása meglehetősen jó feladat jelent, de egyszer, a while, a teljes képernyő frissítésre szorul. Amikor a felhasználó nagyobb képernyőfelbontás (például 4K felbontás) van, a frissítéshez további hálózati sávszélesség-nél kisebb felbontású (például 1024x768px) képernyőre. Ez a módszer vonatkozik, ha több képernyőn átirányításhoz. Sávszélesség kell növelése képernyőkön számával együtt.
- **Vágólap és eszköz átirányítás** – Ez egy Igen egyértelmű probléma, de sok esetben, ha egy felhasználó tárolja a vágólapra, egy nagy adattömb vesz igénybe egy kicsit esetén ezt az információt a távoli asztali ügyfélprogramból a kiszolgáló nem ruházhatja át. A későbbi felület is hatással lehet a felület előtt elküldésének a vágólap tartalmát. Azonos-re érvényes eszköz átirányításhoz - képolvasóról vagy webkamera hoz létre az adatokat, el lehet küldeni a kiszolgálóra felsőbb szintű kell, hogy, illetve a nyomtató kell fogadására a nagyméretű dokumentum helyi tároló van szüksége a nagyméretű fájlok másolása a felhőben futó alkalmazás elérhetővé szeretné tenni, felhasználók megfigyelheti Kihagyott keretek vagy ideiglenes "rögzített" videó, mert az eszköz átirányításhoz szükséges adatokat a hálózat sávszélessége növekszik van szüksége. 

Amikor Ön a hálózati sávszélesség igények felmérése a, feltétlenül fontolja meg a következő tényezők dolgozik, a rendszer.

Ezután térjen vissza a [fő hálózati sávszélesség-cikk](remoteapp-bandwidth.md), vagy lépjen tovább a [hálózat sávszélessége](remoteapp-bandwidthtests.md)tesztelése.

## <a name="learn-more"></a>tudj meg többet
- [Azure RemoteApp hálózati sávszélesség-használat becslése](remoteapp-bandwidth.md)

- [Azure RemoteApp – teszteli a hálózati sávszélesség-használat a néhány gyakori alkalmazási területek](remoteapp-bandwidthtests.md)

- [Azure RemoteApp hálózati sávszélesség - tippet olvashat a (Ha nem lehet tesztelni saját)](remoteapp-bandwidthguidelines.md)