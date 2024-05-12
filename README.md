# Pasos para crear una aplicación simple usando .Net 8 y EF

1. **Crear la carpeta de la solución:**
    ```bash
    mkdir Example1NetEf
    ```

2. **Crear la solución:**
    ```bash
    cd Example1NetEf
    dotnet new sln -n Example1DotNetEntityFramework
    ```

    **Una alternativa de 1 y 2 en un solo comando:**
    ```bash
    dotnet new sln -n Example1DotNetEntityFramework -o Example1NetEf
    ```

3. **Crear los proyectos:**
    ```bash
    dotnet new classlib -n MyProjectDotNet.Domain
    dotnet new classlib -n MyProjectDotNet.Data
    dotnet new classlib -n MyProjectDotNet.Domain.Common
    dotnet new console -n MyProjectDotNet.ConsoleApp
    ```

4. **Crear las clases Entidades en proyecto Domain:**
    ```bash
    cd MyProjectDotNet.Domain
    dotnet new class -n Common.BaseDomainModel -o Common
    dotnet new class -n Video
    dotnet new class -n Streamer
    dotnet new class -n Director
    dotnet new class -n Actor
    dotnet new class -n VideoActor
    ```

    La relación entre las clases es la siguiente:
    - Un Video tiene un Director y un director solo puede tener un video.
    - Un Video puede tener varios Actores y un Actor puede actuar en varios Videos.
    - Un Video es subido por un Streamer y un Streamer puede subir varios Videos.

5. **Definir el código de cada clase (ver el código fuente).**

6. **Crear la clase que representa el contexto de BD usando EF:**
    ```bash
    cd ..
    cd MyProjectDotNet.Data
    dotnet new class -n StreamerDbContext
    ```

7. **Agregar una referencia del proyecto MyProjectDotNet.Domain al proyecto MyProjectDotNet.Data:**
    - Asumiendo que estamos dentro de la raíz del proyecto MyProjectDotNet.Data:
    ```bash
    dotnet add reference ..\MyProjectDotNet.Domain\MyProjectDotNet.Domain.csproj
    ```

    - Asumiendo que estamos en la carpeta raíz de la solución:
    ```bash
    dotnet add MyProjectDotNet.Data reference MyProjectDotNet.Domain/MyProjectDotNet.Domain.csproj
    ```

8. **Crear la BD asumiendo que usamos SQL Server y MS-SSMS:**
    ```sql
    CREATE DATABASE StreamerDb;
    ```

9. **Obtenemos la cadena de conexión de la BD y la aseguramos, la necesitamos más adelante.**

10. **Instalamos las librerías necesarias en el proyecto MyProjectDotNet.Data:**
    ```bash
    dotnet add package Microsoft.EntityFrameworkCore.SqlServer
    dotnet add package Microsoft.EntityFrameworkCore.Tools
    ```

11. **Crear la clase StreamerDbContext en el proyecto MyProjectDotNet.Data:**
    ```bash
    dotnet new class -n StreamerDbContext
    ```

12. **Agregar una referencia del proyecto MyProjectDotNet.Domain y del proyecto MyProjectDotNet.Data al proyecto MyProjectDotNet.ConsoleApp:**
    ```bash
    cd ..
    cd MyProjectDotNet.ConsoleApp
    dotnet add reference ..\MyProjectDotNet.Domain
    dotnet add reference ..\MyProjectDotNet.Data
    ```

13. **Creamos el archivo ejecutable usando la técnica "Top-level statements" o "Sentencias de nivel superior":**
    ```bash
    dotnet new class -n Program
    ```

13. **Ejecutar las Migraciones de EntityFrameworkCore para que se genere la BD a partir de las clases del proyecto MyProjectDotNet.Domain y definidas como DbSet en la clase StreamerDbContext del proyecto MyProjectDotNet.Data:**
    1. **Instalar la herramienta de migraciones de EfCore:**
    ```bash
    dotnet tool install --global dotnet-ef
    ```

    2. **Crear las migraciones EF:**
    ```bash
    dotnet ef migrations add Migracion1StreamerDb --startup-project ..\MyProjectDotNet.ConsoleApp\MyProjectDotNet.ConsoleApp.csproj
    ```

14. **Verificamos en MS-SSMS si las tablas fueron creadas con sus respectivas columnas y relaciones.**

15. **Limpiar, recompilar y ejecutar la aplicación.**
