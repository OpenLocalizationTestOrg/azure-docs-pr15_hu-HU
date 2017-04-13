<properties
    pageTitle="Lépés a 6: A gépi tanulási webszolgáltatás elérésére |} Microsoft Azure"
    description="Az elektronikus oktatóanyagok elkészítése prediktív megoldás útmutató 6 lépésénél: az aktív Azure gépi tanulási webszolgáltatás elérése."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>6 áttekintése a következő lépés: A Azure gépi tanulási webszolgáltatás elérésére

Ez a forgatókönyv, [az Azure gépi tanulási prediktív analytics megoldást fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md) az utolsó lépésben


1.  [Gépi tanulási munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Töltse fel a meglévő adatok](machine-learning-walkthrough-2-upload-data.md)
3.  [Hozzon létre egy új kísérlet](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Képzése és a modellek felmérése](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [A webszolgáltatás terjesztése](machine-learning-walkthrough-5-publish-web-service.md)
6.  **A webszolgáltatás elérésére**

----------

Az előző lépésben az útmutató azt, hogy a hitelkártya kockázat előrejelzési modellt webszolgáltatás telepítését. Most már felhasználóknak kell is adatok küldése és fogadása az eredményeket. 

A webszolgáltatás az Azure webszolgáltatás, amely a vezérlőn és adatok REST API-k használata a két módszer egyikével:  

-   **Kérés/válasz** – a felhasználó küld egy vagy több adatsort hitelképesség a szolgáltatás-HTTP protokoll használatával, és az szolgáltatás válaszol bíró egy vagy több eredmény.
-   A szolgáltatás elküldi a blob helyét és tárolja az egy vagy több adatsort hitelképesség-Azure blob **Köteg adatvégrehajtás** - a felhasználó. A szolgáltatás scores az adatok minden sor be a bemeneti blob tárolja az eredményeket egy másik blob és az URL-cím tároló adja eredményül.  

A leggyorsabban és legegyszerűbben úgy a webszolgáltatás elérésére, a Web App-sablonok a a [Microsoft Azure Web App piactéren](https://azure.microsoft.com/marketplace/web-applications/all/)elérhető keresztül.
Ezek a web app sablonok hozhat létre egyéni webalkalmazás tudja a webszolgáltatás bemeneti adatok, és milyen ad vissza. Az összes kell tennie a webszolgáltatás és az adatok eléréséhez, és a sablont tartalmaz, a többi.

A web app-sablonok használatával kapcsolatos további tudnivalókért lásd: [az Azure gépi tanulási webszolgáltatás web app sablonnal felhasználandó](machine-learning-consume-web-service-with-web-app-template.md).

A webszolgáltatás az R, C# és Python programnyelv, előírt starter kód használatával eléréséhez egyéni alkalmazás is készíthet.
[Hogyan felhasználása az Azure gépi tanulási webszolgáltatás, amely az közzétételét követően a kísérlet tanulási számítógépről](machine-learning-consume-web-services.md)a található összes adatának.
