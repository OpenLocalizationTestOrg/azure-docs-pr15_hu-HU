Az alábbi táblázat ismerteti a fő kvóták, korlátozások, alapértelmezett és az Azure ütemező szabályozás.

|Erőforrás|Korlát leírása|
|---|---|
|**Feladat mérete**|A feladat maximális mérete 16 KB. Ha egy helyezése vagy a javítás eredménye egy feladatot, ezek a korlátok-nál nagyobb, 400 Hibás kérés állapotkód adja vissza.|
|**URL-CÍMEK méretét kérése**|A kérelem URL-címe maximális mérete 2048 karakter.|
|**A fejléc összesített méretét**|Összesítő élőfej maximális mérete 4096 karakter.|
|**Élőfej száma**|Az élőfej maximális száma 50 fejlécek.|
|**Törzs mérete**|Törzsében maximális mérete 8192 karakter.|
|**Ismétlődés lett létrehozva**|Maximális ismétlődés létrehozva a 18 hónap.|
|**Idő elindítása**|Maximális "idő kezdési idő" érték a 18 hónap.|
|**Korábbi**|Maximális válasz korábbi tárolt törzse 2048 bájt.|
|**Gyakoriság**|Az alapértelmezés szerinti maximális gyakoriság kvóta pedig egy ingyenes feladat webhelycsoport az 1 óra 1 perc egy szabványos feladat gyűjteményben. A max gyakoriság argumentum konfigurálható egy feladat gyűjteményt lehet kisebb, mint a maximum mezőben. A feladat webhelycsoport összes feladat korlátozódik értéke a feladat gyűjtemény állítható be. Ha megpróbálja feladat létrehozásához kattintson a feladat gyűjtemény a maximális gyakoriság nél nagyobb gyakorisággal majd kérelem meghiúsul, egy ütközést 409 állapotkód.|
|**Feladatok**|Az alapértelmezés szerinti maximális feladatok kvóta pedig egy ingyenes feladat gyűjteményben 5 feladatok egy szabványos feladat gyűjteményben 50 feladatokat. Feladatok maximális száma egy feladat webhelycsoport konfigurálható. A feladat webhelycsoport összes feladat korlátozódik értéke a feladat gyűjtemény állítható be. Ha próbál létrehozni, mint a maximális feladatok kvóta további feladatokat, majd a kérelem sikertelen 409 ütközés állapotkód.|
|**Feladat Előzmények megőrzése**|Korábbi legfeljebb 2 hónap vagy az utolsó 1000 végrehajtások felfelé tárolja.|
|**Befejezett és hibás feladat megőrzése**|Befejezett és hibás feladatok 60 napig megőrződnek.|
|**Határidő**|Statikus (nem konfigurálható) kérelem időtúllépés 60 másodpercig HTTP műveletek nem. Hosszabb futó műveletek hajtsa végre a HTTP aszinkron protokollok; például egy 202 azonnal vissza, de folytatni a munkát a háttérben.|
