Erőforrás|Alapértelmezett korlát
---|---
Tárterület-fiókok előfizetésenként száma|200<sup>1</sup>
Tárterület-fiók egy TB|500 TB
Blob tárolók, BLOB, fájlmegosztások, táblák, sorban várakozó, személyek vagy tároló fiók üzenetek maximális száma|Csak korlát a 500 TB tárolókapacitással fiókkal rendelkezik
Egy egyetlen blob-tárolóhoz, táblázat vagy várólista maximális mérete|500 TB
Maximális száma a továbbfejlesztett fájlblokkolás blob blocks vagy blob összefűzése|50 000
Maximális méret egy szövegrészt a továbbfejlesztett fájlblokkolás blob vagy blob összefűzése|4 MB
Max blokk blob méretének vagy blob hozzáfűzése|50 000 x 4 MB (körülbelül 195 GB) 
Maximális méret, oldal blob |1 TB
Egy táblázat egység maximális mérete|1 MB
A tulajdonságok egy táblázat egységben maximális száma|252
Maximális méret: várólista üzenet|64 KB
A fájlmegosztás maximális mérete|5 TB
A fájlmegosztás fájlok maximális mérete|1 TB
A fájlmegosztás fájlok maximális száma|Csak korlát a fájlmegosztás 5 TB teljes kapacitásának
Max 8 KB IOPS jutó|1000
A fájlmegosztás fájlok maximális száma|Csak korlát a fájlmegosztás 5 TB teljes kapacitásának
Blob tárolók, BLOB, fájlmegosztások, táblák, sorok, személyek vagy tároló fiók üzenetek maximális száma|Csak korlát a 500 TB tárolókapacitással fiókkal rendelkezik
Az access tárolt házirendek per tároló, fájlmegosztás, táblázat vagy várólista maximális száma|5
Teljes kérése díjat (feltételezve, 1KB objektum méretének) tárterület-fiók|Legfeljebb 20 000 IOPS, a személyek másodpercenként vagy üzeneteket / szekundum
Egyetlen blob-tároló átviteli|Második vagy legfeljebb 500 kérelmek egy második felhasználónként legfeljebb 60 MB
Cél átviteli egyetlen sor (1 KB üzenetek)|Legfeljebb 2000 üzenetek / szekundum
Cél átviteli egyetlen tábla partíciót (1 KB egységek)|Legfeljebb 2000 személyek / szekundum
Egy fájlból megosztás cél átviteli|Legfeljebb 60 MB / szekundum
Max bejövő adatok<sup>2</sup> DB / tároló fiók (US régió)|Ha GRS/ZRS<sup>3</sup> engedélyezve, 20 GB/s-LRS 10 GB/s
Max kilépési<sup>2</sup> DB / tároló fiók (US régió)|Ha ZRS TS-GRS/GRS<sup>3</sup> engedélyezve, 30 GB/s-LRS 20 GB/s
Max bejövő adatok<sup>2</sup> DB / tárterület-fiókot (Európai és ázsiai régióban)|Ha GRS/ZRS<sup>3</sup> engedélyezve, 10 GB/s LRS az 5 GB/s
Max kilépési<sup>2</sup> DB / tárterület-fiókot (Európai és ázsiai régióban)|Ha ZRS TS-GRS/GRS<sup>3</sup> engedélyezve, a 15 GB/s-LRS 10 GB/s

<sup>1</sup> Ide tartoznak a Standard és a prémium tároló fiókok. Ha több mint 200 tároló fiókok van szüksége, ellenőrizze a keresztül [Azure támogatási](https://azure.microsoft.com/support/faq/)kérelmet. Az Azure tároló csoport fogja megvizsgálni az üzleti eset és jóváhagyhatja legfeljebb 250 tárterület-fiókok. 

<sup>2</sup> Minden adat (kérelmek) tárterület-fiókjába küldött *bejövő adatok* hivatkozik. Az összes adat (válasz) tárterület-fiókból fogadott *kilépési* hivatkozik.  

<sup>3</sup> Azure tároló replikációs beállításai a következők:

- **TS-GRS**: olvasásra geo felesleges tároló. Ha engedélyezve van a TS-GRS, kilépési célok kiegészítő helyén megegyeznek az elsődleges helye.
- **GRS**: Geo felesleges tárhelyet. 
- **ZRS**: zóna felesleges tároló. Csak a továbbfejlesztett fájlblokkolás BLOB használható. 
- **LRS**: helyileg felesleges tárhelyet. 

