<properties 
    pageTitle="Jelölők figyelése StorSimple |} Microsoft Azure" 
    description="Fénykibocsátó dióda (LED) és a StorSimple eszköz állapotának nyomon követésére hallható riasztások ismerteti."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Az eszköz kezelése a jelölők figyelése StorSimple használatával   

## <a name="overview"></a>– Áttekintés

A StorSimple eszköz fénykibocsátó dióda (LED) tartalmaz, és riasztókat használható figyelheti a modulok és a StorSimple eszköz általános állapotát. A felügyeleti jelölők találhatók meg az eszköz elsődleges ház és a EBOD ház hardver összetevői. A felügyeleti jelölők lehet LED vagy a program riasztások.

Vannak olyan modul állapotát jelző három LED állapota: zöld, vörös-sárga vagy vörös-sárga-zöld.  

- Zöld LED jelenítik meg a megfelelő működési állapota.  
- Zöld vörös-sárga LED jelentik – nem kritikus feltételek, előfordulhat, hogy felhasználói beavatkozás jelenlétét villogó.  
- Vörös-sárga LED jelzi, hogy van-e egy kritikus hiba, ezen belül a modul.  

Ez a cikk hátralévő ismerteti a különböző felügyeleti jelző LED, a helyekre StorSimple az eszközre, az eszköz állapotát, a LED államok és bármely, társított hallható riasztások alapján.

## <a name="front-panel-indicator-leds"></a>Első panelen jelző LED

Az első panelen, más néven a *Műveletek* vagy *ops panelen*, a rendszer minden modul összesítő állapotának megjelenítése. Az első panelen az elsődleges StorSimple és a EBOD ház azonos, és bemutat.  

   ![Eszköz első panelen][1]
 
Az első panelen tartalmazza a következőkre utaló jelzéseket:  

