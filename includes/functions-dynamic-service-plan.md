## <a name="dynamic-service-plan"></a>Dinamikus szolgáltatáscsomagja

A dinamikus szolgáltatás megtervezése, a függvény alkalmazás függvény alkalmazás példányhoz fog kiosztani. Ha szükséges további előfordulásait kerül dinamikusan.
Azokban az esetekben is kiterjedhet a legtöbbet a rendelkezésre álló Azure infrastruktúra tétele több számítógépes erőforrások között. Ezenkívül a funkciók a teljes időt összehívások szükséges minimalizálása párhuzamosan fog futni. Minden egyes függvény végrehajtási ideje összeadva, másodpercben és összesíti a tartalmazó függvény által. A költség alapú a számú példányok, memóriaméret és teljes végrehajtás ideje szerint gigabájt másodpercben. Az egy kiváló lehetőséget, ha igényeinek megfelelően számítási szakaszos vagy a projekt időpontok gyakran nagyon rövid, lehetővé teszi, hogy csak számítási erőforrások fizet, ha a ténylegesen használatban vannak.   

### <a name="memory-tier"></a>A memória szint

Attól függően, hogy a függvény van szüksége lehet kiválasztani a függvény alkalmazásban (a függvények tároló) futtatásához szükséges memória
A memória méret beállításai a **1536 MB 128 MB**eltérőek. A kijelölt memóriaméret felel meg a munkakészlet szükség szerint a függvények a függvény alkalmazás részét képező. Ha a kód több memóriát, mint a kijelölt méret igényel, a függvény app-példány leáll a rendelkezésre álló memória hiánya miatt.

### <a name="scaling"></a>Méretezés

A függvények Azure platform fog igények felmérése a a forgalmat, a beállított eseményindítók alapján eldönteni, hogy mikor, ha át kívánja méretezni, felfelé vagy lefelé. A méretezés granularitása az függvény alkalmazás. Méretezés ebben az esetben azt jelenti, hogy hozzáadása egy függvény alkalmazás további előfordulásait. Intenzitásfokozatok előrehaladtával forgalom, függvény app-példányok letiltva-ahányat méretezés nulla sem futtatásakor.  

### <a name="resource-consumption-and-billing"></a>Erőforrás-felhasználás és számlázás

A dinamikus az erőforrás-hozzárendelés mód alkalmazás normál szolgáltatáscsomagja másképp befejeződött, ezért a felhasználás modell is különböző "fizetési-használati" modell lehetővé tevő. Felhasználás jelenteni kell egy függvény alkalmazást, csak az idő, amikor kódot végrehajtani.  
Azt számítja ki (GB) a memória méret megszorozza végrehajtási ideje belüli adott függvény alkalmazást futtató összes függvénye (másodpercben) mennyiségét. A mértékegység felhasználási lesz **GB-s (gigabájt másodperc)**.