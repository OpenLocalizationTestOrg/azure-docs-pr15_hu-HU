<properties 
    pageTitle="Cloud App felderítése rendszerbeállítások Proxy szolgáltatások |} Microsoft Azure" 
    description="Ez a témakör célja, hogy a lépések végrehajtásához szükséges port beállítása a Cloud App feltárás agent számítógépeken biztosítson." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App feltárás beállításjegyzék Proxy szolgáltatások beállításai

Alapértelmezés szerint a Cloud App feltárás agent használatára van beállítva csak a portokat 80 és 443-as. Ha azt tervezi, a Cloud App feltárás telepítése a proxykiszolgáló egy egyéni olyan portot (sem a 80, sem a 443-as) használó tartalmazó környezetben, állítsa be a port használni a ügynökök szeretne. A konfiguráció egy beállításkulcs alapul.


Ez a témakör célja, hogy a lépések végrehajtásához szükséges port beállítása a Cloud App feltárás agent számítógépeken biztosítson.



**Ha módosítani szeretné a számítógépen, amelyen a Cloud App felderítése agent által használt port, hajtsa végre az alábbi lépéseket:**


1. Indítsa el a Beállításszerkesztőt. <br> ![Futtatása](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Nyissa meg azt, vagy hozzon létre a következő beállításkulcsot: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud alkalmazás Discovery\Endpoint** 

3. Hozzon létre egy új néven **portok** **több** karakterláncértéket. ![Új](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. A **Több elem karakterlánc szerkesztése** párbeszédpanel megnyitásához kattintson duplán a portokat értéket.


5. Az érték adatok mezőbe írja be az alábbi értékeket, és adja hozzá a szervezet által használt összes egyéni portokat: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**Az 1110** <br><br>
![Több elem karakterlánc szerkesztése](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Kattintson **az OK gombra** kattintva zárja be a **Több elem karakterlánc szerkesztése** párbeszédpanel.



**További források**


* [Hogyan lehet felfedezése a szervezeten belüli használt unsanctioned felhő alkalmazások](active-directory-cloudappdiscovery-whatis.md) 


