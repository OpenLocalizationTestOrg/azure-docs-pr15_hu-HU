Erőforrás|Alapértelmezett korlát|Maximális érték
---|---|---
Egyetlen előfizetés az Azure Media Services (AMS) fiókok||25
Az eszközök / AMS fiók||1,000,000<sup>1</sup>
Feladatonkénti láncolt feladatok||30
Az eszközök / tevékenység||50
Feladatonkénti eszközök||100
Feladatok AMS számla száma ||50 000<sup>2</sup>
Egy tárgyi eszköz egy időben társított egyedi Locator||5<sup>4</sup>
Élő csatornák AMS számla száma </p></td>|5</p></td>|A #hiányzik<sup>1</sup>
Leállítva állam / csatorna programok </p></td>|50</p></td>|A #hiányzik<sup>1</sup>
Állam / csatorna futó programok </p></td>|3</p></td>|3
A folyamatos átvitelű lévő állam / AMS fiók fut</p></td>|2</p></td>|A #hiányzik<sup>1</sup>
Adatfolyam-mennyiség / végpont streaming </p></td>|10 </p></td>|A #hiányzik<sup>1</sup>
AMS fiók kódolási egység </p></td>|25</p></td>|A #hiányzik<sup>1</sup>
Tárterület-fiókok | |1000<sup>5</sup>
Házirendek || 1,000,000<sup>6</sup>

<sup>1</sup> kérhet kvóta korlátozásaik frissítéséhez nyissa meg a támogatási jegyek. Ne hozzon létre növelése korlátozások, helyette a támogatási jegy nyújt további AMS fiókokat.

<sup>2</sup> a telefonszám tartalmaz aszinkron, a kész, aktív és visszavont feladatokat. Nem tartalmazza a törölt feladatokat. A régi feladatok **IJob.Delete** vagy a **Törlés** HTTP-kérés segítségével törölheti.

<sup>3</sup> egy kérés lista feladat szervezetek, amikor legfeljebb 1000 visszatér egy kérelemre. Ha nyomon követheti a beküldött összes feladatot kell a legfelső/kihagyása [OData rendszer lekérdezési beállítások](http://msdn.microsoft.com/library/gg309461.aspx)ismertetett módon használhatja.

<sup>4</sup> Locator nem készült felhasználói hozzáférés-vezérlés kezelésére szolgáló. Különböző hozzáférési jogosultsággal, és az egyéni felhasználók adhat, digitális a tartalomvédelmi szolgáltatást (DRM) használja megoldások használja.

<sup>5</sup> a tárterület-fiókokat az azonos Azure előfizetésből kell lennie.

<sup>6</sup> van legfeljebb 1,000,000 házirendek különböző AMS házirendek (például megnevezés házirend vagy ContentKeyAuthorizationPolicy). Ugyanazon a házirend-Azonosítóval kell használni, ha az mindig ugyanazt a napok használ, és a hozzáférési engedélyeit / stb.
