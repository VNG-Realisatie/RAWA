---
layout: page-with-side-nav
date: 23-03-2022
title: "Referentiearchitectuur en -componenten"
---

# Referentiearchitectuur werken met API's (RAWA)
## Introductie

Onderstaand volgt een toelichting op de referentiearchitectuur werken met API's.

Er wordt van enige technische achtergrond uitgegaan in de uitleg. De architectuur is dusdanig opgezet dat er zo veel mogelijk vrije componentkeuze plaats kan vinden. Als gevolg hiervan kunnen niet alle verschillende implementatiescenario’s in detail beschreven worden omdat ze afhankelijk van de gemaakte keuzes verschillen. Om deze reden wordt verwezen naar de documentatie van componenten, waar uitgebreide omschrijvingen terug te vinden zijn.


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
Realisatie van de bovenstaande omschreven stack heeft als hoofdzakelijke doelstelling het het op een gecontroleerde manier bieden van toegang tot API's. De manier van verlenen van toegang, zoals hierboven omschreven, is tweeledig. Allereerst vindt er als primaire beveiliging, een autorisatiecheck plaats ten aanzien van de vraag of een gebruiker of client überhaupt de API mag aanroepen. Daarna vindt er autorisatie plaats, waar bepaald wordt of het verzoek voldoet aan vooraf gedefinieerd beleid. Op deze manier is er een uiterst fijnmazig toegangsbeleid te realiseren. Het exacte verloop van dit proces kunt u hieronder vinden.

### Omschrijving flows

1.	Gebruiker/clientapplicatie doet een verzoek naar een API. Dit verzoek wordt ontvangen door de API-gateway. Bruikbare authenticatieprotocollen zijn bijv.: 

