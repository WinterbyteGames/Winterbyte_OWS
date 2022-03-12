<p align="center">
    <br>
    <img src="img/Logo512pxWhite.png" alt="SabreDartStudios" width="120">
    <h2 align="center">Open World Server (OWS)</h2>
    <h4 align="center"><a href="http://www.sabredartstudios.com/">By SabreDartStudios</a></h4>
</p>
<h1></h1>
<p align="center">
    <img src="https://img.shields.io/travis/Dartanlla/OWS?style=flat-square">
    <a href="https://github.com/Dartanlla/OWS/blob/master/LICENSE">
        <img src="https://img.shields.io/github/license/Dartanlla/ows.svg?style=flat-square">
    </a>
    <a href="https://discord.gg/RxMkuJF">
        <img src="https://img.shields.io/badge/Discord-%237289DA.svg?style=flat-square&logo=discord&logoColor=white" alt="Join Discord">
    </a>
    <img src="https://img.shields.io/badge/unrealengine-%23313131.svg?style=flat-square&logo=unrealengine&logoColor=white">
    <img src="https://img.shields.io/badge/docker-%230db7ed.svg?style=flat-square&logo=docker&logoColor=white">
    <img src="https://img.shields.io/badge/.NET-5C2D91?style=flat-square&logo=.net&logoColor=white">
</p>

Open World Server (OWS) is a server instance manager designed to create large worlds in UE4. Either by stitching together multiple UE4 maps or by splitting a single large map into multiple zones, OWS will spin up and shut down server instances as needed based on your world population. OWS can load balance your world across multiple hardware devices. OWS can support thousands of players in the same world by instancing up and out. A single zone can be instanced multiple times to support a large population in one area. Areas of a map can also be split up into multiple zones to support a larger population. In addition to server instance management, OWS also handles persistence for Accounts, Characters, Abilities, Inventory and more.

# Projects
- OWS Benchmarks - This project will allow us to configure and run performance testing on the OWS API.  This will be important for comparing the impact of certain changes.
- OWS Character Persistence - The Character Persistence API will be responsible for storing our player characters and all related data.
- OWS Data - This is a shared project that houses our data repository access code.
- OWS External Login Providers - This project contains code for integrating with external login providers such as Xsolla, Google, Facebook, etc.
- OWS Instance Launcher - This project builds our Instance Launcher that replaces the RPG World Server in OWS 1.
- OWS Instance Management - This API manages Zone Instances and the OWS Instance Launchers.
- OWS Public API - This API handles all API calls that come directly from player clients such as registration, login, and connecting to the game.
- OWS Status - This service provides Monitoring & Health Checking (Beta)
- OWS Shared - This project houses various miscellaneous code that multiple other projects require.

# Additional Files Needed (not required for the Docker version of OWS)
- OWS 1 Web API: https://drive.google.com/file/d/1usSUaohEKJv1yPJKV2CVFuhyeocDt6Is/view?usp=sharing  (this will be needed until we replace all of the OWS 1 functionality)
- OWS 1 MSSQL 2017 DB: https://drive.google.com/file/d/1pEXNK1fK5e4fLzSFARzdKvY7ESMkY9Ru/view?usp=sharing (this will be needed until we break out the DB into separate microservice respositories)

# Contributing
* [Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)
* [Naming Guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines)
* [Coding Style](https://github.com/dotnet/corefx/blob/368fdfd86ee3a3bf1bca2a6c339ee590f3d6505d/Documentation/coding-guidelines/coding-style.md)

# Docker Setup Instructions
### Requirements
- Download Or Clone OWS
- [.Net 5.0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)
- [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)

## Windows
1. Download and Install [Docker Desktop For Windows](https://www.docker.com/products/docker-desktop)
2. Open OWS Project in [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/)
3. Select Docker Compose startup project and Launch Docker Compose.

    ![docker-compose-windows](https://i.imgur.com/HbRNXDG.png)

4. **(Optional)** Running Docker Compose without Visual Studio Debugger, Enter the following Command Prompt from the OWS root directory.
    ```
    docker-compose -f docker-compose.yml -f docker-compose.override.windows.yml up -d
    ```
    For a fresh build use the following command
    ```
    docker-compose -f docker-compose.yml -f docker-compose.override.windows.yml up -d --build --no-cache
    ```
5. Open SQL Server Management Studio and connect to localhost with sa / yourStrong(!)Password
6. Run the following SQL statment against the Open World Server database.  Be sure to replace the values CustomerName, FirstName, LastName, Email, and Password.
    ```
    EXEC [dbo].[AddNewCustomer] 'CustomerName', 'FirstName', 'LastName', 'Email', 'Password'
    ```
7. Run the following SQL statment against the Open World Server database to get your API key.  Save this for later.
    ```
    SELECT TOP 1 CustomerGUID FROM Customers
    ```
8. Run the following SQL statment against the Open World Server database to add your local PC as a World Server.  Replace [CustomerGUID] with the API key from the previous step.
    ```
    INSERT INTO WorldServers (CustomerGUID, ServerIP, MaxNumberOfInstances, ActiveStartTime, Port, ServerStatus, InternalServerIP, StartingMapInstancePort)
    VALUES ('[CustomerGUID]', '127.0.0.1', 10, NULL, '8081', 0, '127.0.0.1', '7778')
    ```
9. Follow the instructions here to setup the OWS Starter Project and connect it to your OWS API.  [Instructions for OWS Starter Project](http://rpgwebapi.sabredartstudios.com/Docs/Install)
    
## OSX
1. Download and Install [Docker Desktop For Mac](https://www.docker.com/products/docker-desktop)
2. Open OWS Project in [Visual Studio For Mac](https://visualstudio.microsoft.com/vs/mac/)
3. Run the following command in an terminal to install the Development Certificates
    ```
    dotnet dev-certs https --trust
    ```
3. Select Docker Compose startup project and Launch Docker Compose.

    ![docker-compose-osx](https://i.imgur.com/QOGyGih.png)

4. **(Optional)** Running Docker Compose without Visual Studio Debugger, Run the following  command in an terminal from the OWS root directory.
    ```
    docker-compose -f docker-compose.yml -f docker-compose.override.osx.yml up -d
    ```
    For a fresh build use the following command
    ```
    docker-compose -f docker-compose.yml -f docker-compose.override.osx.yml up -d --build --no-cache
    ```
## Linux
1. Download and Install [Docker Engine For Linux](https://www.docker.com/products/docker-desktop)
2. Close all Web Browsers
3. Download and Run [dotnet-dev-certificate-linux](https://github.com/CodewareGames/dotnet-dev-certificate-linux) to install Development HTTPS Certificate.
4. Run the following  command in an terminal from the OWS src directory.
    ```
    sudo docker-compose -f docker-compose.yml -f docker-compose.override.linux.yml up -d
    ```
    For a fresh build use the following command
    ```
    sudo docker-compose -f docker-compose.yml -f docker-compose.override.linux.yml up -d --build --no-cache
    ```
