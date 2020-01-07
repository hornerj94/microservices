
# microservices
## What is this project about?
Das Projekt setzt eine einfache Microservices-Architektur um. Enthalten sind ein API-Gateway, ein Service für die Authentifizierung, ein Service-Discovery-Server und ein Service welcher zwei einfache HTML-Dokumente verwaltet.

## Details
Das Projekt enthält mehrere git submodule von welchen jedes submodul ein eigenständiges git Projekt darstellt. Die Submodule sind:

 - api-gateway
 - authentication-service
 - dummy-service
 - eureka-server

## Spring-Boot und Spring-Cloud
Jeder der Services nutzt das Spring-Boot und das Spring-Cloud-Framework. 

Das Spring-Boot-Framework automatisiert verschiedene Abläufe wie das einrichten eines Webservers oder das deployen einer Anwendung und erleichtert somit sowohl die Entwicklung als auch das Starten einer Anwendung.

Das Projekt nutzt einen Serverseitigen-Discovery-Mechanismus, welcher über  Eureka umgesetzt wird. Eureka ist der Service-Discovery Server und Client, von Netflix, welcher über das Spring Cloud Framework bereitsgestellt wird. 

## eureka-server
Der Eureka Server wird im Projekt eureka-server umgesetzt. Die Services registrieren sich bei diesem als Eureka-Clients und schicken diesem in regelmäßigen Intervallen Aktualisierungen ihres Standorts. Wenn ein Service sich eine bestimmte Zeit lang nicht mehr beim Server meldet wird dieser abgemeldet und aus der Liste der verfügbaren Services entfernt. Da die Services sich selbständig beim Server registrieren können diese auf einfache Art und Weiße gefunden werden.

## api-gateway
Der API-Gateway wird im Projekt api-gateway umgesetzt. Der Client kommuziert ausschließlich mit dem API-Gateway. Je nachdem welchen Pfad der Client anfragt, überprüft der API-Gateway ob die erforderlichen Berechtigungen vorliegen, routet die Anfrage an einen der Services weiter und das resultierende Ergebnis wieder zurück zum Client. Wenn Services untereinander kommunizieren erfolgt diese ebenfalls über den API-Gateway. Der API-Gateway erfragt dafür den Standort der Services beim Eureka-Server.

Das routing wird mit zuul umgesetzt. zuul ist der Gateway Service, von Netflix, welcher neben routing weitere Funktionalitäten anbietet, zum Beispiel  Ausfallsicherheit, Lastverteilung oder Sicherheitsaspekte. zuul ist ebenfalls im Spring-Cloud-Framework enthalten.

Des Weiteren nutzt das Projekt Thymeleaf, JWT und das Spring-Security-Framwork zum überprüfen der Benutzer Berechtigungen und entsprechender Weiterleitung dieser.

**Authorisierung**
Wenn ein Benutzer auf eine Ressource zugreifen will, auf die nur Benutzer mit einer besonderen Rolle zugreifen dürfen, erkennt der API-Gateway dies und sendet dem Benutzer eine Aufforderung sich zu authentisieren. Dies geschieht über eine Login Seite auf welcher der Benutzer seinen Benutzernamen und sein Passwort eingibt. Die Anmeldedaten werden an den API-Gateway gesendet welcher diese wiederum an den Authentifizierungsservice weiterleitet. 

Der Authentifizierungsservice überprüft ob die vom Benutzer eingegebenen Anmeldedaten korrekt sind. Wenn das der Fall ist erstellt der Service einen Token, welcher den Benutzer repräsentiert. Der Token wird dem Benutzer über den API-Gateway zugesendet.

Die letztendliche Autorisierung erfolgt über den Token welchen der Benutzer zusammen mit der jeweilig angeforderten Ressource zum API-Gateway schickt. Der API-Gateway entschlüsselt den Token mit dem dazugehörigen Schlüssel und validiert somit das der Token vom Authentifizierungsservice ausgestellt wurde. Als letztes wird überprüft ob der Benutzer die erforderlichen Rechte besitzt um auf die Ressource zuzugreifen und diese wenn die Rechte vorliegen zurückgegeben.

## authentication-service
Der Authentifizierungsservice wird im Projekt authentication-service umgesetzt. Der authentication-service überprüft ob die vom Benutzer eingegebenen Anmeldedaten korrekt sind. Wenn das der Fall ist erstellt der authentication-service einen Token und signiert diesen mit einer HMAC mit SHA256 und einem geheimen Schlüssel. Der Token enthält die Anmeldeinformation des Benutzers und seine Rechte. Der Token wird dem Benutzer über den API-Gateway zugesendet.

## dummy-service
Der Service welcher die zwei HTML-Dokumente verwaltet wird im Projekt dummy-service umgesetzt. Der dummy-service ist eine einfache Anwendung welche die erwähnten HTML-Dokumente verwaltet, die securedPage.html und die index.html. Beide Dokumente enthalten einen Link zur jeweils anderen Seite.

Außerdem enthält das Projekt eine Controller Klasse welche Anfragen abfängt und einem HTML-Dokument zuordnet. Diese Zuordnung wird ebenfalls automatisiert von Spring Boot durchgeführt.

## Run the application
Um die Anwendung laufen zu lassen müssen die einzelnen Projekte in einer bestimmten Reihenfolge gestartet werden.
1. Starten des Eureka-Servers

## Update submodules

## Add a new service to the API-Gateway

## Authorize a service
