<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet hibaelhárítási útmutatójának - szolgáltatás" 
   description="Azure mobil tetszés szerint elmélyedhet útmutatói – hibaelhárítás" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-service-issues"></a>Útmutató a szolgáltatással kapcsolatos problémák elhárítása

Az alábbiakban az Azure Mobile tetszés szerint elmélyedhet működésének előfordulhatnak lehetséges problémákat.

## <a name="service-outages"></a>Szolgáltatás kimaradások

### <a name="issue"></a>A probléma
- Problémák okozhatják Azure Mobile tetszés szerint elmélyedhet szolgáltatás kimaradások jelennek meg.

### <a name="causes"></a>Okok
- Problémák okozhatják Azure Mobile tetszés szerint elmélyedhet szolgáltatás kimaradások megjelenő számos különböző problémák okozhatják:
    - Megjelenő eredetileg rendszerből, az összes Azure Mobile tetszés szerint elmélyedhet elszigetelt problémák
    - Ismert problémák okozta kiszolgáló kimaradások (nem mindig jeleníti meg a kiszolgáló állapota):
    - Az ütemezési késések célcsoportkezelést, jelvény frissítés problémákat, hibák statisztika leállítása gyűjteni, leküldéses nem működik, API leállítása működik, új alkalmazások, vagy a felhasználók nem hozható létre, DNS-hibákat, és időtúllépést a felhasználói felület, az API-val és az alkalmazások az eszközön.
    - Felhőalapú függőség kimaradások [Azure szolgáltatás állapota](http://status.azure.com/)
    - Leküldéses értesítést szolgáltatások (PNS) függőség kimaradások
    - Alkalmazás-áruházból kimaradások

1) Ellenőrizze, hogy ha a probléma nem rendszeres lehetősége van tesztelje a át egy másik azonos függvény
   
   - Azure Mobile tetszés szerint elmélyedhet integrált alkalmazások
   - Próba-eszköz
   - Teszt eszközön, operációs rendszer verziója
   - Marketingkampány
   - Rendszergazdai felhasználói fiók
   - Böngésző (IE, Firefox, Chrome stb.)
   - Számítógép

2) Ha a probléma csak hatással van a felhasználói felület vagy a API teszteléséhez:

   - Tesztelje a Azure Mobile tetszés szerint elmélyedhet a felhasználói felület és a az Azure Mobile tetszés szerint elmélyedhet API ugyanazt a funkciót.

3) Annak ellenőrzése, ha a probléma van a mobiltelefon-hálózaton:

   - Tesztelje a közben WIFI keresztül, és közben a 3G mobiltelefon-hálózaton keresztül csatlakozik az internethez csatlakozik.
   - Győződjön meg arról, hogy a tűzfal nem blokkolja valamelyik Azure Mobile tetszés szerint elmélyedhet IP-címek vagy portokat.

4) Ha a probléma az eszközhöz ellenőrzése:

   - Ha az eszköz nem tud csatlakozni Azure Mobile tetszés szerint elmélyedhet a másik Azure Mobile tetszés szerint elmélyedhet integrált alkalmazások tesztelje.
   - Tesztelje, hogy az események, feladatok és összeomlik hozhat létre, amely látható legyen a felhasználói felület Azure Mobile tetszés szerint elmélyedhet a telefonról. 
   - Elküldheti leküldéses értesítéseket az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület az eszközre az eszköz azonosító alapján vizsgálata 

5) Annak ellenőrzése, ha a probléma az alkalmazást:

   - Telepítse, és tesztelje az alkalmazást egy irányító, hanem a fizikai eszközről:
   
6) Annak ellenőrzése, ha a probléma az operációs rendszer frissítése végfelhasználó eszközök, amelyekhez feloldásához SDK frissítésre van szükség:

   - Az alkalmazás, a rendszer a különböző verzióival eszközön tesztelése.
   - Győződjön meg arról, hogy a SDK a legújabb verzióját használja.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Kapcsolódási és helytelen adatokat kapcsolatos problémák

### <a name="issue"></a>A probléma
- Bejelentkezés az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület problémákat.
- Internetkapcsolat hibáinak az Azure Mobile tetszés szerint elmélyedhet API-val.
- A problémák alkalmazás információ címkék keresztül az eszköz API feltöltése.
- A problémák naplók vagy az exportált adatokat letöltésével Azure Mobile részvételét.
- Helytelen adatokat Azure Mobile tetszés szerint elmélyedhet a felhasználói felület látható.
- Helytelen adatokat Azure Mobile tetszés szerint elmélyedhet naplók látható.

### <a name="causes"></a>Okok
* Erősítse meg, hogy a feladat végrehajtásához szükséges engedélyekkel rendelkező fiókkal.
* Győződjön meg arról, hogy a probléma nem jelenik meg az egyik számítógépen vagy a helyi hálózaton elszigetelt.
* Győződjön meg arról, hogy az Azure Mobile tetszés szerint elmélyedhet szolgáltatás nincs jelentett kimaradások.
* Győződjön meg arról, hogy az alkalmazás információ címke fájlok kövesse az összes szabályok:
    - Használja az UTF8 karakterkészletet (az ANSI karakterkészlet nem támogatott).
    - Használjon vesszőt ",", az elválasztó karaktert (megnyithatja a .csv elválasztó karakter módosítása történő vesszőt igénylésével szolgáltatás "," egy másik, például egy pontosvessző karaktert a ";").
    - Az összes kisbetű logikai értékek használata "igaz" és "false értéket".
    - Használjon, amely kisebb, mint a maximális fájlméret 35MB-os fájlt.
 
