<properties
    pageTitle="Szerkesztés és Azure App szolgáltatásban Java API alkalmazások terjesztése"
    description="Megtudhatja, hogy miként egy Java API-alkalmazáscsomag létrehozása és Azure alkalmazás szolgáltatás üzembe."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Szerkesztés és Azure App szolgáltatásban Java API alkalmazások terjesztése

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Ebben az oktatóanyagban mutatja Java-alkalmazás létrehozása és Azure Alkalmazásindítónalkalmazások szolgáltatás API kell üzembe [mely számjegy]használatával. Ebben az oktatóprogramban az utasításokat bármely operációs rendszeren futó Java képes követheti. A kód ebben az oktatóanyagban [maven tesztelése]elkészült. [Jax Kompakt] lehet létrehozni a RESTful szolgáltatást használja, és jön létre a [Szerkesztő Swagger]segítségével [Swagger] metaadatok specifikációja alapján.

## <a name="prerequisites"></a>Előfeltételek

1. [Java Developer Kit 8] \(vagy újabb verzió)
1. A fejlesztői számítógépre telepítve [maven tesztelése]
1. [Mely számjegy] a fejlesztés számítógépre telepítve
1. [Microsoft Azure] fizetett vagy [ingyenes próba] -előfizetést
1. Egy HTTP vizsgálatot alkalmazást, például a [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Az API segítségével Swagger.IO scaffold

A-szerkesztőben swagger.io online, Swagger JSON vagy YAML kódot, amely a API felépítésének adhat meg. Ha befejezte a tervezett API felülete, exportálhatja a különféle platformokon és keretek kódját. A következő szakaszban a scaffolded kódot a mock funkcionalitást lesz módosítható. 

Ez a bemutató, amely meg fog illessze be a swagger.io szerkesztő, majd a REST API-végpont eléréséhez JAX kompakt felhasználásával kód létrehozása használandó Swagger JSON szervezethez elindul. Ezután meg fog a scaffolded kód szerkesztése mock adatok, amelyek a REST API-t egy adatok adatmegőrzési mechanizmusa felett beépített.  

1. Másolja az alábbi Swagger JSON-kódot a vágólapra.

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Nyissa meg az [Online Swagger szerkesztő]. Egyszer megnyitva, kattintson a **Fájl -> Beillesztés JSON** parancsot.

    ![JSON menü elem beillesztése][paste-json]

1. Illessze be a partnerek lista API Swagger JSON előbb másolt. 

    ![Swagger JSON kód beillesztése][pasted-swagger]

1. A dokumentáció oldalak és az API összefoglaló való megjelenítését a szerkesztőben megtekintése. 

    ![Nézet Swagger létrehozott dokumentumok][view-swagger-generated-docs]

1. A beállítással a **készítése a kiszolgáló-> JAX Kompakt** menü scaffold a kiszolgálóoldali kódot kell mock végrehajtása hozzáadása később szerkesztése. 

    ![Kód menüpont készítése][generate-code-menu-item]

    A kód jön létre, miután, fogja meg kell adni egy ZIP-fájl letöltése. Ez a fájl a Swagger kód nyilvántartás-készítő alkalmazás által scaffolded kódot tartalmazza, és az összes kapcsolódó hozza létre a parancsfájlok. Bontsa ki a teljes tár egy mappába a fejlesztés számítógépen. 

## <a name="edit-the-code-to-add-api-implementation"></a>A kód hozzáadása API végrehajtása szerkesztése

Ebben a szakaszban a Swagger által generált kód kiszolgálóoldali végrehajtása fogja cserélje az egyéni kódot. Az új kódot ad vissza egy összeomlanak a kapcsolattartó-egységekkel a hívó ügyfél számára. 

1. Nyissa meg a modell az *src/gen/java/io/swagger/modell* mappában található, a [Visual Studio kódot] vagy a kedvenc szövegszerkesztőben *Contact.java* fájlt. 

    ![Nyissa meg a partner modell fájl][open-contact-model-file]

1. Az **ügyfél** osztályozási adja hozzá a következő konstruktor. 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Nyissa meg a *ContactsApiServiceImpl.java* szolgáltatás végrehajtása fájl, amely az *src/fő/java/io/swagger/api/impl* mappájában található, a [Visual Studio kódot] vagy a kedvenc szövegszerkesztőben.

    ![Nyissa meg a kapcsolattartó szolgáltatás kód fájl][open-contact-service-code-file]

1. Az új kód egy mock végrehajtása hozzáadása a szolgáltatáskód felülírása a kódot a fájlban. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Nyisson meg egy parancssort, és módosítsa a címtár az alkalmazás gyökérmappájában.

1. Hajtsa végre a következő maven tesztelése parancsot a kód létre, és indítsa el a helyi meghajtóra rakodóhely alkalmazás kiszolgálót használ. 

        mvn package jetty:run

1. Meg kell jelennie a parancssorablakban rakodóhely port 8080 elindult a kód megfelelően. 

    ![Nyissa meg a kapcsolattartó szolgáltatás kód fájl][run-jetty-war]

1. Segítségével [Postman] kérelmének megszerezni a"az összes névjegy" 8080/api/partnerek API-módszernek.

    ![A névjegyek API][calling-contacts-api]

1. Segítségével [Postman] kérelmének a "adott partnerrel beolvasása" API-módszerrel található 8080/api/partnerek/2.

    ![A névjegyek API][calling-specific-contact-api]

1. Végezetül összeállítása a Java HÁBORÚ (webes archívum) fájlt a konzolban hajtja végre a következő maven tesztelése parancsot. 

        mvn package war:war

1. Ha elkészült a HÁBORÚ fájlt, akkor a **tároló** mappába kerül. Nyissa meg a **cél** mappába, és nevezze át a HÁBORÚ fájl **ROOT.war**. (Győződjön meg arról, hogy a hibás nagybetűhasználat megfelelő ebben a formátumban).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Végül hajtsa végre a következő parancsok végrehajtása az alkalmazás telepítése a HÁBORÚ fájlt az Azure **üzembe** mappa létrehozása a legfelső szintű mappából. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>A kimenet Azure alkalmazás szolgáltatás közzététele

Ebben a részben, fog az Azure-portálon új API-alkalmazás létrehozása című szolgáltatója Java-alkalmazásokat, hogy API-alkalmazás előkészítése, és telepítse az újonnan létrehozott HÁBORÚ fájlt az Azure alkalmazás szolgáltatás az új API-alkalmazás futtatásához. 

1. API új alkalmazás létrehozása az [Azure-portálon]kattintson az **Új webhely -> + Mobile-alkalmazás API >** menüpont, beírása az app adatait, majd **létrehozása**.

    ![Új API-alkalmazás létrehozása][create-api-app]

1. Az API-alkalmazás létrehozása után nyissa meg az alkalmazást a **Beállítások** lap, és válassza az **alkalmazás beállításai** parancsot. Jelölje ki a legújabb verzió Java lehetőségek, majd a **webes tároló** menüben válassza a legújabb Tomcat, és kattintson a **Mentés**.

    ![Az API-alkalmazás lap Java beállítása][set-up-java]

1. Kattintson a **telepítés hitelesítő adatok** beállítások menüelemre, és adja meg a felhasználónevet és jelszót, a fájlok közzététele az API-alkalmazást használni kívánja. 

    ![Telepítési hitelesítő adatok beállítása][deployment-credentials]

1. Kattintson a **telepítési forrást** beállításai menüpontra. Egyszer megnyitva, kattintson az **Adatforrás kiválasztása** gombra, válassza a **Helyi mely számjegy tárházba** lehetőséget, és kattintson **az OK**gombra. Azure, amelynek a társítás az API-alkalmazást futtató mely számjegy összegyűjti ez hoz létre. Minden alkalommal, amikor a *fő* részlege a mely számjegy tárházba visszavonhatatlanul kódot a kód teszi közzé a élő futó alkalmazás API-példány be. 

    ![Új helyi mely számjegy tár beállítása][select-git-repo]

1. Az új mely számjegy tárházba URL-cím másolása a vágólapra. A mentése állapotúként fontos a egy kis időt igénybe venni. 

    ![Az alkalmazás új mely számjegy összegyűjti beállítása][copy-git-repo-url]

1. Mely számjegy leküldéses a HÁBORÚ fájlt, az online tárházba. Ehhez nyissa meg a korábban létrehozott, hogy a kódot a tárat, az alkalmazás szolgáltatást futtató felfelé véglegesítése egyszerűen után is **üzembe** mappába. Miután az konzol ablakába, és a mappába, ahol a webalkalmazás mappában található átláthassák, hiba az alábbi mely számjegy parancsokat a folyamat elindítása és fire kikapcsolása a telepítést. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Miután a **leküldéses** kérés elküldésekor, a jelszót, a telepítési hitelesítő adatok korábban létrehozott kell adnia. Miután megadta a hitelesítő adatait, meg kell jelennie a megjelenítése, hogy telepítették-e a frissítés portált.

1. Ha még egyszer Postman találati az újonnan rendszerbe API-alkalmazás Azure alkalmazás szolgáltatás fut, látni fogja, hogy a vezérlő viselkedése egységes és, hogy most azt ad eredményül kapcsolattartói adatok meg a várt módon, és használja a Swagger.io egyszerű kód módosításai scaffolded Java-kód. 

    ![A Java partnerek REST API live használata Azure-ban][postman-calling-azure-contacts]

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben volt tud elindulni Swagger JSON-fájllal, és néhány scaffolded a Swagger.io szerkesztő nyert Java-kódot. Itt a egyszerű módosításokat, és egy mely számjegy üzembe folyamat a problémát a funkcionális API-at, a Java nyelven írt megszakításához. A következő oktatóanyag mutatja, hogy hogyan [API-alkalmazások CORS használatáról JavaScript-ügyfelek felhasználása][App Service API CORS]. A sorozat oktatóanyagai újabb megjelenítése hitelesítési és engedélyezési megvalósításáról.

Ez a példa épülnek, akkor talál további információt a [Java tárolás SDK] a probléma továbbra is fennáll a JSON BLOB. Másik lehetőségként, ha a [Dokumentum DB Java SDK] segítségével Azure dokumentum DB a kapcsolattartó-adatok mentése. 

Azure Java használatával kapcsolatos további tudnivalókért lásd: a [Java Developer Center].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure portál]: https://portal.azure.com/
[A dokumentum DB Java SDK]: ../documentdb/documentdb-java-application.md
[ingyenes próbaverzió]: https://azure.microsoft.com/pricing/free-trial/
[Mely számjegy]: http://www.git-scm.com/
[Java Developer Center]: /develop/java/
[Java Developer Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax Kompakt]: https://jax-rs-spec.java.net/
[Maven tesztelése]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger szerkesztő]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[SDK Java tárhely]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Szerkesztő swagger]: http://editor.swagger.io/
[Visual Studio kódot.]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
