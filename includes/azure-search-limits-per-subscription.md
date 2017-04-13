Létrehozhat egy előfizetését, egy adott réteg, csak minden réteg engedélyezett szolgáltatások számának korlátozza a kiépítve mindegyik belül több szolgáltatásban. Ha például úgy lehetett létrehozni felfelé 12, az egyszerű réteg és egy másik 12 szolgáltatásokat, a S1 réteg ugyanabban az előfizetésben belül. Rétegek kapcsolatos további tudnivalókért lásd: [választható Termékváltozatot vagy réteg Azure keresés](../articles/search/search-sku-tier.md).

Maximális szolgáltatás határértékeket is léptethető kérésre. Ha szüksége van az azonos előfizetés belül több szolgáltatás, lépjen kapcsolatba Azure támogatás.

Erőforrás|Ingyenes|Egyszerű|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Maximális szolgáltatások |1 |12 |12  |6 |6 |6 
Maximális méret SZU <sup>2</sup>|A #hiányzik <sup>3</sup>|3 SZU <sup>4</sup> |36 SZU|36 SZU|36 SZU|12 SZU, 3 SZU <sup>5</sup>

<sup>1</sup> S3 HD nem támogatja az [Indexelő](../articles/search/search-indexer-overview.md) adott időben. 

<sup>2</sup> keresés (felső) egységek számlázható-mennyiség / szolgáltatás, *kópia* vagy a *partíciót*kiosztva. Mindkét erőforrások szükséges tárhely, az indexelés és a lekérdezés műveletek. Érvényes kombinációi, amelyek kapcsolatának csoportban a maximális számával kapcsolatos további információért olvassa el a [Méretarány erőforrásszintek az index és a lekérdezés munkaterhelésekből](../articles/search/search-capacity-planning.md)című témakört. 

<sup>3</sup> ingyenes több előfizetők által használt megosztott erőforrásokat alapul. Ez a szint a nincsenek olyan egyéni előfizető dedikált erőforrások. Emiatt maximális méret van megjelölve nem alkalmazható.

<sup>4</sup> a basic egy rögzített partíciót tartalmaz. A réteg, a további SUs további kópiák oszt ki megnövelt lekérdezés munkaterhelésekből segítségével.

<sup>5</sup> S3 HD szerkezete különböző terhelés megengedett összes lehetséges kombinációinak számát tekintve. Replikáinak 12 legfeljebb is. A partíciók a maximális érték 3.