1. Elnémítás gomb
2. A Power jelző LED (zöld/vörös-sárga)
3. A modul hibát jelző vezetett (a vörös-sárga-és kikapcsolása)
4. Logikai hibát jelző vezetett (a vörös-sárga/kikapcsolása
5. Egység azonosító megjelenítése  

A fő közötti az első panelen az eszköz LED és a EBOD ház az különbség LED-kijelzőjén a megjelenő **Rendszer egység azonosítószámot** . Az alapértelmezett egység azonosítója jelenik meg az eszköz a **00**, mivel az alapértelmezett egység azonosítója jelenik meg a EBOD ház: **01**. Ez lehetővé teszi, hogy gyorsan különbözteti meg az eszköz és a EBOD ház Ha az eszköz be van kapcsolva. Ha az eszköz ki van kapcsolva, [kapcsolja be egy új eszközt](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) a megadott adatokkal segítségével az eszköz megkülönböztetni a EBOD ház.  

## <a name="front-panel-led-status"></a>Első panelen LED állapota  

Az alábbi táblázat segítségével azonosítani a LED meg az első panelen az eszköz vagy a EBOD ház jelölt állapotát.  

|Rendszer power | Hibafa modul | Logikai hiba | Riasztás | Állapot|
|-------------|---------------|-----------------|-------|-------|
|Vörös-sárga | KIKAPCSOLÁSA     | KIKAPCSOLÁSA | A #HIÁNYZIK | AC power elveszett, a power biztonságimásolat vagy AC power működő, és a vezérlő modulok megszűnt.|
|Zöld | TOVÁBB | TOVÁBB | A #HIÁNYZIK | OPS panel bekapcsolás (5s) tesztelése állam|
|Zöld | KIKAPCSOLÁSA | KIKAPCSOLÁSA | A #HIÁNYZIK | Kiemelt a jó az összes függvény|
|Zöld | TOVÁBB |A #HIÁNYZIK | PCM hibafa LED, ventilátor hibafa LED | Bármely PCM hiba, ventilátor hiba, vagy a hőmérsékleti keresztül|
| Zöld | TOVÁBB | A #HIÁNYZIK | I/O modul LED  | Bármely vezérlő modul hiba|
| Zöld | TOVÁBB | A #HIÁNYZIK | A #HIÁNYZIK | Ház logika hiba|
| Zöld | Flash | A #HIÁNYZIK | A modul állapota LED vezérlő modulra. PCM hibafa LED, ventilátor hibafa LED | Ismeretlen vezérlő modul típusa telepítette, I2C bus hiba vezérlő modul elengedhetetlen termék VPD adatainak konfigurációs hiba |

## <a name="power-cooling-module-pcm-indicator-leds"></a>A Power hűtési modul (PCM) jelző LED   

HATVÁNY hűtési modul (PCM) jelző LED az elsődleges háza vagy EBOD ház hátulján minden PCM modulban található. Ez a témakör azt ismerteti, hogy a következő LED használata a Lync-StorSimple eszköze állapotát.  

- Az elsődleges ház az PCM LED
- PCM LED-a EBOD ház

## <a name="pcm-leds-for-the-primary-enclosure"></a>Az elsődleges ház az PCM LED  

A StorSimple eszköz és egy további elem 764W PCM modul tartalmaz. Az alábbi ábrán a az eszköz LED-panelen.  

   ![Kattintson az elsődleges ház PCM LED][2]

A jelmagyarázat LED:

1. AC áramszünet
2. Ventilátor hiba
3. Elem hiba
4. PCM OK GOMBRA.
5. Adatközpont hiba
6. Jó elem  

A PCM állapotának fel van tüntetve a LED panelen. Az eszköz PCM LED panelen hat LED tartalmaz. Négy ezek LED power ellátására és a ventilátor állapotának megjelenítése A hátralévő két LED a PCM biztonsági elem moduljának állapotát jelzi. Az alábbi táblázatokban határozza meg a PCM állapotának is használhatja.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM jelző LED power ellátási és ventilátor
| Állapot | PCM OK (zöld) | AC fail (sárga) | Ventilátor fail (sárga) | Adatközpont fail (sárga) |
|--------|----------------|-----------------------|------------------|----------------------|
| Nincs AC power (a ház) | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA|
| Nincs AC power (csak ezt a PCM) | KIKAPCSOLÁSA | TOVÁBB | KIKAPCSOLÁSA | TOVÁBB |
| AC bemutató PCM be -, az OK gombra     | TOVÁBB | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA |
| Fail: PCM (ventilátor fail) | KIKAPCSOLÁSA | KIKAPCSOLÁSA | TOVÁBB | A #HIÁNYZIK |
| PCM hibafa (felett amp fölé feszültség aktuális keresztül) | KIKAPCSOLÁSA | TOVÁBB | TOVÁBB | TOVÁBB |
| PCM (ventilátor hibatűrést kívül) | TOVÁBB | KIKAPCSOLÁSA | KIKAPCSOLÁSA | TOVÁBB |
| Készenléti | Villogó | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA |
| PCM belső vezérlőprogram letöltése | KIKAPCSOLÁSA | Villogó | Villogó | Villogó |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>A biztonsági mentés elem LED PCM mutató  

| Állapot | Jó elem (zöld) | Elem hibafa (sárga) |
|--------|----------------------|-----------------------|
| Elem nem szerepel | KIKAPCSOLÁSA | KIKAPCSOLÁSA |
| Bemutató és feltöltött elem | TOVÁBB | KIKAPCSOLÁSA |
| Elem töltés alatt vagy karbantartási lezárása | Villogó | KIKAPCSOLÁSA |
| Elem "finom" hiba (helyreállítható) | KIKAPCSOLÁSA | Villogó |
| Elem "kemény" hiba (nem helyreállítható) | KIKAPCSOLÁSA | TOVÁBB |
| Elem ártalmatlanított | Villogó | KIKAPCSOLÁSA |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM LED-a EBOD ház  

A EBOD ház egy 580W PCM és nincs további elem van. A EBOD ház PCM munkaablakbeli jelző LED csak az a power éppen a Kellékek és a ventilátor tartalmaz. Az alábbi ábrán a következő LED.

   ![Kattintson a EBOD ház PCM LED][3] 
 
Az alábbi táblázat segítségével határozza meg a PCM állapotát.  

| Állapot | PCM OK (zöld) | AC fail (sárga) | Ventilátor fail (sárga) | Adatközpont fail (sárga) |
|--------|---------------|------------------------|------------------|----------------------|
| Nincs AC power (a ház) | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA |
| Nincs AC power (csak ezt a PCM) | KIKAPCSOLÁSA | TOVÁBB | KIKAPCSOLÁSA | TOVÁBB |
| AC bemutató PCM tovább – az OK gombra | TOVÁBB | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA |
| Fail: PCM (ventilátor fail) | KIKAPCSOLÁSA | KIKAPCSOLÁSA | TOVÁBB | X |
| PCM hibafa (felett amp feszültség fölé aktuális keresztül | KIKAPCSOLÁSA | TOVÁBB | TOVÁBB | TOVÁBB |
| PCM (ventilátor hibatűrést kívül) | TOVÁBB | KIKAPCSOLÁSA | KIKAPCSOLÁSA | TOVÁBB |
| Készenléti modell | Villogó | KIKAPCSOLÁSA | KIKAPCSOLÁSA | KIKAPCSOLÁSA |
| PCM belső vezérlőprogram letöltése | KIKAPCSOLÁSA | Villogó | Villogó | Villogó |

## <a name="controller-module-indicator-leds"></a>Vezérlő modul jelző LED  

Az StorSimple eszköz LED tartalmaz, az elsődleges vezérlő és a EBOD vezérlő modulokat.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Az elsődleges vezérlő LED figyelése
Az alábbi ábra segítségével azonosítani tudja a LED az elsődleges vezérlő. (Minden összetevő szerepelnek, a tájolás támogatási.)  

   ![Figyelés LED - elsődleges vezérlő][4]
 
Az alábbi táblázat segítségével határozza meg, hogy a vezérlő modul megfelelően működik-e.  

### <a name="controller-indicator-leds"></a>Vezérlő jelző LED  

| LED | Leírás                                                                            
|---- | ----------- |
| Azonosító LED (kék) | Azt jelzi, hogy a modul azonosítjuk. A kék LED van villogó futó vezérlőn, ha a vezérlő szerepel-e az active vezérlő, és a másikkal a készenléti vezérlő. További tudnivalókért lásd: [azonosítása az aktív vezérlő az eszközön](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Hibafa LED (sárga) | Azt jelzi, hogy a vezérlő meghibásodása.        
| OK-LED (zöld) | Állandó zöld azt jelzi, hogy a vezérlő az OK gombra. Egy vezérlő VPD konfigurációs hiba zöld jelzi. |
| Társítások tevékenység LED (zöld) | Állandó zöld jelzi az aktuális tevékenység nélküli kapcsolat. Zöld azt jelzi, a kapcsolat folyamatban lévő tevékenységet. |
| LED Ethernet állapota | Jobb oldalán hivatkozás/hálózati tevékenység jelöli: (villogó zöld) aktív (stabil zöld) hivatkozásra hálózati tevékenység. Bal oldali jelzi a hálózat sebességétől: (sárga) 1000 Mb/s (zöld) 100 Mb/s és (ki) 10 MB-os/s. Attól függően, hogy a összetevő modell mindezek villogó előfordulhat, hogy akkor is, ha a hálózati kapcsolaton nincs engedélyezve. |
| BEJEGYZÉS LED | Indítási előrehaladását jelzi, amikor az adatkezelő be van kapcsolva. Az StorSimple eszköz nem sikerül indítsa el, ha a LED segítséget nyújt a Microsoft Support melyik közülük a a indítási folyamat a hiba történt. |

>[AZURE.IMPORTANT] 
A hiba LED ég van, ha probléma van a vezérlő modul, amely esetleg elhárítható a vezérlő újraindításával. Kérjük, forduljon a Microsoft Support, ha a vezérlő újraindítás nem oldja meg a probléma.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>A EBOD (EBOD ház) LED figyelése  

A 6 Gb/s Társítások EBOD vezérlők mindegyikének LED jelölő állapota az alábbi ábrán látható módon.  

  ![Figyelés LED - EBOD ház][5]

Az alábbi táblázat segítségével határozza meg, hogy megfelelően működik-e a EBOD vezérlő modulra.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD vezérlő modul jelző LED  

|Állapot | I/O modul OK (zöld) | I/O modul hibafa (sárga) | A Host port tevékenység (zöld) |
|-------|----------------------|-------------------------------|----------------------------|
| Az OK gombra a vezérlő modul | TOVÁBB | KIKAPCSOLÁSA | - |
| Vezérlő modul hiba | KIKAPCSOLÁSA | TOVÁBB | - |
| Nincsenek külső host port kapcsolatban | - | - | KIKAPCSOLÁSA |
| Külső host port kapcsolat – nincs tevékenység | - | - | TOVÁBB |
| Külső host port kapcsolat - tevékenység | - | - | Villogó |
| Vezérlő modul metaadatok hiba | Villogó | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Meghajtón jelző LED az elsődleges ház és EBOD ház

A StorSimple eszköznek lemezmeghajtók az elsődleges ház és a EBOD ház is található. Az egyes meghajtók tartalmazza figyelése jelző LED, a jelen szakaszban ismertetett. 

A lemezmeghajtók, a zöld a meghajtó állapotát jelzi LED és a vörös-sárga LED csatlakoztatott minden meghajtó carrier modulban elején. Az alábbi ábrán a következő LED.

  ![Meghajtón LED][6]
 
Az alábbi táblázat segítségével határozza meg az egyes viszont alkalmazásával miként változna meg az általános első panelen LED állapot lemezmeghajtók az állapotát.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Meghajtón mutató LED a EBOD ház  

| Állapot | Tevékenység OK LED (zöld) | Hibafa LED (vörös-sárga) | Kapcsolódó LED ops panel |
|-------|--------------------------|----------------------|-------------------------|
| Nincs telepítve meghajtó | KIKAPCSOLÁSA | KIKAPCSOLÁSA | Nincs lehetőség |
| A telepített és műveleti meghajtó | A tevékenység ki-be kapcsoló villogó | X | Nincs lehetőség |
| SCSI ház szolgáltatások (SES) eszközt identitás beállítása | TOVÁBB | 1 másodperc a/1 ki a második villogó | Nincs lehetőség |
| SES eszköz hibafa bittel | TOVÁBB | TOVÁBB | Logikai hibafa (piros) |
| Vezérlő áramkör áramszünet | KIKAPCSOLÁSA | TOVÁBB | A modul hibafa (piros) |

## <a name="audible-alarms"></a>Hallható riasztás  

Egy StorSimple eszközt társított az elsődleges ház és a EBOD ház is hallható riasztások tartalmazza. Hallható figyelmeztető mindkét letölthetés elülső (más néven a ops panelen) lapján található. A hallható riasztás azt jelzi, hogy mikor hibafa feltétel szerepel. Az alábbi feltételek aktiválja a riasztás:  

- Hibafa ventilátor vagy sikertelen
- Feszültség kívül esik
- A hőmérsékleti feltétel vagy keresztül
- Hőmérsékleti túlcsordulás
- Hibafa rendszer
- Logikai hiba
- A Power ellátási hiba
- Hűtési modul (PCM) hatványát eltávolítása  

Az alábbi táblázat ismerteti a különböző riasztás államok.  

### <a name="alarm-states"></a>Riasztás államok  

| Riasztás állam | Művelet | Művelet végrehajtása a lenyomott Elnémítás gomb |
|------------|---------|---------------------------------|
| S0 | Normál mód: Csendes | Kétszer hangjelzés |
| S1 | Hibafa mód: 1 másodperc a/1 második kikapcsolása | Áttűnés S2 vagy S3 (lásd: Megjegyzés) |
| S2 | Emlékeztetheti mód: szakaszos hangjelzés | Nincs lehetőség |
| S3 | Elnémított mód: Csendes | Nincs lehetőség |
| S4 | Kritikus hibafa mód: folytonos riasztás | Nem érhető el: nem aktív elnémítása |

> [AZURE.NOTE] 

>  - Riasztás állapotban S1 nem lenyomásával elnémítása belül 2 percig, az állapot automatikus átmenetek S2 vagy S3.  
>  - Riasztás államok S1 S4 vissza S0 után a hibafa feltétel nincs bejelölve.  
>  - Kritikus hibafa állam S4 meg lehet adni bármely más állam  

A program riasztás elnémíthatja a ops panelen az Elnémítás gomb megnyomásával. Automatikus némítása akkor fordul elő, két perc után, ha a elnémítása Váltás a manuális nem üzemelteti. Ha a riasztás némítva van, akkor is megmaradnak rövid időszakos hangjelzés jelzi, hogy a probléma továbbra is fennáll a hang. A riasztás csendes lesz, amikor az összes hibát nincs bejelölve.  

Az alábbi táblázat ismerteti a különböző riasztás feltételeket.  

### <a name="alarm-conditions"></a>Riasztás feltételek  

| Állapot | Súlyosságát | Riasztás | OPS LED panel |
|--------|---------|--------|----------------|
| PCM értesítés – egy egyetlen PCM Adatközpont power funkcióvesztés | Hibafa – redundancia adatvesztés nélkül | S1 | Hibafa modul|
|PCM értesítés – egy egyetlen PCM Adatközpont power funkcióvesztés | Hibafa – redundancia funkcióvesztés | S1 | Hibafa modul |
| PCM ventilátor fail: | Hibafa – redundancia funkcióvesztés | S1 | Hibafa modul |
| SBB modul PCM hibát észlelt | Hibafa | S1 | Hibafa modul |
| PCM eltávolítása | Konfigurációs hiba | Nincs lehetőség | Hibafa modul |
| Ház konfigurációs hiba | Hibafa – kritikus | S1 | Hibafa modul |
| Alacsony figyelmeztetés hőmérsékleti jelzése | Figyelmeztetés | S1 | Hibafa modul |
| Magas figyelmeztetés hőmérsékleti jelzése | Figyelmeztetés | S1 | Hibafa modul |
| Hőmérsékleti riasztás keresztül | Hibafa – kritikus | S1 | Hibafa modul |
| I2C bus hiba | Hibafa – redundancia funkcióvesztés | S1 | Hibafa modul |
| OPS panel kapcsolati hiba (I2C) | Hibafa – kritikus     | S1 | Hibafa modul |
| Vezérlő hiba | Hibafa – kritikus | S1 | Hibafa modul |
| SBB felület modul hiba | Hibafa – kritikus | S1 | Hibafa modul |
| SBB felület modul hibafa – nem működő modulok hátralévő | Hibafa – kritikus | S4 | Hibafa modul |
| SBB felület modul eltávolítása | Figyelmeztetés | Nincs lehetőség | Hibafa modul |
| Meghajtó power vezérlő hiba | Figyelmeztetés – meghajtó power adatvesztés nélkül | S1 | Hibafa modul |
| Meghajtó power vezérlő hiba | Hibafa – kritikus; meghajtó power funkcióvesztés | S1 | Hibafa modul |
| Eltávolított meghajtó | Figyelmeztetés | Nincs lehetőség | Hibafa modul |
| Nincs elegendő power érhető el | Figyelmeztetés | nincs lehetőség | Hibafa modul |

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [hardver-összetevő StorSimple és állapotát](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
