
# <a name="azure-and-internet-of-things"></a>Azure és Internet dolog

Üdvözöljük a Microsoft Azure és az Internet dolog (IoT). Ez a cikk bemutatja egy IoT megoldás lehet telepíteni a Azure szolgáltatások használata közös jellemzőit írja le egy IoT megoldási. IoT megoldásokat igényelnek biztonságos, eszközök, esetleg számozás milliós és a megoldás hátsó vége közötti kétirányú kommunikációt. A megoldás háttérkiszolgálón például automatikus, prediktív analytics felfedezhető a eszköz-felhő esemény adatfolyam elképzelések használhatja.

Borzas IoT Hub kulcsfontosságú esetén a IoT megoldási Azure szolgáltatások segítségével valósíthatja meg. IoT Suite lehetővé teljes, végpontok közötti, az architektúra IoT bizonyos esetekben. Például: 

- A *távoli figyelés* megoldás lehetővé teszi eszközök, például automatából állapotának figyelése. 
- A *prediktív karbantartás* megoldás segít felkészülni a karbantartó eszközök, például a távoli szivattyúzó állomásokon szivattyúk igényeit és nem tervezett állásidő elkerülése érdekében.

## <a name="iot-solution-architecture"></a>IoT megoldási

A következő ábrán egy tipikus IoT megoldás architektúrájának. A diagram minden egyes Azure szolgáltatások nevét tartalmazza, de a IoT általános megoldási kulcs elemeit írja le. Ebben a rendszerben IoT eszközök felhő átjáró küldenek adatokat gyűjteni. A felhő átjáró elérhetővé teszi az adatok más ahol adatok más sor üzleti alkalmazások vagy emberi szereplők irányítópult vagy más bemutató eszköz keresztül átadott a háttérszolgáltatások feldolgozását.

![IoT megoldási][img-solution-architecture]

> [AZURE.NOTE] Részletes ismertető IoT architektúra, lásd: a [Microsoft Azure IoT referencia-architektúra][lnk-refarch].

### <a name="device-connectivity"></a>Eszköz-kapcsolat

A IoT megoldási eszközök küldenie távmérő, például az érzékelő mérési szivattyúzó állomásról, tárolás és feldolgozás céljából a felhő végponthoz. Prediktív karbantartás esetén a háttér meghatározására, amikor egy adott szivattyú igényel karbantartást használhatja az adatfolyam a mozgásérzékelő-adatokat. Eszközök is kap, és üzeneteit olvasó a felhő végpont által felhő-eszköz parancsok válaszolni. Például a prediktív karbantartás esetén a megoldás háttér lehet parancsok küldhetők egyéb szivattyú forgalom átirányítását csak megelőző karbantartás érdekében ellenőrizze, hogy a karbantartási mérnök érkezésekor őt az első lépések a kezdéshez szivattyúzó állomáson.

A legnagyobb kihívása IoT projektek egyik biztonságosan és megbízhatóan csatlakozás eszközök a megoldás háttér. IoT eszközök rendelkeznek eltérő jellemzőkkel rendelkeznek, mint például a böngészők és a mobil alkalmazások más ügyfelek. IoT eszközök:

- Nincs emberi operátor beágyazott rendszerek gyakran történik.
- Telepíthető a távoli helyeken, ahol a fizikai hozzáféréssel drága.
- Csak az lehet a megoldás háttér keresztül érhető el. Nincs más mód az eszköz interaktív.
- Előfordulhat, hogy korlátozott teljesítmény és az erőforrások feldolgozása.
- Szakaszos, lassú vagy drága kapcsolat lehet.
- Lehet, hogy kell használni a saját, egyéni vagy iparág-specifikus alkalmazások protokolljai.
- Létrehozhat nagyszámú népszerű hardver és szoftver rendszerek segítségével.

A fenti követelményeken túl bármely IoT oldatot kell is nyilvánítania skála, biztonságot és megbízhatóságot. Kapcsolódás követelményeit eredő kemény és időigényes, a hagyományos technológiák, mint például a webes konténerek és alkuszok messaging megvalósításához. Borzas IoT Hub és a IoT eszköz SDK megkönnyíti megvalósítására, amelyek megfelelnek ezeknek a követelményeknek.

Az eszköz közvetlenül kommunikálhat a felhő átjáró végpont, vagy ha az eszköz nem használja a kommunikációs protokollok, amely támogatja a felhő átjáró, akkor egy köztes átjárón keresztül kapcsolódhatnak. Például az [Azure IoT protokoll gateway] [ lnk-protocol-gateway] protokoll fordítási hajtható végre, ha az eszköz nem használható a protokollok által támogatott IoT Hub.

### <a name="data-processing-and-analytics"></a>Adatfeldolgozás és analytics

A felhőben egy biztonsági megoldás véget IoT, ahol az adatfeldolgozás a legtöbb történik, szűrést és távmérő összesítésével és egyéb szolgáltatások irányítás. Vissza a IoT megoldás Befejezés:

- Az eszközök távmérő méretekben megkapja, és meghatározza, hogyan kell feldolgozni, és tárolja ezeket az adatokat. 
- Lehetővé teszi, hogy parancsokat küldhet a felhőből eszközre.
- Rendelkezést és vezérlő, mely eszközök alkalmazása megengedett csatlakozni az infrastruktúra lehetővé tevő eszköz regisztrációs szolgáltatásokat nyújt.
- Lehetővé teszi az eszközök állapotának nyomon követésére és megfigyelésére tevékenységüket.

A prediktív karbantartás esetén a megoldás háttér történelmi távmérő adatait tárolja. A háttér ezen adatok segítségével segítségével mintákat esedékes egy konkrét szivattyú karbantartás jelzi.

IoT megoldások automatikus visszacsatolással is tartalmazhat. Például egy analytics modulja a háttér azonosíthatja a távmérő, amely egy adott eszköz hőmérséklete szokásos működési szint felett. Az oldatot ezután küldhetnek parancs az eszközre, amely megfelelő javítóműveleteket hajt végre.

### <a name="presentation-and-business-connectivity"></a>Bemutató és üzleti kapcsolatok

A bemutató és üzleti adatkapcsolati réteg lehetővé teszi a végfelhasználók számára, hogy kezelje a IoT megoldás és az eszközök. Lehetővé teszi a felhasználók megtekintsék és a saját eszközök gyűjtött adatok elemzése. Ezek a nézetek irányítópultok vagy mindkét történelmi adatokat megjelenítő jelentések BI vagy közel valós idejű adatok formájában is igénybe vehet. Például az üzemeltető adott szivattyúzó állomás állapotának ellenőrzése és tekintheti meg ezeket az értesítéseket a rendszer által felvetett. Ebben a rétegben is lehetővé teszi a IoT megoldás háttér vállalati üzleti folyamatok, illetve munkafolyamatok társításokkal meglévő sor üzleti alkalmazásokkal. A prediktív karbantartás megoldás például könyvek szivattyúzó állomás meglátogatására, amennyiben a megoldás azonosít egy igénylő karbantartási szivattyú szakmérnök ütemezési rendszer integrálható.

![IoT megoldás irányítópult][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