2.	Afhankelijk van het benodigde type autorisatie verwijst de API-Gateway voor authenticatie door naar de Identity Provider en ontvangt authenticatiegegevens over de gebruiker of de clientapplicatie. Bruikbare authenticatieprotocollen zijn:
	- Oauth 2.0 (gebruiker of client): De IDP levert authenticatiegegevens ('scopes') op basis waarvan de API Gateway wel/geen toegang verleent. In een aantal situaties is gebruik van het [NL GOV Assurance profile for OAuth 2.0](https://forumstandaardisatie.nl/open-standaarden/nl-gov-assurance-profile-oauth-20) verplicht.
	- OIDC (gebruiker): De IDP levert naast authenticatiegegevens ook gegevens over de gebruiker ('claims'). Afhankelijk van IDP-configuratie (op basis van access policies) kan hier ook Multi-Factor-Authentication (MFA) worden afgedwongen of kan federatief toegang worden verleend..

3.	Aan de hand van de uitkomst van stap 2 zal de API Gateway beslissen of het verzoek voor API-gebruik wordt ingewilligd. De API gateway fungeert hier als Policy Enforcement Point (PEP).

4.	Indien “Ja” bij stap 3 zal Autorisatie plaatsvinden. Het API-verzoek, aangevuld met metadata vanuit de authenticatie, wordt (in de regel in JSON format) naar de policy engine gestuurd. Hier wordt aan de hand van een vooraf gedefinieerde policy getoetst of het verzoek toegestaan is en de uitslag wordt teruggekoppeld naar de API-gateway. (Voor details over exacte werking, zie: [Aanvullende toelichting](#Hoewerkenpolicyagents)) 

5.  Aan de hand van de uitkomst van stap 4 zal de API Gateway beslissen of het verzoek wel/niet door mag. De API gateway fungeert hier als Policy Enforcement Point (PEP).

6.  Het verzoek wordt doorgestuurd naar de doel-API.

7.  De respons wordt teruggekoppeld aan de API gateway.

8.  De respons wordt teruggekoppeld aan de Client.

## Overzicht referentiecomponenten
De GEMMA onderscheidt 'applicatiecomponenten' die nodig zijn binnen de gemeentelijke informatiearchitectuur: "een modulair, zelfstandig inzetbaar en vervangbaar deel van een systeem, dat zijn functionaliteit aanbiedt via goed gedefinieerde interfaces. Applicatiecomponenten stellen een functionaliteit beschikbaar, die gebruikt wordt om de applicatiediensten mee te leveren". Binnen deze context is sprake van een drietal referentiecomponenten: de Identity Provider, een Policy engine en een API gateway (die als een Policy Enforcement Point, PEP, optreedt. De identity provider en de API gateway worden in de verschillende architectuurmodellen al toegepast en vergen geen verdere uitleg. Met betrekking tot de vermelde policy engine is de volgende toelichting te geven:

Een policy engine is een component die vaststelt of een opvraag voldoet aan een gedefinieerd toegangsbeleid. De Policy engine bestaat in de regel uit 2 componenten: de Policy Decision Point (PDP) en een Policy Administration Point (PAP). De PDP beoordeelt de opvraag die vanaf de Policy Enforcement Point (PEP) afkomstig is tegen de policy zoals die in de PAP is geregistreerd. In de XACML architectuur zijn de verschillende componenten afzonderlijk gemodelleerd, maar in de praktijk worden met name de PDP en de PAP functionaliteit binnen één component vervat.

Voorbeelden van de voor deze werkwijze noodzakelijke componenten zijn:


|IDP - Identity provider|PE - Policy Engine     |API gateway            |
|:---------------------:|:---------------------:|:---------------------:|
|Keycloak |OPA - Open Policy Agent|Kong	|
|Authentik							|Axiomatics							|Nginx									|
|Azure AD								|PlainID								|Mulesoft								|
|Okta										|												|Axway									|
|Auth0									|												|Ambassador							|

## Aanvullende toelichting

### OIDC/OAuth 2.0 koppeling
Het gebruik van het OAuth 2.0 autorisatieprotocol wordt in deze architectuur aangeraden. De configuratie hiervan is noodzakelijk om authenticatie plaats te laten vinden tussen de API-gateway en de IDP, waarna de API-gateway namens de initiërende identiteit de opvraag uitvoert, de gateway wordt namens de opvrager toegestaan de uitvraag te doen. Alhoewel er kleine verschillen kunnen zitten tussen verschillende applicaties en use-cases, blijft de configuratie in essentie grotendeels gelijk.

De configuratie begint met het aanmaken van een client in de identiteitenrepository (en als sprake is van een Identity as a Service component, IDAAS, ook in de IdP) waar de gebruikers in opgenomen zijn. Dit is een punt waar de API-gateway naar toe zal communiceren om informatie uit te kunnen wisselen en controleren.

Er zin drie verschillende configuratieprofielen voor clients denkbaar:
- Authenticatie van gebruikers en/of clients
- Authenticatie van clients,gebruikerstoegang via een client(applicatie
- OAuth 2.0 (systeem toegang, toegang namens een gebruiker) 

De keuze voor deze profielen is afhankelijk van de componenten en de gewenste flow. Dit moet expliciet gedocumenteerd worden.

Voor de exacte procedure van het aanmaken van een client is het verstandig de documentatie van de gekozen IDP zelf door te nemen. Hier staat meestal stap voor stap, uitgelegd hoe dit gerealiseerd kan worden.
Wat in essentie de bedoeling is, is dat er een client aangemaakt wordt, en hier enkele parameters worden opgehaald die naar de client verwijzen.
Ook hier is het advies om voor het gekozen platform naar de OIDC plugin documentatie te kijken. Hier wordt benoemd hoe, indien noodzakelijk, de plugin aangezet kan worden, en waar de bovenstaande parameters (provider_url, client_id, client_secret, etc.) ingevoerd dienen te worden.
Hierna zou de OIDC-koppeling gereed moeten zijn, en kan de OIDC aangezet worden op verschillende “routes” of “services”, of globaal toegepast worden. Dit houdt in dat voor bepaalde “wegen” naar een dienst toe OIDC afgedwongen wordt in het geval van “routes”. Of dat er voor bepaalde diensten OIDC afgedwongen wordt in het geval van services. Tot slot is het mogelijk ten alle tijden OIDC-authenticatie af te dwingen in het geval van globale deployment.

### Policy engines
Een policy agent is een fundamenteel onderdeel van Policy Based Access Control (PBAC). Echter zijn er meer elementen nodig om het concept werkend te krijgen, namelijk een PEP, PDP, en PAP (met optioneel een of meer PIPs). In de "ARCHITECTUURPLAAT" zijn deze aangegeven met blauwe vakjes. Effectieve inzet van deze elementen samen, zijn ook in lijn met de principes van Zero-Trust Architecture. Een toegangsfilosofie met als hoofdzakelijke uitgangspunten real-time autorisatie en granulariteit. Beide concepten, worden samen met de componenten hieronder toegelicht. (Voor meer informatie over Zero-Trust, zie: [NIST 800-207](https://csrc.nist.gov/publications/detail/sp/800-207/final)

PDP - Policy Decision Point
De hoofdzakelijke taak van een policy engine, is het maken van beslissingen. Afhankelijk van de configuratie binnen de API-gateway, wordt er op verschillende routes, services, of op globaal niveau beleid afgedwongen. De PDP is de plek waar de API-call met metadata binnenkomt, waarna deze getoetst wordt. Nadat de toetsing heeft plaatsgevonden, stuurt de PDP zijn beslissing terug naar de API-gateway. Het toetsen gebeurt aan de hand van vooraf gedefinieerd beleid, wat terug te vinden is in de PAP.

PAP - Policy Administration Point
Naast dat een policy engine de rol van PDP vervult, beheert deze ook vaak de vooraf gedefinieerde policies zelf. Deze zijn samengesteld in programmeertaal, of via een no-code gebruikersinterface. De inhoud van de policies is vaak terug te herleiden aan businesslogica en compliance maatregelen. Zo is het scheiding-van-taken principe te definiëren in het beleid, en zo toe te passen op een bepaald type API-calls, of aan de hand van metadata.

PEP - Policy Enforcement Point
Nadat de PDP een evaluatie heeft gedaan, krijgt de API-gateway hier de uitslag van. Gezien de APIgateway, afhankelijk van de uitslag, bepaalt of het verzoek door mag, vervult hij hiermee direct de functie van PEP.
Een voorbeeld om de verschillende onderdelen goed te kunnen plaatsen, is de verdeling van machten in een staatsbestel. De PDP is de rechtelijke, de PAP de wetgevende, en de PEP de uitvoerende macht.

PIP - Policy information Point
De bovenstaande elementen samengebracht, kunnen op deze manier elke transactie beoordelen of deze voldoet aan vooraf bepaald beleid. Hiermee wordt voldaan aan een fundamenteel Zero-Trust principe (real-time authorization). Daarnaast is het ook mogelijk, om meerdere policies te combineren, of op verschillende plekken in de keten afgedwongen laten worden. Met enkel authenticatie en autorisatie, is ook het principe van granulariteit gedemonstreerd. Het kunnen realiseren van gelaagde autorisatiestructuren biedt enorm veel mogelijkheden in het verlenen van fijnmazige toegang, wat gewenst is vanuit een veiligheidsperspectief.

Tot slot is het mogelijk dat op basis van de data van het API-verzoek, en bijbehorende metadata, onvoldoende informatie beschikbaar is om tot een beslissing te komen door de PDP. Het kan voorkomen dat externe databronnen geraadpleegd moeten worden, en deze vervullen in dit geval de rol van PIP. Zo kunnen er gedurende de overwegingen van een verzoek, API-calls gemaakt worden door de policy engine, om aanvullende data op te halen uit bijvoorbeeld een HR-systeem.



### Koppeling policy engine
In de meeste gevallen zijn policy engines aan te sluiten op de API-gateway door middel van een plug-in. In de componentkeuze is dit een onderdeel dat goed overwogen moet worden, gezien niet elke policy engine met even veel gemak aangesloten kan worden op elke API-gateway.

