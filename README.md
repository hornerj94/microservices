
# microservices
## What is this project about?
Das Projekt setzt eine einfache Microservices-Architektur um. Enthalten sind ein API-Gateway, ein Service f�r die Authentifizierung, ein Service-Discovery-Server und ein Service welcher zwei einfache HTML-Dokumente verwaltet.

## Details
Das Projekt enth�lt mehrere git submodule von welchen jedes submodul ein eigenst�ndiges git Projekt darstellt. Die Submodule sind:

 - api-gateway
 - authentication-service
 - dummy-service
 - eureka-server

## Spring-Boot und Spring-Cloud
Jeder der Services nutzt das Spring-Boot und das Spring-Cloud-Framework. 

Das Spring-Boot-Framework automatisiert verschiedene Abl�ufe wie das einrichten eines Webservers oder das deployen einer Anwendung und erleichtert somit sowohl die Entwicklung als auch das Starten einer Anwendung.

Das Projekt nutzt einen Serverseitigen-Discovery-Mechanismus, welcher �ber  Eureka umgesetzt wird. Eureka ist der Service-Discovery Server und Client, von Netflix, welcher �ber das Spring Cloud Framework bereitsgestellt wird. 

## eureka-server
Der Eureka Server wird im Projekt eureka-server umgesetzt. Die Services registrieren sich bei diesem als Eureka-Clients und schicken diesem in regelm��igen Intervallen Aktualisierungen ihres Standorts. Wenn ein Service sich eine bestimmte Zeit lang nicht mehr beim Server meldet wird dieser abgemeldet und aus der Liste der verf�gbaren Services entfernt. Da die Services sich selbst�ndig beim Server registrieren k�nnen diese auf einfache Art und Wei�e gefunden werden.

## api-gateway
Der API-Gateway wird im Projekt api-gateway umgesetzt. Der Client kommuziert ausschlie�lich mit dem API-Gateway. Je nachdem welchen Pfad der Client anfragt, �berpr�ft der API-Gateway ob die erforderlichen Berechtigungen vorliegen, routet die Anfrage an einen der Services weiter und das resultierende Ergebnis wieder zur�ck zum Client. Wenn Services untereinander kommunizieren erfolgt diese ebenfalls �ber den API-Gateway. Der API-Gateway erfragt daf�r den Standort der Services beim Eureka-Server.

Das routing wird mit zuul umgesetzt. zuul ist der Gateway Service, von Netflix, welcher neben routing weitere Funktionalit�ten anbietet, zum Beispiel  Ausfallsicherheit, Lastverteilung oder Sicherheitsaspekte. zuul ist ebenfalls im Spring-Cloud-Framework enthalten.

Des Weiteren nutzt das Projekt Thymeleaf, JWT und das Spring-Security-Framwork zum �berpr�fen der Benutzer Berechtigungen und entsprechender Weiterleitung dieser.

**Authorisierung**
Wenn ein Benutzer auf eine Ressource zugreifen will, auf die nur Benutzer mit einer besonderen Rolle zugreifen d�rfen, erkennt der API-Gateway dies und sendet dem Benutzer eine Aufforderung sich zu authentisieren. Dies geschieht �ber eine Login Seite auf welcher der Benutzer seinen Benutzernamen und sein Passwort eingibt. Die Anmeldedaten werden an den API-Gateway gesendet welcher diese wiederum an den Authentifizierungsservice weiterleitet. 

Der Authentifizierungsservice �berpr�ft ob die vom Benutzer eingegebenen Anmeldedaten korrekt sind. Wenn das der Fall ist erstellt der Service einen Token, welcher den Benutzer repr�sentiert. Der Token wird dem Benutzer �ber den API-Gateway zugesendet.

Die letztendliche Autorisierung erfolgt �ber den Token welchen der Benutzer zusammen mit der jeweilig angeforderten Ressource zum API-Gateway schickt. Der API-Gateway entschl�sselt den Token mit dem dazugeh�rigen Schl�ssel und validiert somit das der Token vom Authentifizierungsservice ausgestellt wurde. Als letztes wird �berpr�ft ob der Benutzer die erforderlichen Rechte besitzt um auf die Ressource zuzugreifen und diese wenn die Rechte vorliegen zur�ckgegeben.

## authentication-service
Der Authentifizierungsservice wird im Projekt authentication-service umgesetzt. Der authentication-service �berpr�ft ob die vom Benutzer eingegebenen Anmeldedaten korrekt sind. Wenn das der Fall ist erstellt der authentication-service einen Token und signiert diesen mit einer HMAC mit SHA256 und einem geheimen Schl�ssel. Der Token enth�lt die Anmeldeinformation des Benutzers und seine Rechte. Der Token wird dem Benutzer �ber den API-Gateway zugesendet.

## dummy-service
Der Service welcher die zwei HTML-Dokumente verwaltet wird im Projekt dummy-service umgesetzt. Der dummy-service ist eine einfache Anwendung welche die erw�hnten HTML-Dokumente verwaltet, die securedPage.html und die index.html. Beide Dokumente enthalten einen Link zur jeweils anderen Seite.

Au�erdem enth�lt das Projekt eine Controller Klasse welche Anfragen abf�ngt und einem HTML-Dokument zuordnet. Diese Zuordnung wird ebenfalls automatisiert von Spring Boot durchgef�hrt.

## Run the application
Um die Anwendung laufen zu lassen m�ssen die einzelnen Projekte in einer bestimmten Reihenfolge gestartet werden.
1. Starten des Eureka-Servers

## Update submodules

## Add a new service to the API-Gateway

## Authorize a service
