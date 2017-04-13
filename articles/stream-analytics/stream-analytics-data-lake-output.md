<properties
    pageTitle="Adatfolyam Analytics adattár tó kimeneti |} Microsoft Azure"
    description="Hitelesítés és az Azure adatok tó tár Értékáram-elemzés feladatban engedélyezési konfigurálása"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Adatfolyam Analytics adattár tó kimeneti

Adatfolyam Analytics feladatok több kimeneti módszer, az [Azure tó adattár](https://azure.microsoft.com/services/data-lake-store/)egyik támogatja. Azure tó adattár egy vállalati szintű hyper skála a tárháza nagy adatok analitikus munkaterhelésekből. Tó adattár lehetővé teszi, hogy minden méretét, a típus és a bevitel sebességét működési és felderítő analytics adatainak tárolása.

## <a name="authorize-a-data-lake-store-account"></a>Engedélyezheti egy tó adattár fiók

1.  Az Azure adatkezelési portálon egy kimenetként tó adattár kijelölésekor kérni fogja a meglévő tó adattár a használatát engedélyezni vagy a következőképpen igényelhet hozzáférést az adatok tó áruházból előzetes az Azure klasszikus portálon keresztül.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Ha már rendelkezik hozzáféréssel tó adattár, kattintson a "Most már engedélyezni", és lap egy rövid ideig felugró jelző "Engedély átirányítása...". Az oldal automatikusan bezárul, és a lapot, amely lehetővé teszi a tó adattár kimeneti beállítása a bemutatni.

Ha Ön nem regisztráltak Adatvillámnézet tó áruházból, a kérni "Regisztráció most" hivatkozásra, vagy kövesse az [első lépések útmutató](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Kimeneti tó adattár tulajdonságainak konfigurálása

Ha befejezte a hitelesített tó adattár fiókot, tulajdonságait módosíthatja a tó adattár kimeneti. Az alábbi táblázat tulajdonságok neve és azok leírását a tó adattár kimeneti konfigurálása a lista található.

<table>
<tbody>
<tr>
<td><B>TULAJDONSÁG NEVE</B></td>
<td><B>LEÍRÁS</B></td>
</tr>
<tr>
<td>Kimeneti Alias</td>
<td>Ez a használt lekérdezések irányítsa át a lekérdezés eredménye a adatok tó áruház rövid nevét.</td>
</tr>
<tr>
<td>A fiók tó adattárhoz</td>
<td>Ha szeretné elküldeni a kimeneti tárterület-fiók neve. Választhat a tartalmazó legördülő listát, amelyhez hozzáférése van a portálra bejelentkezett felhasználó tó adattár fiókok.</td>
</tr>
<tr>
<td>Elérési út előtag mintát [<I>választható</I>]</td>
<td>A fájl elérési útja, írja be a fájl a megadott adatok tó áruházból számla belüli használt. <BR>{date}, {idő}<BR>Példa: 1: mappa1/naplók / {date} / {time}<BR>Példa 2: mappa1/naplók / {date}</td>
</tr>
<tr>
<td>A dátumformátum [<I>választható</I>]</td>
<td>Ha a dátum jogkivonat az előtag elérési utat, választhat a dátumformátumot, amelyben a fájlok vannak rendezve. Példa: YYYY/hh/nn</td>
</tr>
<tr>
<td>Időformátum [<I>választható</I>]</td>
<td>Ha az időt jogkivonat az előtag elérési utat, adja meg az időformátumot, ahol a fájlok vannak rendezve. Jelenleg az egyetlen támogatott érték HH.</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>Kimeneti adatai szerializálási formátum. JSON CSV és Avro támogatottak.</td>
</tr>
<tr>
<td>Kódolás</td>
<td>Ha a CSV- vagy JSON formázásához kódolást meg kell adni. UTF-8 jelenleg csak az támogatott kódolási formátumot.</td>
</tr>
<tr>
<td>Elválasztó karakterrel</td>
<td>Csak a CSV-szerializálási alkalmazható. Értékáram-elemzés közös határolójel számos támogatja a CSV-adatok szerializálása. Támogatott értékek vesszővel, pontosvesszővel, hely, lapon, és függőleges vonás:.</td>
</tr>
<tr>
<td>Formátum</td>
<td>Csak a JSON szerializálási alkalmazható. Vonal elválasztott Itt adhatja meg, hogy úgy, hogy minden új sor elválasztva JSON-objektum kimenetének lesz formázva. Tömb Itt adhatja meg, hogy a kimenet formátumú JSON objektumok tömbjét.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Tó adattár engedélyezési megújítása

Jelenleg nincs korlátozás hol a hitelesítési token kell manuálisan frissíteni kell minden 90 napig tó adattár kimeneti az összes feladatot. Szüksége lesz is, ha módosította a jelszavát, mivel a feladatok létrehozásának vagy az utolsó hitelesített újból hitelesíteni a tó adattár fiókját. A problémát a jelenség akkor nincs feladat kimenet és a művelet naplókban újbóli engedély szükséges jelző hiba.

A probléma megoldásához a futó feladat leállítása, és nyissa meg a tó adattár kimeneti. A "Engedély megújítása" hivatkozásra, és egy rövid ideig lap felugró jelző "Engedély átirányítása...". Az oldal automatikusan bezárul, és sikerrel jár, ha megjelöli "Engedély sikeresen megújult". Ekkor a lap alján kattintson a "Mentés" segítségre van szüksége, és indítsa újra az adatvesztés elkerülése érdekében a legutóbbi leállt időpontot a feladatok folytatható.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
