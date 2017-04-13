<properties
   pageTitle="Lassú fájljait és mappáit Azure biztonsági másolata elhárítása |} Microsoft Azure"
   description="Nyújt hibaelhárítási útmutatást segítséget okának Azure biztonsági másolat teljesítménnyel kapcsolatos problémák"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Lassú fájljait és mappáit Azure biztonsági másolata – problémamegoldás

Ez a cikk segít okának fájlok és mappák biztonsági lelassulhat használata esetén Azure biztonsági hibaelhárítási útmutatást tartalmaz. Biztonsági másolatok használatakor az Azure biztonsági ügynök a biztonsági mentés vártnál hosszabb igénybe vehet. A késleltetés okozhatja az alábbiak közül:

-   [Nincsenek teljesítmény szűk azon a számítógépen, másolat készül.](#cause1)
-   [Egy másik folyamat vagy a víruskereső szoftver zavarja az Azure biztonsági másolat folyamat.](#cause2)
-   [A biztonsági másolat agent fut az Azure virtuális gépen (virtuális).](#cause3)  
-   [Nagyszámú (milliós) fájlok esetén biztonsági mentése](#cause4)

Mielőtt megkezdené a hibaelhárítás, az azt javasoljuk, töltse le és telepítse az [Azure biztonsági másolat agent legújabb verzióját](http://aka.ms/azurebackup_agent). Gyakori frissítések különböző problémák, a funkciók felvétele és a teljesítmény javítása a biztonsági másolat ügynökének VÁLLALUNK.

Azt is ajánljuk, hogy tekintse át az [Azure biztonsági másolat szolgáltatás – gyakori kérdések](backup-azure-backup-faq.md) , hogy nem tapasztal az közös konfigurációs problémák.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>OK: Teljesítményt szűk azon a számítógépen

Azon a számítógépen, másolat készül szűk okozhatják késések. A számítógépen azt jelenti, hogy olvasása és írása lemezre vagy sávszélességre adatok küldése a hálózaton keresztül, például szűk okozhatja.

A Windows egy beépített eszköz, amely a [Teljesítmény Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) észleli az alábbi szűk hívta biztosít.

Íme, néhány teljesítményét számláló és tartományok, akkor lehet hasznos optimális biztonsági másolatok szűk azonosításában.

| A számláló  | Állapot  |
|---|---|
|Logikai (tényleges) lemez – üresjárati %   | • 100 %-os üresjárati 50 %-os üresjárati kifogástalan =</br>• 49 % 20 %-os üresjárati üresjárati = figyelmeztetés vagy a Monitor</br>• 19 % üresjárati üresjárati 0 % = kritikus vagy a "Házon kívül" specifikáció|
|  Logikai (tényleges) lemez – % átlagos Lemezen Sec írási és olvasási |  • 0,015 MS 0,001 ms kifogástalan =</br>• 0,025 MS 0,015 ms = figyelmeztetés vagy a Monitor</br>• 0.026 ms vagy már = kritikus vagy a "Házon kívül" specifikáció|
|  Logikai (tényleges) lemez – aktuális lemez hossza (az összes előfordulását) | 6 percnél hosszabb 80 kérések |
| A memória – nem lapozható bájt|• Felhasznált készlet 60 %-nál kisebb kifogástalan =<br>• 61-80 %-készlet elfogyasztott = figyelmeztetés vagy a Monitor</br>• Nagyobb, mint az elfogyasztott mennyiség 80 %-os készlet = kritikus vagy a "Házon kívül" specifikáció|
| A memória – lapozható bájt |• Felhasznált készlet 60 %-nál kisebb kifogástalan =</br>• 61-80 %-készlet elfogyasztott = figyelmeztetés vagy a Monitor</br>• Nagyobb, mint az elfogyasztott mennyiség 80 %-os készlet = kritikus vagy a "Házon kívül" specifikáció|
| A memória – elérhető MB| • 50 %-os ingyenes rendelkezésre álló memória vagy több = kifogástalan</br>• 25 %-os ingyenes rendelkezésre álló memória = Monitor</br>• 10 %-os ingyenes rendelkezésre álló memória = figyelmeztetés</br>• 100 MB vagy ingyenes rendelkezésre álló memória 5 %-nál kevesebb = kritikus vagy a "Házon kívül" specifikáció|
|Processzor –\%processzor kihasználtsága ()|• 60 %-nál kisebb elfogyasztott kifogástalan =</br>• 61-90 % felhasznált = Monitor vagy óvatos</br>• 91-100 %-elfogyasztott kritikus =|


> [AZURE.NOTE] Ha úgy dönt, hogy a infrastruktúra-e a probléma oka, azt javasoljuk, hogy a lemezen rendszeresen jobb teljesítményt töredezettségmentesítéséhez.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>OK: Egy másik folyamat vagy a víruskereső szoftver megzavarása Azure biztonsági mentése

Láthatta, hogy hol más folyamatok a Windows rendszer van negatív hatással a teljesítményt az Azure biztonsági másolat ügynök folyamat több példánnyal. Például ha használatával az Azure biztonsági másolat ügynök és egy másik program is kevésbé részletes adatok, vagy ha a víruskereső szoftver fut, és zárolta a fájlok másolat készül, a többszörös zárolja, a fájlok okozhatnak kérelem. Ebben az esetben a biztonsági mentés előfordulhat, hogy nem, vagy a feladatot a vártnál hosszabb ideig tarthat.

A legjobb ajánlási ebben az esetben is kapcsolja ki a többi biztonsági program látható, hogy a biztonsági másolat időpontot tudja kiválasztani az Azure biztonsági ügynök változik. Gondoskodhat arról, hogy több biztonsági mentési feladat nem fut egyszerre rendszerint elegendő megakadályozhatja, hogy az érintő egymást.

A víruskeresők azt javasoljuk, hogy kizárja a következő fájlokat és a helyekre:

- C:\Program Files\Microsoft Azure helyreállítási szolgáltatások Agent\bin\cbengine.exe egy folyamat szerint
- C:\Program Files\Microsoft Azure helyreállítási szolgáltatások Agent\ mappák
- Hely megkarcolhatja, (Ha nem használ a szabványos hely)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>OK: Biztonsági másolat ügynök Azure virtuális-gépen fut

A biztonsági másolat agent egy virtuális a Windowst futtat, ha teljesítmény futtatásakor azt a fizikai gépen lassabb lesz. Ez a várakozások IOPS korlátai miatt.  A teljesítmény optimalizálható azonban Váltás az adatok meghajtók Azure prémium tárolóhoz éppen másolatban szerint. A problémák orvoslására: Ez a probléma dolgozunk a probléma, és a fix érhetők el az elkövetkező kiadásokban.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Okai lehetnek: Nagyszámú (milliós) fájlok mentésével

Nagy mennyiségű adat áthelyezése-nél kisebb mennyiség, az adatok áthelyezése hosszabb ideig tart. Egyes esetekben biztonsági idő nemcsak az adatokat, hanem a fájlok és mappák számát méretének kapcsolódik. Ez a különösen igaz, ha kisméretű fájlt (néhány KB néhány bájt) milliónyi éppen biztonsági másolat készül.

A jelenség oka az, hogy amíg Ön az adatok biztonsági másolatának és áthelyezése a Azure, a Azure van egyidejű rendszerezésével kapcsolatban a fájlokat. Ritka bizonyos esetekben a vártnál hosszabb ideig igénybe vehet a katalógus műveletet.

A következőkre utaló jelzéseket is segítsen megérteni szűk és ennek megfelelően működik a következő lépésekkel:

- A **felhasználói felület látható az adatátvitel elért haladás**. Az adatok átvitele továbbra is. A hálózat sávszélessége és az adatok méretét problémái késését okozhatják.

- A **felhasználói felület nem látható az adatátvitel elért haladás**. Nyissa meg a naplókat C:\Microsoft Azure helyreállítási szolgáltatások Agent\Temp található, és ellenőrizze a FileProvider::EndData bejegyzéshez a naplókat. Ez a bejegyzés jelzi, hogy az adatátvitel elkészült, és a katalógus művelet történik. A biztonsági mentési feladat nem megszakítása. Ehelyett kissé már várja meg a katalógus művelet befejezéséhez. Ha a probléma nem szűnik meg, lépjen kapcsolatba az [Azure támogatja](https://portal.azure.com/#create/Microsoft.Support).
