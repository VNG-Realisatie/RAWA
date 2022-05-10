---
layout: page-with-side-nav
date: 23-03-2022
title: "Referentiearchitectuur en -componenten"
---

# Referentiearchitectuur werken met API's
## Introductie

Onderstaand volgt een toelichting van de referentiearchitectuur werken met API's, zoals hieronder weergegeven. De toelichting is bedoeld om een begrip van de doelstelling te verkrijgen, en als ondersteuning bij realisatie hiervan in een zo breed mogelijke context. 

Er wordt van enige technische achtergrond uit gegaan in de uitleg, en begrip van enkele concepten zoals: (Docker, OIDC/OAuth). De architectuur is dusdanig opgezet dat er zo veel mogelijk vrije componentkeuze plaats kan vinden. Als gevolg hiervan kunnen niet alle verschillende implementatiescenarios in detail beschreven worden, gezien deze afhankelijk van de gemaakte keuzes kunnen verschillen. Om deze reden word er met enige regelmaat verwezen naar de documentatie van componenten, waar uitgebreide omschrijvingen terug te vinden zijn.   

Om toch een goed beeld te kunnen schetsen voor een breder publiek met minder technische kennis, is er een handleiding geschreven om een lokale demo-opstelling te realiseren. Deze is ingericht met de gedachte het moeten configureren minimaal te behouden, en toch een goed gevoel te kunnen krijgen bij het concept.

## Inhoudsopgave
De beschrijving van de referentiearchitectuur heeft de volgende structuur:

