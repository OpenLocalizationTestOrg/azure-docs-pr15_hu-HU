Azure fogja használni a virtuális környezetben, a következő prioritású Python verziójának:

1. a legfelső szintű mappa runtime.txt megadott verzió
1. Python beállításával a web app konfigurációban megadott verziót ( **Beállítások** > az Azure-portálon a web App**Beállításai** lap)
1. Python-2.7 az alapértelmezett érték, ha a fentiek egyike sem meg vannak adva

A tartalmát az érvényes értékek 

    \runtime.txt

a következők:

- Python 2.7.
- Python 3.4.

Ha a micro verziót (harmadik számjegy) van megadva, akkor figyelmen kívül hagyja.
