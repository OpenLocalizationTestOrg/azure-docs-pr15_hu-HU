1. A Visual Studio **Megoldás Intéző**, kattintson a jobb gombbal a projektet, és válassza ki **hozzáadása > Docker támogatási** a helyi menüből.

    ![Adja hozzá a Docker támogatási helyi menüje](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Egy ASP.NET Core Docker támogatás hozzáadása a project web eredményez hozzáadva a projekthez, például fájlok Docker írása, a Windows PowerShell-parancsfájlokat telepítési és Docker tulajdonság fájlok több Docker kapcsolódó fájlok hozzáadása. 

    ![Projekthez való hozzáadásának docker fájlok](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]A [Windows béta-Docker](https://beta.docker.com)használata esetén nyissa meg a Properties\Docker.props, távolítsa el az alapértelmezett értéket, és indítsa újra a csak akkor lépnek érvénybe értéket a Visual Studio.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
