<properties
    pageTitle="A vezérlő szolgáltatást (Java) által visszaadott SAML megtekintése"
    description="Megtudhatja, hogy miként tekintheti meg a vezérlő szolgáltatást az Azure is Java-alkalmazások által visszaadott SAML."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>A vezérlő Azure szolgáltatás által visszaadott SAML megtekintése

Ez az útmutató megtudhatja, hogy miként tekintheti meg a mögöttes biztonsági állítás Korrektúra nyelvi SAML () az alkalmazás az Azure Access vezérlő szolgáltatást (ACS) adja vissza. Az útmutató épül a [webes felhasználók hitelesítést végezni az Azure Access vezérlő szolgáltatás használatával Holdas hogyan][] témakör, mert a kódot, amely a SAML információkat jelenít meg. A kész alkalmazást az alábbihoz hasonlóan néznek ki.

![Példa SAML kimeneti][saml_output]

ACS a további tudnivalókért lásd: a [következő lépésekkel](#next_steps) szakaszban.

> [AZURE.NOTE]
> Az Azure az Access Services vezérlő szűrő közösségi technológia előnézetét. Előzetes verziójú szoftvereihez, mint azt nem hivatalos támogatott Microsoft.

## <a name="prerequisites"></a>Előfeltételek

Ebből az útmutatóból a feladatok elvégzéséhez, fejezze be a minta how [to webfelhasználók hitelesítést végezni az Azure Access vezérlő szolgáltatás használatával Holdas][] , és ebben az oktatóprogramban az kiindulópontként használhatja azt.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>A Szerkesztés elérési utat és a telepítési összeállítás a JspWriter tár hozzáadása

Adja hozzá a **javax.servlet.jsp.JspWriter** osztály az összeállítás elérési utat és a telepítési összeállítás tartalmazó tárat. Tomcat használja, a tár esetén **jsp-api.jar**, amely a Apache **tár** mappájában található.

1. Holdas a Project Explorer, a kattintson a jobb gombbal a **MyACSHelloWorld**, **Összeállítása**kattintson, kattintson **konfigurálása összeállítása**, kattintson a **tárak** fülre, és kattintson a **Külső JARs hozzáadása**.
2. **JAR kiválasztása** párbeszédpanelen keresse meg a szükséges üveg, jelölje ki azt, és kattintson a **Megnyitás**gombra.
3. A még nyitva **MyACSHelloWorld tulajdonságai** párbeszédpanelen kattintson a **Telepítés összeállítás**.
4. A **Web telepítésének összeállítás** párbeszédpanelen kattintson a **Hozzáadás**gombra.
5. Az **Új összeállítás irányelvet** párbeszédpanelen kattintson a **Java összeállítása elérési út bejegyzéseket** , és kattintson a **Tovább gombra**.
6. Jelölje ki a megfelelő tárat, és kattintson a **Befejezés gombra**.
7. Kattintson az **OK gombra** a **MyACSHelloWorld tulajdonságai** párbeszédpanel bezárásához.

## <a name="modify-the-jsp-file-to-display-saml"></a>A SAML megjelenítendő JSP-fájl módosítása

Módosítsa a **index.jsp** használatához az alábbi kódot.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Futtassa az alkalmazást

1. Az alkalmazás az a számítógép irányító vagy az Azure- [webfelhasználók hitelesítést végezni az Azure Access vezérlő szolgáltatás használatával Holdas hogyan][]ismertetett lépésekkel telepíteni.
2. Indítsa el a böngészőben, és nyissa meg a webalkalmazást. A bejelentkezés után az alkalmazás, látni fogja a SAML adatait, beleértve az az identitás szolgáltató által biztosított biztonsági állítás.

## <a name="next-steps"></a>Következő lépések

További feltárása a ACS a használható funkciók körét, és kísérletezzen bonyolultabb esetek, lásd: [Access-vezérlő szolgáltatást 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access 2.0-s szolgáltatás]: http://go.microsoft.com/fwlink/?LinkID=212360
[Hogyan kívánja hitelesíteni a webes felhasználók Holdas használata az Access Azure vezérlő szolgáltatással]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 