<properties 
    pageTitle="Áttekintése megfeleltetések nagyvállalati integrációs csomag |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="Térképek használata a nagyvállalati integrációs csomag és logika alkalmazással" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="learn-about-maps-and-the-enterprise-integration-pack"></a>További tudnivalók a térképekről és a nagyvállalati integrációs csomag

## <a name="overview"></a>– Áttekintés
Nagyvállalati integrációs térképeket használja az XML-adatokat az egyik formátumról más formátumban. 

## <a name="what-is-a-map"></a>Mi az térkép?
Térkép az XML-dokumentum, amely meghatározza, hogy a dokumentum mely adatokat kell átalakítani, adatait más formátumba. 

## <a name="why-use-maps"></a>Miért érdemes használni a térképek?
Tegyük fel, rendszeresen kapott B2B megrendelések vagy számlák egy ügyfelek, akik a dátumok YYYMMDD formátuma. Azonban a szervezet tárolt dátumokat MMDDYYY formátumban. Is használhatja a térképre *átalakítása* a YYYMMDD dátumformátumot a MMDDYYY előtt a sorrend vagy a számla részletei tárolása az ügyfél tevékenység adatbázis.

## <a name="how-do-i-create-a-map"></a>Hogyan térkép létrehozása?
A Visual Studio 2015 [Vállalati integrációs csomag](./app-service-logic-enterprise-integration-overview.md "megtudhatja, hogy a nagyvállalati integrációs csomag") lehetővé teszi a Biztalk integrációs projektek hozható létre.  Integráció térkép fájl létrehozása lehetővé teszi, vizuálisan megfeleltetéséhez elemeket két XML-séma fájlok között.  A projekt létrehozása, után egy XSLT kombináció kimeneti.

## <a name="how-to-upload-a-map"></a>Töltse fel a térkép hogyan?
Az Azure portálról:  
1. Válassza a **Tallózás gombra**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integráció** a szűrés a Keresés mezőbe írja be, és válassza a **Fiókok integráció** az eredménylistából     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Jelölje ki a **integrációs fiók** , amelybe hozzáadja a térképen  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Jelölje ki a **térképek** mozaikot  
![](./media/app-service-logic-enterprise-integration-maps/map-1.png)  
5. Válassza a **Hozzáadás** gombra, amely megnyitja a térképek lap  
![](./media/app-service-logic-enterprise-integration-maps/map-2.png)  
6. Írja be egy **nevet** a térképre, majd a térkép fájl feltöltése, jelölje be a mappa ikonja a **térkép** szövegdoboz jobb oldalán. A feltöltés után folyamat befejeződik, és válassza az **OK** gombra.  
![](./media/app-service-logic-enterprise-integration-maps/map-3.png)  
7. A térkép hozzáadott integrációs fiókjába. Egy, amely jelzi a sikeres vagy sikertelen a térkép fájl hozzáadása képernyő értesítést kap. Miután értesítést kap, jelölje ki a **térképek** mozaikot, majd jelenik meg az újonnan hozzáadott a térképek lap térkép:    
![](./media/app-service-logic-enterprise-integration-maps/map-4.png)  

## <a name="how-to-edit-a-map"></a>Hogyan térkép szerkeszteni?
Ha szerkeszteni szeretné egy térképen, akkor fel kell töltenie a kívánt módosításokat az új leképezés fájl. Először töltse le a térképet, és szerkesztheti. 

Kövesse ezeket a lépéseket követve töltse fel a létező leképezés felváltó új leképezést:  
1. Jelölje ki a **térképek** mozaikot  
2. Jelölje ki a térképet, amikor megnyitja a térképek lap szerkeszteni kívánt  
3. A **térképek** lap jelölje be a hivatkozás **frissítése**  
![](./media/app-service-logic-enterprise-integration-maps/edit-1.png)   
4. Jelölje ki a térkép fájlt szeretne feltölteni a fájl kiválasztása párbeszédpanel fel a megnyíló használatával, majd válassza a **Megnyitás** a fájlválasztó   
![](./media/app-service-logic-enterprise-integration-maps/edit-2.png)   
5. A térkép feltöltése után felbukkanó értesítést kap.    

## <a name="how-to-delete-a-map"></a>Hogyan törölheti a térkép?
1. Jelölje ki a **térképek** mozaikot  
2. Jelölje ki a törlendő, amikor megnyitja a térképek lap térkép  
3. Jelölje ki a hivatkozás **törlése**    
![](./media/app-service-logic-enterprise-integration-maps/delete.png)   
4. Győződjön meg arról, hogy valójában szeretné törölni a térképen.  
![](./media/app-service-logic-enterprise-integration-maps/delete-confirmation-1.png)   

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag")  
- [További tudnivalók a rendelkezést] (./app-service-logic-enterprise-integration-agreements.md "Megtudhatja, hogy nagyvállalati integrációs rendelkezést")  
- [További tudnivalók a átalakítások] (./app-service-logic-enterprise-integration-transform.md "Megtudhatja, hogy nagyvállalati integrációs átalakítások")  