- [Architectuurplaat](#ImplementatiescenarioRAWA)
- [Omschrijving flows architectuurplaat](#Omschrijvingflows)
- [Overzicht referentiecomponenten](#Overzichtreferentiecomponenten)
- [Aanvullende toelichting](#Aanvullendetoelichting)
- [Demo](#Demo)

Feedback? Hoe u kunt [bijdragen](https://github.com/VNG-Realisatie/API-Kennisbank/blob/master/CONTRIBUTING.md) vind u hier.

## Implementatiescenario RAWA

![Referentiearchitectuur en -componenten](./assets/referentiearchitectuur.png)

## Toelichting architectuurplaat
Realisatie van de bovenstaande omschreven stack heeft als hoofdzakelijke doelstelling het beveiligen van API's. De manier van beveiligen zoals hierboven omschreven, is tweeledig. Allereerst vind er authorizatie plaats, als primaire beveiligingslaag of een gebruiker of client uberaupt de API mag aanroepen. Daarna vind er authorizatie plaats, waar bepaald wordt of het verzoek voldoet aan vooraf gedefinieerd beleid. Op deze manier is er een uiterst fijnmazig toegangsbeleid te handhaven. Het exacte verloop van dit proces kunt u hieronder vinden.

### Omschrijving flows

1.	Gebruiker doet een verzoek naar een API. Dit verzoek wordt ontvangen door de API gateway. 

2.	Verzoek komt aan bij API Gateway. Indien de client onbekend is zal deze worden doorverwezen naar de Identity Provider (IDP) voor authenticatie.
	- Oauth 2.0	: De IDP controleert een vooraf opgehaald access token, en verleent op basis hiervan wel/geen toegang.
	- OIDC			: De IDP vraagt om inloggegevens. Afhankelijk van IDP configuratie kan hier ook Multi-Factor-Authentication (MFA) worden afgedwongen of federatief toegang worden verleend.
	
	(Het gebruik van OAuth 2.0 of OIDC als authenticatieprotocol wordt in het kader van de architectuurprincipes aanbevolen.)

3.	Aan de hand van de uitkomst van stap 2 zal de API Gateway handhaven, of het verzoek wel/niet door mag. De API gateway fungeert hier als Policy Enforcement Point (PEP).

4.	Indien “Ja” bij stap 3 zal Authorizatie plaatsvinden. Het API verzoek, aangevuld met metadata vanuit de authenticatie, word in JSON format naar de policy engine gestuurd. Hier wordt aan de hand van een vooraf gedefinieerde policy, getoetst of het verzoek doorgang krijgt, en een uitslag teruggekoppeld naar de API gateway. (Voor detail over exacte werking, zie: [Aanvullende toelichting](#Hoewerkenpolicyagents)  

5.  Aan de hand van de uitkomst van stap 4 zal de API Gateway handhaven, of het verzoek wel/niet door mag. De API gateway fungeert hier als Policy Enforcement Point (PEP).

6.  Het verzoek wordt doorgestuurd naar de doel-API.!


7.  De respons wordt teruggekoppeld aan de API gateway.
8.  De respons wordt teruggekoppeld aan de Client.

## Overzicht referentiecomponenten

|IDP - Identity provider|PE - Policy Engine     |API gateway            |
|:---------------------:|:---------------------:|:---------------------:|
|Keycloak								|OPA - Open Policy Agent|Kong										|
|Authentik							|Axiomatics							|Nginx									|
|Azure AD								|PlainID								|Mulesoft								|
|Okta										|												|Axway									|
|Auth0									|												|Ambassador							|

## Aanvullende toelichting

### OIDC/OAuth 2.0 koppeling
Het gebruik van het het OAuth 2.0 authenticatieprotocol wordt in deze architectuur aangeraden. De configuratie hiervan is noodzakelijk om authenticatie plaats te laten vinden tussen de API gateway en de IDP. Alhoewel er kleine verschillen kunnen zitten tussen verschillende applicaties en use-cases, blijft de configuratie in essentie grotendeels gelijk. 

De configuratie begint met het aanmaken van een client in de IdP waar de te gebruikers in opgenomen zijn. Dit is een punt waar de API gateway naar toe zal praten om informatie uit te kunnen wisselen en controleren. Voorafgaand aan het aanmaken van een client, is het belangrijk om te bepalen of er OIDC (gebruikers toegang) of OAuth 2.0 (systeem toegang) gebruikt gaat worden. Dit kan afhankelijk van de gekozen IDP invloed hebben op de configuratiestappen.
Voor de exacte procedure van het aanmaken van een client is het verstandig de documentatie van de gekozen IDP zelf door te nemen. Hier staat meestal stap voor stap, uitgelegd hoe dit gerealiseerd kan worden. 

Wat in essentie de bedoeling is, is dat er een client aangemaakt wordt, en hier enkele parameters worden opgehaald die naar de client verwijzen.

Na het ophalen van deze gegevens kan de API gateway geconfigureerd worden. Hier zal in de meeste gevallen een OIDC plugin de koppeling maken. Ook per API gateway kunnen er verschillen zijn hoe dit excact geconfigureerd dient te worden. Echter blijft de essentie altijd om de bovenstaande waardes van de OIDC client, in te voeren als omgevingsvariabelen in een configuratie.

Ook hier is het advies om voor het gekozen platform naar de OIDC plugin documentatie te kijken. Hier wordt benoemd hoe, indien noodzakelijk, de plugin aangezet kan worden, en waar de bovenstaande parameters (provider_url, client_id, client_secret, etc.) ingevoerd dienen te worden. 

Hierna zou de OIDC koppeling gereed moeten zijn, en kan de OIDC aangezet worden op verschillende “routes” of “services”,  of globaal toegepast worden. Dit houdt in dat er voor bepaalde “wegen” naar een dienst toe OIDC afgedwongen wordt in het geval van “routes”. Of dat er voor bepaalde diensten OIDC afgedwongen wordt in het geval van services. Tot slot is het mogelijk ten alle tijden OIDC authenticatie af te dwingen in het geval van globale deployment.

### Hoe werken policy agents
Een policy agent is een fundamenteel onderdeel van Policy Based Access Control (PBAC). Echter zijn er meer elementen nodig om het concept werkend te krijgen, namelijk een PEP, PDP, en PAP (met optioneel een PIP). In de "ARCHITECTUURPLAAT" zijn deze aangegeven met blauwe vakjes. Effectieve inzet van deze elementen samen, zijn ook in lijn met de principes van Zero-Trust Architecture. Een toegangsfilosofie met als hoofdzakelijke uitgangspunten real-time authorizatie en granulariteit. Beide concepten, worden samen met de componenten hieronder toegelicht. (Voor meer informatie over Zero-Trust, zie: [NIST 800-207](https://csrc.nist.gov/publications/detail/sp/800-207/final)

PDP - Policy Descision Point

De hoofdzakelijke taak van een policy engine, is het maken van beslissingen. Afhankelijk van de configuratie binnen de API gateway, wordt er op verschillende routes, services, of op globaal niveau beleid afgedwongen. De PDP is de plek waar de API call met metadata binnenkomt, waarna deze getoetst wordt. Nadat de toetsing heeft plaatsgevonden, stuurt de PDP zijn beslissing terug naar de API gateway. Het toetsen gebeurt aan de hand van vooraf gedefinieerd beleid, wat terug te vinden is in de PAP. 

PAP - Policy Administration Point

Naast dat een policy engine de rol van PDP vervult, beheert deze ook vaak de vooraf gedefinieerde policies zelf. Deze zijn samengesteld in programmeertaal, of via een no-code gebruikersinterface. De inhoud van de policies is vaak terug te herleiden aan businesslogica en compliance maatregelen. Zo is het scheiding-van-taken principe te definieren in het beleid, en zo toe te passen op een bepaald type API calls, of aan de hand van metadata.

PEP - Policy Enforcement Point

Nadat de PDP een evaluatie heeft gedaan, krijgt de API gateway hier de uitslag van. Gezien de API gateway, afhankelijk van de uitslag, bepaalt of het verzoek door mag, vervult hij hiermee direct de functie van PEP.

Een voorbeeld om de verschillende onderdelen goed te kunnen plaatsen, is de verdeling van machten in een staatsbestel. De PDP is de rechtelijke, de PAP de wetgevende , en de PEP de uitvoerende macht.

PIP - Policy information Point

De bovenstaande elementen samengebracht, kunnen op deze manier elke transactie beoordelen of deze voldoet aan vooraf bepaald beleid. Hiermee wordt voldaan aan een fundamenteel Zero-Trust principe (real-time authorization). Daarnaast is het ook mogelijk, om meerdere policies te combineren, of op verschillende plekken in de keten afgedwongen laten worden. Met enkel authenticatie en authorizatie, is ook het principe van granulariteit gedemonstreerd. Het kunnen realiseren van gelaagde authorizatiestructuren bied enorm veel mogelijkheden in het verlenen van fijnmazige toegang, wat gewenst is vanuit een veiligheidsperspectief. 

Tot slot is het mogelijk dat op basis van de data van het API verzoek, en bijbehorende metadata, onvoldoende informatie beschikbaar is om tot een beslissing te komen door de PDP. Het kan voorkomen dat externe databronnen geraadpleegd moeten worden, en deze vervullen in dit geval de rol van PIP. Zo kunnen er gedurende de overwegingen van een verzoek, API calls gemaakt worden door de policy engine, om aanvullende data op te halen uit bijvoorbeeld een HR systeem.

### Koppeling policy agents
In de meeste gevallen zijn policy agents aan te sluiten op de API gateway doormiddel van een plugin. In de componentkeuze is dit een onderdeel dat goed overwogen moet worden, gezien niet elke policy engine met even veel gemak aangesloten kan worden op elke API gateway. 

(FORWARD AUTH benoemen en nader uitzoeken)

# Demo

In de volgende sprint zal de demo toegevoegd worden.

