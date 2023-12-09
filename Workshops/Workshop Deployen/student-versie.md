# Workshop Deployen
## Voorwaarden deelname
Om deel te nemen, dien je de Skylab-workshop te hebben gevolgd en moet je webapplicatie een repository op Git hebben.
## Beschrijving
Tijdens deze workshop ga je je webapplicatie live zetten, zodat anderen er online bij kunnen komen! Dit bereik je door middel van zeven opdrachten. 🚀🛠️
## Eindresultaat
Na het doorlopen van deze workshop, heb je:
-	Een reverse proxy opgezet 🔄
-	Een SQL server opgezet 🗄️
-	Gewerkt met Docker 🐳
-	Een TSL-certificaat geïmplementeerd (inclusief een grondige scan) 🔒
-	Jouw webapplicatie met succes gedeployed 🌐

## Opdracht 1 ASP dotnet core
In deze taak installeer en configureer je de Dotnet Core applicatieserver, inclusief de SDK, op de op de acceptatieomgeving.

 **Stap 1**:  Open je opdrachtenprompt en login via SSH.   
 
 `ssh s-studentnummer@145.xxx.xxx`



**Stap 2** : Voeg de Microsoft repository to aan de APT (Advanced Package Tool) door de volgende commando’s uit: 

`sudo wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb`

`sudo dpkg -i packages-microsoft-prod.deb`

`sudo rm packages-microsoft-prod.deb`


**Stap 3** : Installeer de SDK: ` sudo apt-get update && \ sudo apt-get install -y dotnet-sdk-8.0`


**Stap 4**:  Controleer de versie `dotnet –version`


**Stap 5**: Test Dotnet doormiddel van een console applicatie. Als je de volgende commando’s uit voert zul je “Hello World!” zien.

`sudo dotnet new console --output myConsoleApp`

`sudo dotnet run --project myConsoleApp`


**Stap 6**: Installeer entity framework tool door de volgende commando uit te voeren: 

`sudo dotnet tool install --tool-path /usr/bin dotnet-ef`


**Stap 7**: Als gelukt is zul je de versie van entity framework zien als je de volgende commando uitvoert: 

` $ sudo dotnet tool list  --tool-path /usr/bin`


**Stap 8:** Installeer .Nettools: 

`dotnet tool install -g dotnetsay`

✅Opdracht 1 is Klaar! 

## Opdracht 2  Docker installeren
Docker is een platform waarmee je applicaties kunt verpakken en draaien in geïsoleerde, draagbare containers. Het maakt het gemakkelijk om applicaties consistent en efficiënt over verschillende omgevingen te verplaatsen. In deze opdracht ga je docker installeren, in een latere opdrachten ga je nog meer hiermee doen.

**Stap 1**:  Open je opdrachtenprompt en login via SSH.   `ssh s-studentnummer@145.xxx.xxx`
**Stap 2**:  Run de volgende commando’s om verouderde packages te verwijderen 

` for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done`

ℹ️ Je gaat nu de Docker APT-repository instellen. Dit houdt in dat je een pakketbeheerder op je Debian-systeem configureert, waardoor het systeem Docker-softwarepakketten kan herkennen en gebruiken

**Stap 3:** Voer de volgende commando’s een voor een uit : 

` sudo apt-get update`

` sudo apt-get install ca-certificates curl gnupg`

` sudo install -m 0755 -d /etc/apt/keyrings `

`  curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg `

` sudo chmod a+r /etc/apt/keyrings/docker.gpg `

` echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null `
` sudo apt-get update`

**Stap 4**: Controleer of alles correct is verlopen. Als dat het geval is, zal de uitvoer niet leeg zijn. Gebruik hiervoor het commando, ` cat /etc/apt/sources.list.d/docker.list`

**Stap 5**:  Installeer docker `sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

**Stap 6** : Controleer of de Docker-installatie succesvol is verlopen. Als alles correct is, zal het commando `sudo docker run hello-world` "Hello World" weergeven en het programma afsluiten.

✅Opdracht 2 is Klaar! 

# Opdracht 3 SQL server
In deze opdracht ga je een SQL Server instellen op een Docker-image. Daarna maak je een testdatabase aan.

## Deel 1 SQL server installeren

**Stap 1**: Haal het SQL Server 2022 (15.x) Linux-containerimage op vanuit de Microsoft Container Register. ` sudo docker pull mcr.microsoft.com/mssql/server:2022-latest`

**Stap 2**: Run de container image met Docker met behulp van de volgende commando, waarbij je <YourStrong@Passw0rd> vervangt door een wachtwoord dat minimaal 10 tekens lang is en zowel hoofdletters als kleine letters bevat.  Dit is het wachtwoord voor de systeembeheerder in SQL server.
` sudo docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong@Passw0rd>" \
   -p 1433:1433 --name sql1 --hostname sql1 \
   -d \
   mcr.microsoft.com/mssql/server:2022-latest `

**Stap 3**:  Controleer of alles goed is gegaan door te kijken of de container met de naam 'sql1' actief is met het commando 

`sudo docker ps -a`

ℹ️ Handige commando's zijn onder andere het starten van een Docker-image met **sudo docker start {naam}**, het stoppen ervan met **sudo docker stop {naam}**, en het verwijderen met **sudo docker rm {naam}**.

##Deel 2 SQL server Testen 

**Stap 4**: Voer de commando `sudo docker exec -it sql1 "bash"` uit.

**Stap 5**: Je bent nu binnen de container en je moet lokaal verbinding maken met sqlcmd door het volledige pad 

` /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"` te gebruiken, met het wachtwoord dat je zojuist hebt aangemaakt.

**Stap 6**: Creëer een testdatabase met het commando 'CREATE DATABASE TestDB;' gevolgd door 'GO'.

**Stap 7**: Verifieer of je database is aangemaakt met het commando 'SELECT Name from sys.databases;' gevolgd door 'GO'.


✅Opdracht 3 is Klaar! 

