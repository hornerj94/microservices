
# microservices
## Worum geht es in diesem Projekt??
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
Der Authentifizierungsservice wird im Projekt authentication-service umgesetzt. Zur Durchf�hrung der Authentizierung nutzt das Projekt Thymeleaf,
JWT und das Spring-Security-Framwork.

Der authentication-service �berpr�ft ob die vom Benutzer eingegebenen Anmeldedaten korrekt sind. Wenn das der Fall ist erstellt der authentication-service einen Token und signiert diesen mit einer HMAC mit SHA256 und einem entsprechenden Schl�ssel. Der Token enth�lt die Anmeldeinformation des Benutzers und seine Rechte. Der Token wird dem Benutzer �ber den API-Gateway zugesendet.

## dummy-service
Der Service welcher die zwei HTML-Dokumente verwaltet wird im Projekt dummy-service umgesetzt. Der dummy-service ist eine einfache Anwendung welche die erw�hnten HTML-Dokumente verwaltet, die securedPage.html und die index.html. Beide Dokumente enthalten einen Link zur jeweils anderen Seite.

Au�erdem enth�lt das Projekt eine Controller Klasse welche Anfragen abf�ngt und einem HTML-Dokument zuordnet. Diese Zuordnung wird ebenfalls automatisiert von Spring Boot durchgef�hrt.

## F�hren Sie die Anwendung aus
**Vorbedingungen**
Installiere das Build Tool Apache Maven auf deinem Rechner. 

Um die Anwendung laufen zu lassen m�ssen die einzelnen Projekte in einer bestimmten Reihenfolge gestartet werden. Die optimale Reihenfolge ist folgende:

 1. eureka-server
 2. dummy-service
 3. api-gateway
 4. authorization-service

Jedes der Projekte nutzt einen Maven Wrapper. Um die Anwendung zu starten muss dazu in den jeweiligen Projektordner navigiert werden und hier kann die Anwendung unter Unix-Systemen mit folgender Anweisung gestartet werden...

    mvnw spring-boot:run

und unter Windows mit...

    mvnw.cmd spring-boot:run

Nach dem Start der letzten Anwendung kann es vorkommen das manche der Anwendungen noch nicht beim Eureka-Server registriert sind bzw. der API-Gateway eine veraltete Version der Liste der beim Eureka-Server registrierten Server vorliegen hat. Da der API-Gateway in diesem Fall den jeweiligen Service nicht findet wird er eine Weiterleitung verweigern und eine Fehlermeldung ausl�sen. Ist das der Fall empfiehlt es sich einige Sekunden zu warten bis der API-Gateway sich eine neue Version der Liste der Services besorgt hat.

## Submodule aktualisieren
Um die submodule zu aktualisieren kann im microservice Projekt folgender Befehl eingegeben werden...
```
git submodule foreach git pull origin master
```
## Hinzuf�gen eines neuen Service
Um einen neuen Service zum Projekt hinzuzuf�gen und den Zugriff zu diesen auf bestimmte Benutzer zu beschr�nken m�ssen mehrere Schritte getan werden.

Als erstes muss definiert werden zu welchem Service bei welchem Anfrage-Pfad weitergeleitet wird. Dazu muss die unten abgebildete application.yml angepasst werden. 

    spring:
      application:
        name: api-gateway # Identifier of this application
    
    server:
      port: 2222
    
    eureka:
      client:
        registryFetchIntervalSeconds: 5
        serviceUrl:
          defaultZone: http://localhost:8761/eureka/ # URL of the eureka server for registration
                
    zuul:
      ignoredPatterns: /login/** # The login path is served by the api-gateway directly
      ignored-services: "*"
      routes: # Forwardings of requests
        dummy-service:
          path: /**
          service-id: dummy-service
        auth-service:
          path: /auth/**
          service-id: auth-service
          strip-prefix: false # Forward the request all together with the auth path
          sensitive-headers: Cookie, Set-Cookie # Avoid to send internal cookies to external entities

Wenn zum Beispiel der Service "test-service" beim angefragten Pfad "/test/" aufgerufen werden soll m�ssen folgende Zeilen zur Datei hinzugef�gt werden...
 
    test-service:
      path: /test/**
      service-id: test-service

Dann muss f�r den Pfad f�r welchen der Zugang beschr�nkt werden soll, in der unten abgebildeten Klasse SecurityConfiguration eine neue Beschr�nkung hinzugef�gt werden . 

    /*
     * Copyright 2019 (C) by Julian Horner.
     * All Rights Reserved.
     */
    
    package de.rtuni.ms.apig.config;
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Bean;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.config.http.SessionCreationPolicy;
    import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
    
    import de.rtuni.ms.apig.filter.JWTAuthenticationFilter;
    
    /**
     * Class that handles several security configurations.
     * 
     * @author Julian
     */
    @EnableWebSecurity
    public class SecurityConfiguration extends WebSecurityConfigurerAdapter {
        //---------------------------------------------------------------------------------------------
    
        /** The <code>JwtConfiguration</code>. */
        @Autowired
        private JWTConfiguration jwtConfiguration;
    
        //---------------------------------------------------------------------------------------------
    
        /**
         * Get a new <code>JwtConfiguration</code>.
         * 
         * @return The stated JWT configuration
         */
        @Bean
        public JWTConfiguration jwtConfig() { return new JWTConfiguration(); }
    
        //---------------------------------------------------------------------------------------------
    
        /**
         * Configure custom security configurations.
         */
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.csrf().disable()
            // Use stateless sessions.
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS).and()
            
            // Add filter to validate tokens with every request.
            .addFilterAfter(new JWTAuthenticationFilter(jwtConfiguration),
                    UsernamePasswordAuthenticationFilter.class)
     
            .authorizeRequests()
            // Permit only users with ADMIN role.
            .antMatchers("/securedPage/**").hasRole("ADMIN")
            // Permit auth and login path for sending credentials. 
            .antMatchers("/auth/**", "/login").permitAll().and()
            // Configures where to forward if authentication is required.
            .formLogin().loginPage("/login");
        }
    
        //---------------------------------------------------------------------------------------------
    }

Wenn zum Beispiel auf den Pfad "testPfad" nur Nutzer mit der Rolle "TESTER" zugreifen d�rfen dann muss der folgende Code in der configure(HttpSecurity http)-Methode eingef�gt werden.
```
        .antMatchers("/testPfad/**").hasRole("TESTER")
```


Die ** Zeichen bedeuten dass die Beschr�nkung f�r den angegebene Pfad und alle darunter liegenden Ressourcen gilt.


