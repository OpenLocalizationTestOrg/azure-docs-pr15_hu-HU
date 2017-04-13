<properties
    pageTitle="Excel-bővítmény gépi tanulási webszolgáltatásokhoz |} Microsoft Azure"
    description="Hogyan használhatók az Azure gépi tanulási webszolgáltatásokhoz közvetlenül az Excelben programozás nélkül."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-bővítmény Azure gépi tanulási webes szolgáltatásokhoz

Az Excel egyszerűen webszolgáltatásokhoz közvetlenül ne kelljen minden kódírás hívni.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Lépéseket, ha egy meglévő webes szolgáltatás használatával a munkafüzetben

1. Nyissa meg az [Excel mintafájl](http://aka.ms/amlexcel-sample-2), amely tartalmazza az Excel-bővítmény és a Titanic utasok adatait.
2. Válassza ki a webszolgáltatás kattintással-"Titanic túlélő előrejelző (Excel bővítmény minta) [eredmény]" Ebben a példában.

    ![Jelölje be a webes szolgáltatás][01]

3. Ezzel eljut az **Predict** szakasz.  A munkafüzet már tartalmaz, a mintaadatok, de egy üres munkafüzetet az Excel jelöljön ki egy cellát és kattintson a **használat mintaadatok**.
4. Jelölje ki az adatokat a fejlécek, majd kattintson a beviteli adatok tartomány ikonra.  Győződjön meg arról, hogy a "az adatok fejlécet tartalmaznak" jelölőnégyzet be van jelölve.
5. A **Kimenet**írja be a cella számát, amelyre a kimenet, például "H1" Itt kell.
6. Kattintson az **előrejelzési**.

    ![Szakasz előrejelzésére][02]

## <a name="steps-to-add-a-new-web-service"></a>Új webhely szolgáltatás hozzáadása lépései

Webszolgáltatás üzembe, vagy egy meglévő webes szolgáltatással. Webes szolgáltatás üzembe helyezése a további tudnivalókért lásd: [áttekintése a következő lépés 5: Azure gépi tanulási webszolgáltatás üzembe](machine-learning-walkthrough-5-publish-web-service.md).

Ismerkedés a webszolgáltatás az API billentyűt. Ha ezt megteheti klasszikus gépi tanulási webszolgáltatás egy új gépi tanulási webszolgáltatás közzététele e függ.

**Klasszikus webszolgáltatással** 

1. A gépi tanulási Studióban kattintson a bal oldali ablaktáblában a **WEBES szolgáltatások** szakaszra, és válassza ki a webszolgáltatás.

    ![Webszolgáltatás Studio kiválasztása][04]

2. Másolja a vágólapra a webszolgáltatás az API billentyűt.

    ![Studio API billentyűt][05]

3. A webszolgáltatás **az IRÁNYÍTÓPULT** lapon a **KÉRÉS/válasz** hivatkozásra.
4. Keresse meg a **Kérelem URI** szakaszban.  Másolja a vágólapra, és mentse az URL-címet.

>[AZURE.NOTE] Most már mindenre jelentkezzen be az [Azure gépi tanulási Web Services](https://services.azureml.net) portál beszerzése a klasszikus gépi tanulási webszolgáltatás API billentyűjét.

**Új webszolgáltatással**

1. Az [Azure gépi tanulási Web Services](https://services.azureml.net) portál **Webszolgáltatásokhoz**gombra, majd válassza ki a webszolgáltatás. 
2. Kattintson a **felhasználni**.
3. Keresse meg az **alapvető felhasználási Infó** szakaszban. Másolja a vágólapra, és mentse az **Elsődleges kulcs** és **Kérés-válasz** URL-CÍMÉT.


## <a name="steps-to-add-a-new-web-service"></a>Új webhely szolgáltatás hozzáadása lépései

1. Webszolgáltatás üzembe, vagy egy meglévő webes szolgáltatással. Webes szolgáltatás üzembe helyezése a további tudnivalókért lásd: [áttekintése a következő lépés 5: Azure gépi tanulási webszolgáltatás üzembe](machine-learning-walkthrough-5-publish-web-service.md).
2. Kattintson a **felhasználni**.
3. Keresse meg az **alapvető felhasználási Infó** szakaszban. Másolja a vágólapra, és mentse az **Elsődleges kulcs** és **Kérés-válasz** URL-CÍMÉT.
2. Az Excel programban, nyissa meg a **Webes szolgáltatások** szakasz (Ha **Predict** szakaszában, kattintson a vissza nyílra kattintva nyissa meg a webes szolgáltatások listáját).

    ![Nyissa meg a webes szolgáltatás kiválasztása][03]
    
3. Kattintson a **webes szolgáltatás hozzáadása**elemre.
4. Illessze be az Excel-bővítmény szövegmező **URL-CÍMÉT**.
5. Illessze be a szövegmezőbe, címkézett **API-ja kulcs**az API/elsődleges kulcs.
6. Kattintson a **hozzáadása**gombra.

    ![Klasszikus webszolgáltatás URL-CÍMEK és az API billentyűt.][06]

10. A webszolgáltatás használatához kövesse az előző útmutatást "Lépéseket, ha egy meglévő webes szolgáltatás használatával."

## <a name="sharing-your-workbook"></a>A munkafüzet megosztott használatára

Ha menti a munkafüzetet, a hozzáadott webszolgáltatásokhoz API-/ elsődlegeskulcs is menti. Ez azt jelenti, hogy meg kell csak a munkafüzet megosztása személyek megbízható.

Kérje meg a következő megjegyzésre szakaszban, vagy a [fórum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409)kérdéseit.

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
