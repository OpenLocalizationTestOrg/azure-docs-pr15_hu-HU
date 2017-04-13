<properties 
    pageTitle="Kibocsátó neve és a kibocsátó kulcs BizTalk szolgáltatások |} Microsoft Azure" 
    description="Megtudhatja, hogy miként kibocsátó neve és a kibocsátó kulcs lekérése szolgáltatás Bus vagy a hozzáférési beállítás (ACS) BizTalk szolgáltatások. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk szolgáltatások: Kibocsátó neve és a kibocsátó billentyűk

Azure BizTalk támogatási szolgálata a szolgáltatás Bus kibocsátó nevét és kibocsátó billentyűt, és a hozzáférés vezérlőnév kibocsátó és kibocsátó billentyűt. Kifejezetten:

Tevékenység | Mely kibocsátó neve és a kibocsátó billentyűt
--- | ---
A Visual Studio alkalmazás telepítése | Hozzáférés vezérlőnév kibocsátó és a kibocsátó kulcs
Az Azure BizTalk Services portál beállítása | Hozzáférés vezérlőnév kibocsátó és a kibocsátó kulcs
ÜZLETI jelfogók létrehozása a Visual Studio BizTalk kártya szolgáltatásokkal | Szolgáltatás Bus kibocsátó neve és a kibocsátó kulcs

Ez a témakör felsorolja a lépéseket a kibocsátó neve és a kibocsátó kulcs beolvasásához. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Hozzáférés vezérlőnév kibocsátó és a kibocsátó kulcs
A hozzáférés vezérlőnév kibocsátó és a kibocsátó kulcs használják a következőket:

- Az Azure BizTalk szolgáltatásalkalmazás létrehozása a Visual Studio: sikeresen Azure telepítéshez használni a BizTalk szolgáltatásalkalmazás a Visual Studióban, meg kell adni a hozzáférés vezérlőnév kibocsátó és kibocsátó billentyűt. 
- Az Azure BizTalk Services portál: Ha BizTalk szolgáltatás hozzon létre, és nyissa meg az BizTalk szolgáltatások portált, a hozzáférés vezérlőnév kibocsátó és a kiállító kulcsát automatikusan regisztrált a telepítések hozzáférés-vezérlés ugyanazokkal az értékekkel.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Másolja és illessze be a hozzáférés vezérlőnév kibocsátó és a kibocsátó kulcs

1. Jelentkezzen be az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. A bal oldali navigációs ablaktáblában válassza a **BizTalk szolgáltatásokat**.
3. Jelölje ki a BizTalk szolgáltatást. 
4. Jelölje ki a **Kapcsolat adatait** a tálcán. Az Access vezérlő Namespace alapértelmezett kibocsátó (kibocsátó neve), és az alapértelmezett kulcs (kibocsátó) jelennek meg, és is másolandó és beillesztendő.  

Összefoglalva:  
Kibocsátó neve alapértelmezett kibocsátó =  
Kiállító kulcsát = alapértelmezett billentyű


Emellett bejelölheti a hozzáférés-vezérlés értékek **Megnyitott ACS Kezelőportálja segítségével** :

1. Jelentkezzen be az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. A bal oldali navigációs ablaktáblában válassza a **BizTalk szolgáltatásokat**.
3. Jelölje ki a BizTalk szolgáltatást.
4. Kattintson a kapcsolat adatait gombra, és válassza a **Megnyitás ACS Kezelőportálja segítségével**.
5. A **Szolgáltatás beállításai**a portálon jelölje be a **Szolgáltatás identitások**. A szolgáltatás a hozzáférés vezérlőnév kibocsátó érték identitást jeleníti meg. Jelölje ki a szolgáltatás identitás hivatkozást a jelszót, amely a kibocsátó kulcs érték megjelenítéséhez. Az értékük másolható.<br/><br/>
Ha például **Szolgáltatás identitások**látni "tulajdonosa". "Tulajdonosa" a hozzáférési Vezérlőelemnév kibocsátó. Amikor a "tulajdonosa" hivatkozásra, a **jelszó**látható. Amikor a "Jelszó" hivatkozásra, az érték látható. A jelszó értéke a vezérlő kibocsátó hívóbetű.  

Összefoglalva:  
Kibocsátó neve = szolgáltatás azonosító neve  
Kiállító kulcsát = jelszó érték

A bal oldali navigációs, **Az Active Directory** beolvasni a hozzáférés-vezérlés értékek is választhat. 

> [AZURE.IMPORTANT]Amikor létrejön egy Access-vezérlő Namespace **Az Active Directory**, a szolgáltatás identitás használatával **nem** automatikusan létrejön. Amikor Ön kiépítése BizTalk szolgáltatás, az Access vezérlő Namespace szolgáltatás azonosító "tulajdonos nevű" (kibocsátó név), a jelszó (kulcs kibocsátó), és szimmetrikus kulcs automatikusan létrejön egy.<br /> 
[Hogyan: használata ACS alkalmazáskezelési szolgáltatás konfigurálása szolgáltatás azonosítási](http://go.microsoft.com/fwlink/p/?LinkID=303942) Access vezérlő szolgáltatás identitások több információt biztosít.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Szolgáltatás Bus kibocsátó neve és a kibocsátó kulcs
Szolgáltatás Bus kibocsátó neve és a kibocsátó kulcs BizTalk kártya szolgáltatások által használt. A Visual Studio BizTalk szolgáltatások projektben a kártya szolgáltatások BizTalk egy helyszíni sor üzleti (üzleti) rendszerhez való csatlakozáshoz használhatja. Szeretne csatlakozni, a LOB továbbítási létrehozása, és adja meg az üzleti rendszer adatait. Ha így tesz, is be a szolgáltatás Bus kibocsátó neve és a kibocsátó billentyűt.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>A szolgáltatás Bus kibocsátó neve és a kibocsátó kulcs beolvasása

1. Jelentkezzen be az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. A bal oldali navigációs ablaktáblában válassza a **Szolgáltatás Bus**.
3. Válassza ki a névteret. Menüsávján válassza a **Kapcsolat adatait**. Ekkor megjelenik az **Alapértelmezett kibocsátó** (kibocsátó neve) és a **Kulcs alapértelmezett** (kibocsátó billentyűt). Az értékük másolható.  

Összefoglalva:  
Kibocsátó neve alapértelmezett kibocsátó =  
Kiállító kulcsát = alapértelmezett billentyű

## <a name="next"></a>Következő
További Azure BizTalk Services témakörök:

-  [Az Azure BizTalk szolgáltatások SDK telepítése](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Oktatóprogram: Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Lásd még:
-  [Útmutató: állítsa be a szolgáltatás identitások ACS alkalmazáskezelési szolgáltatás használatával](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk szolgáltatások: Fejlesztőeszközök, Basic, normál és Premium kiadásában diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk szolgáltatások: Kiépítési használatával Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk szolgáltatások: Kiépítési állapot diagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk szolgáltatások: Irányítópult, a Monitor és a méret lapon](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk szolgáltatások: szabályozásának](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
