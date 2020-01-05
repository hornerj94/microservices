# microservices
## What is this project about?
Das Projekt setzt eine einfache Microservices-Architektur um. Enthalten sind ein API-Gateway, ein Service für die Authentifizierung, ein Service-Discovery-Server und ein Service welcher zwei einfache HTML-Dokumente verwaltet.

## Details
Das Projekt enthält mehrere git submodule von welchen jedes submodul ein eigenständiges git Projekt darstellt. Die Submodule sind:
* api-gateway
* authentication-service
* dummy-service
* eureka-server

### Spring-Boot und Spring-Cloud
Jedes dieser Projekte bzw. jede der Anwendungen nutzt das Spring-Boot und das Spring-Cloud-Framework. Das Spring-Boot-Framework automatisiert verschiedene Abläufe wie das einrichten eines Webservers oder das deployen einer Anwendung auf diesem und erleichtert somit die Entwicklung und das Starten einer Anwendung. Das Projekt nutzt einen Serverseitigen-Discovery-Mechanismus, dies erfolgt über Eureka einen Service-Discovery-Server welcher vom Spring Cloud Framework bereitsgestellt wird.
 
### eureka-server
Der Eureka Server wird im Projekt eureka-server umgesetzt. Die Services registrieren sich bei diesem als Eureka-Clients und schicken diesem in regelmäßigen Intervallen Aktualisierungen ihres Standorts. Wenn ein Service sich eine bestimmte Zeit lang nicht mehr beim Server meldet wird dieser abgemeldet und aus der Liste der verfügbaren Servies entfernt. Da die Services sich selbständig registrieren können die Services auf eine einfache Art und Weiße gefunden werden.
authentication-service

### api-gateway
Ein Client kommuziert ausschließlich mit dem API-Gateway, je nachdem welchen Pfad der Client anfragt überprüft der API-Gateway ob die erforderlichen Berechtigungen vorliegen und routet die Anfrage an einen der Services weiter und das resultierende Ergebnis wieder zurück zum Client. Wenn Services untereinander kommunizieren erfolgt diese ebenfalls über den API-Gateway.

### Ergänzen mit Autorisierungs Details

### authentication-service


### dummy-service
Der dummy-service ist eine einfache Anwendung welche zwei HTML-Dokumente verwaltet, die securedPage.html und die index.html. Beide Dokumente enthalten einen Link zur jeweils anderen Seite.

Außerdem enthält das Projekt eine Controller Klasse welche Anfragen abfängt und einem HTML-Dokument zuordnet. Diese Zuordnung wird ebenfalls automatisiert von Spring Boot durchgeführt. 

Damit der dummy-service von anderen Services gefunden wird registriert sich dieser in einer Service-Registry dem eureka-server. Dies erfolgt über Eureka einen Service Discovery Server welcher vom Spring Cloud Framework bereitsgestellt wird.

## Run the application

## Add a new service to the API-Gateway

## Authorize a service
