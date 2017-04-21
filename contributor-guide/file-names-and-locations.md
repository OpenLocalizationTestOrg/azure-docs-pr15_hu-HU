<properties title="" pageTitle="A fájl nevét és helyét Azure technikai cikkek" description="Ebből a cikkből megtudhatja, cikkek, és amikor létrehoz egy új cikk célszerű követnie elnevezési szabályokat fájlszerkezet." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>A fájl nevét és helyét Azure technikai cikkek

A műszaki tartalomtár használjuk meg az összes cikk egy mappájába (a **cikkek** mappa). Nincs mappa hierarchiája – az összes cikk live strukturálatlan fájlhoz szerkezetében. Ha cikkeket azokat a mappákat hoz létre, a cikkek nem tehető közzé.

Egy fájlszerkezet egy szervezése elve szerint helyett egy szigorú fájlelnevezési szabályok, amelyek egyértelműen azonosítja a témakörök és, amely a webes feltárást hozzájárul használjuk.

Az alábbiakban mit kell tudnia:

+ [Szabályok]
+ [Minta]
+ [Szabványos példák]
+ [Tartalom piactér]
+ [Fájl neve jóváhagyása]

##<a name="rules"></a>Szabályok

- Fájlok neveket is tartalmazhatnak, csak kisbetűket, számokat és kötőjelet. 
- Szóköz vagy írásjeleket. A kötőjel segítségével választják el a szavak és számok az a fájl nevét.
- Legfeljebb 80 karakterek – Ez a közzétételi rendszer korlátozott
- Használata művelet műveletek, például az adott fejlesztése, vásárlása, Szerkesztés, kapcsolatos hibák elhárítása. Nincsenek - ing szavak.
- Nincsenek kis szavak - nem veszi a, majd a, a vagy, stb.
- Az összes fájl a markdown legyen, és használja a .md fájlkiterjesztés.
- Ügyeljen a szavak a fájlnevekben nem jóváhagyott vagy szükségtelen mozaikszavak elkerülése

Betűszavak és initialisms a fájlnevekben - adott irányelvek:

- Nem rövidítheti: Azure szolgáltatásnevek – a fájl nevét az első szavak kell lennie a standard Azure szolgáltatás vagy technológia nevének betűvel. 
-   Nem engedélyezik a erőforrás-kezelő vagy arm, a fájl nevét a bárhol mozaikszavak
- Más szabványos rövidítéseket elfogadhatók a fájlnevekben szükség szerint. 

##<a name="pattern"></a>Minta

Az alábbiakban az általános mintázattal:

 **Service-platform-Language-Content-Product-Version.md**

Használja a minta részeit, valamint tekintse át a tárházba arról meglévő nevek, hogy a cikkeket listáját tartalmazza. A fejlesztők platform nem kezdődő neveket vagy a szolgáltatás neve pedig valószínűleg gyanús keresztül alkalmazni.

##<a name="standard-examples"></a>Szabványos példák

Íme néhány példa, hajtsa végre a mintázat érvényes nevek. :

- felhőalapú-szolgáltatások-dotnet-folyamatos-delivery.md
- mobil-szolgáltatások – ios-get-started.md
- documentdb kezelése account.md
- Mobile-Services-DotNet-backend-Get-Started-Settings-Sync.md
- Active-Directory-Java-Authenticate-Users-Access-Control-eclipse.md
- Virtual-machines-Install-Windows-Server-2008r2.md
- gyorsítótár-aspnet-munkamenet-állapot-szolgáltató
- Azure-SDK-DotNet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Tartalom piactér

Tartalom, az Azure Piactérhez partner adományok koncentrál megkülönböztetni, indítsa el a "piactér" a fájl nevét. A tartalom nem kell túl általános, mint a legtöbb partner tartalmat kell létrehozni a partnerek saját webhelyek.

- Marketplace-mongodb-Virtual-machines-Install-Windows-Server-2008r2.md

##<a name="file-name-approval"></a>Fájl neve jóváhagyása

A csoport ki kérelem véleményezőinek neve, tekintse át a fájlnevekben, amikor egy új fájlt a tárházba elküldése az első alkalommal a feladat. Leküldéses kérelem véleményezők kell ellenőrizze a fájl nevét, és a leküldéses kérelem Megjegyzés adatfolyam keresztül visszajelzést, ha a szükséges módosításokat. A fájl nevét kell javítani kell ahhoz a leküldéses felkérés elfogadása. Munkatársak könnyen közzétehetik a frissítést a függőben lévő ki kérelmet.

##<a name="folder-names-in-the-repo"></a>A repó mappa Névlista

Csak az szolgáltatások jönnek létre mappák, és a fájlnevet egyeznie kell a service Infó. Csak betűket és kötőjeleket használják, de az összes kisbetűket. Szerezzen be jóváhagyási a tárházba felügyeleti, amely nem egy kiadott szolgáltatás új mappa létrehozása előtt.

##<a name="changing-case-in-file-names"></a>A fájlnevekben eset módosítása

Windows operációs rendszerek és nagybetűk, így ha módosítania kell a fájl nevét a javítás géphez, akkor célszerűbb érdemi módosítsa, hacsak nem nézheti meg a módosítást, Mac vagy Linux rendszerhez Példa:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

A következő paranccsal nevezze át a fájlt:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Munkatársak naptárának útmutató hivatkozások

- [A cikk – áttekintés](./../README.md)
- [Index útmutatást cikkek](./contributor-guide-index.md)


<!--Anchors-->
[Szabályok]: #rules
[Minta]: #pattern
[Szabványos példák]: #standard-examples
[Tartalom piactér]: #marketplace-content
[Fájl neve jóváhagyása]: #file-name-approval
