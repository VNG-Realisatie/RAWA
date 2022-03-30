---
layout: page-with-side-nav
date: 30-03-2022
title: "Aanpassing GEMMA architectuurprincipes"
---
# Aanpassing en uitbreiding GEMMA architectuurprincipes

Ten aanzien van de referentiearchitectuur voor het werken met API's zijn op een aantal vlakken architectuurprincipes benoemd op de volgende aspecten:

* [Basis op orde](./index.md#basis-op-orde)
* [Toegang](./index.md#toegang)
* [Policy based accces (PBAC)](./index.md#policy-based-access-control-pbac)
* [Federatie](./index.md#federatie)

Deze architectuurprincipes worden als aanvulling op de GEMMA principes opgenomen.

> De onderstaande principes zijn de eerste conceptversie van de principes. Gemeenten en leveranciers worden met nadruk gevraagd om input te leveren op deze concept principes. Het geven van input op de principes kan via het aanmaken van een [issue](https://github.com/VNG-Realisatie/IAM/issues) of door een mail te sturen naar de [product owner](mailto:arnoud.quanjer@vng.nl).

## Basis op orde

<table> 
<tr>
    <th><b>#</b></th> <th><b>Principe</b></th> <th><b>Rationale</b></th> <th><b>Implicatie</b></th>
</tr>  
<tr>  
    <td>B1</td> 
    <td>Elke IT oplossing is passend binnen het IT- landschap</td> 
    <td>Een applicatie die afwijkt van de standaarden zorgt voor extra beveiligingsrisico’s, extra onderhoud en breder benodigd specialisme.</td>  
    <td><ul><li>Controleren huidige status</li><li>IT-architectuur onderdeel laten zijn van inkoopprocessen ongeacht de grootte van de IT-oplossing</li></ul></td> 
</tr>  
<tr>  
    <td>B2</td> 
    <td>Data is altijd geclassificeerd</td> 
    <td>Zonder classificatie is niveau maatregelen van specifieke data sets niet te bepalen.</td> 
    <td><ul><li>Controleren huidige status</li><li>BIV-classificatie beleid opstellen (o.a granulariteit)</li><li>BIV-classificatie implementatie wijze bepalen</li><li>BIV-classificatie implementatie verrichten</li></ul></td>  
</tr>  
<tr>  
    <td>B3</td> 
    <td>Elke IT oplossing is voorzien van een baselinetoets en pre DPIA en afhankelijk daarvan een Baseline niveau / risicoanalyse en/of DPIA</td> 
    <td>Zonder de juiste documentatie per IT oplossing zijn de achtergrond en details niet bekend, waardoor impact op de organisatie, maar ook mogelijke gaps niet bekend zijn.</td>  
    <td><ul><li>Controleren van aanwezigheid templates</li><li>Opstellen van benodigde templates</li><li>Controleren huidige status</li><li>Controle proces opzetten voor uitvoering documentatie</li><li>Borgen van documentatie bij IT oplossing</li></ul></td> 
</tr>  
<tr>  
    <td>B4</td> 
    <td>Beveiligde communicatie is standaard</td> 
    <td>Inbreuken op integriteit en vertrouwelijkheid zijn niet acceptabel en communicatiebeveiliging dient standaard te zijn tussen interne en externe applicaties.</td> 
    <td><ul><li>Controleren huidige status</li><li>Opstellen van Zero Trust Architectuur (Zero Trust Architectuur)</li><li>Acties bepalen voor implementatie Zero Trust Architectuur</li></ul></td> 
</tr> 
<tr>  
    <td>B5</td> 
    <td>Veilige software code, minimaal conform Open Web Application Security Project (OWASP) standaarden</td> 
    <td>Wanneer eigen ontwikkeling plaatsvindt of ontwikkeling is uitbesteed moet dit volgens strakke standaarden gebeuren en moet hier op gecontroleerd worden om een goede score op BIV te realiseren.</td> 
    <td><ul><li>Controleren huidige status</li><li>Softwareontwikkeling beleid opstellen</li><li>Softwareontwikkeling beleid gebruiken als controle wijze, voor gap analyse</li><li>Softwareontwikkeling gaps oplossen</li></ul></td> 
</tr>  
<tr>  
    <td>B6</td> 
    <td>Voor beveiliging van communicatie wordt mTLS toegepast</td> 
    <td>Communicatie tussen Identity Provider en Service Provider (door tussenkomst van de 'user') is beveiligd</td> 
    <td><ul><li>De Identity Provider is voorzien van een geldig TLS certificaat. Dat zal van een (in de browsers) bekende root-CA moeten zijn om externe werking te kunnen hebben.</li></ul></td>  
</tr> 
</table>

## Toegang

<table> 
<tr>
    <th><b>#</b></th> <th><b>Principe</b></th> <th><b>Rationale</b></th> <th><b>Implicatie</b></th>
</tr>  
<tr>  
    <td>T1</td> 
    <td>Toegangsregels (tot welk object dan ook) worden rekening houdend met risicoanalyse op basis van BIV classificatie bepaald.</td> 
    <td>Bij een minimaal risico, hoeven er vanuit de optiek van efficiency alleen minimale beveiligingsmaatregelen te worden getroffen</br>Identiteiten kunnen zijn: mensen, services, processen, componenten, Robot Processing Automation (RPA), devices, dus zowel ‘human’ als ‘non-human’.</td> 
    <td><ul><li>Openbare informatie (C=laag) mag publiek toegankelijk zijn voor raadplegen (niet voor muteren, waar voor openbare informatie I=Hoog geldt).</li></ul></td>  
</tr>  
<tr>      
    <td>T2</td> 
    <td>Toegang tot beveiligde objecten wordt uitsluitend toevertrouwd aan vertrouwde identiteiten</td> 
    <td>Iedere toegangspoging wordt gerelateerd aan een identiteit van een vertrouwde identity provider.</td> 
    <td><ul><li>Of de Identity Provider voert het in-, door- en uitstroomproces van de Service Provider zelf uit, of er is een contractuele relatie tussen Service Provider en Identity Provider.</li><li>De identiteiten binnen Identity Provider worden (geautomatiseerd via een IAM-voorziening) beheerd vanuit de reguliere in-, door- en uitstroomprocessen van de organisatie.</li></ul></td>  
</tr>  
<tr>     
    <td>T3</td> 
    <td>Toegang vindt alleen plaats na validatie van geldigheid van de identiteit.</td> 
    <td>Zonder vast te stellen of de gebruiker die wil authentiseren wel gemachtigd is om te authentiseren en zonder de juiste authenticatie mogelijkheden kan de BIV van de IT-oplossing in gevaar komen.</td> 
    <td><ul><li>Validatie van de identiteit hangt samen met authenticatie van de digitale identiteit.</li><li>Identiteiten en accounts zijn voorzien van passende wachtwoorden (lengte prefereert boven complexiteit).</li><li>Multi-factor authenticatie als extra validatie- & beschermingsmaatregel.</li></ul></td>  
</tr>  
<tr>      
    <td>T4</td> 
    <td>Authenticatie vindt plaats via Single Sign-On na inlog bij een vertrouwde identity provider.</td>
    <td>Gebruiksgemak en verlagen beheerlast en risico identiteit management.</br>Separaat inloggen in applicaties, platformen en informatiesystemen is ongewenst.</td> 
    <td><ul><li>Federatieve toegang via SAML, OIDC of Oauth.</li><li>Voor legacy, on-prem systemen kan LDAP worden toegepast.</li></ul></td>
</tr>  
<tr>      
    <td>T5</td> 
    <td>Wachtwoordbeveiliging is end-of-life</td> 
    <td>Wachtwoorden zijn per definitie niet voldoende veilig</td> 
    <td><ul><li>Onderzoek gebruik van wachtwoorden en stel migratiepad op.</li></ul></td>
</tr>  
<tr>      
    <td>T6</td> 
    <td>Alle, al dan niet geslaagde, toegangspogingen tot de IT-oplossing worden gelogd.</td> 
    <td>Logging geeft inzicht in het gebruik, daaruit volgend trends en mogelijke aanvallen.</br>Verder is logging een vereiste om de BIV aspecten te waarborgen.</td> 
    <td><ul><li>Identiteit [Id], van Identity Provider [Identity Provider] heeft op datum [D] en tijd [T], toegang gevraagd tot resource [Res].</li><li>Identiteit [Id], van Identity Provider [Identity Provider] kreeg op datum [D] en tijd [T], op grond van policy [Pol], versie [Ver] met attributenset [Attr] toegang tot resource [Res].</li></ul></td>
</tr> 
</table>

## Policy Based Access Control (PBAC)

<table> 
<tr>
    <th><b>#</b></th> <th><b>Principe</b></th> <th><b>Rationale</b></th> <th><b>Implicatie</b></th>
</tr>  
<tr>  
    <td>P1</td> 
    <td>Er wordt uitsluitend toegang verleend als de toegangsvraag op het moment van aanvraag voldoet aan de policy om toegang te krijgen</td> 
    <td>Toegang is alleen mogelijk als alle relevante stakeholders daarin toestemmen</br>Verlenen van toegang vergt actuele informatie om dynamiek te ondersteunen</td> 
    <td><ul><li>Implementeren Access Governance.</li><li>Identificeren relevante belanghebbenden</li><li>Definiëren policy structuur</li><li>Definiëren Access architectuur</li></ul></td>
</tr> 
<tr>  
    <td>P2</td> 
    <td>Businessrollen binnen de IAM-voorziening kunnen worden gebruikt als attributen die bij de toegangspoging worden getoetst.</td> 
    <td>Informatie uit de IAM-voorziening kan worden gebruikt als input voor de keuzes voor toegang.</td> 
    <td><ul><li>De kwaliteit van informatie uit de IAM-voorziening moet goed zijn.</li><li>De koppeling tussen rollen en de externe toegang moet worden onderhouden, i.v.m. consequenties van wijzigingen.</li></ul></td>
</tr> 
<tr>  
    <td>P3</td> 
    <td>Als er geen policy is gedefinieerd, wordt er geen toegang verleend.</td> 
    <td>Conform BIO H9, Default deny</td> 
    <td><ul><li>Policies moeten expliciet worden vastgesteld</li><li>Policy enforcement wordt ingericht</li></ul></td>
</tr> 
<tr>  
    <td>P4</td> 
    <td>Het is niet toegestaan om zonder policyverificatie toegang te krijgen</td> 
    <td>Discretionary Access Control (BIO H9)</td> 
    <td><ul><li>Zie P3</li></ul></td>
</tr> 
<tr>  
    <td>P5</td> 
    <td>Risicoanalyse / BIV-classificatie zijn onderdeel van de access beslissing, onderdeel van de policy</td> 
    <td>Policy based access control houdt rekening met diverse attributen en regels die door belanghebbenden zijn vastgesteld</td> 
    <td><ul><li>Policies moeten verwijzen naar risico-analyse of BIV-classificatie</li><li>Ofwel: BIV > policy uitspraken</li></ul></td>
</tr> 
<tr>  
    <td>P6</td> 
    <td>Betrouwbaarheid van Identity Provider, Identiteit en authenticatieniveau is onderdeel van de access beslissing</td> 
    <td>Policy based access control houdt rekening met diverse attributen en regels</td> 
    <td><ul><li>Bij inlog of toegangspoging wordt authenticatiemiddel en –niveau gevalideerd</li><li>In Federatieve berichten is Multi Factor Authenticatie-niveau in een claim beschikbaar.</li><li>Voorkeur voor Stork-claims, te mappen op eIDAS</li></ul></td>
</tr>     
<tr>  
    <td>P7</td> 
    <td>Alle policies van alle belanghebbenden worden getoetst binnen de toegangsvraag</td> 
    <td>Policies moeten achtereenvolgens in samenhang werken</td> 
    <td><ul><li>Policy engines dwingen achtereenvolgens toepassing van policies af</li></ul></td>
</tr> 
</table>

## Federatie

<table> 
<tr>
    <th><b>#</b></th> <th><b>Principe</b></th> <th><b>Rationale</b></th> <th><b>Implicatie</b></th>
</tr>  
<tr>  
    <td>F1</td> 
    <td>Alle intern beheerde identiteiten worden per definitie vertrouwd als onderdeel van eigen beheercontext.</td>
    <td>et HR beheerproces voor alle interne identiteiten is vertrouwd en levert betrouwbare identiteiten en attributen.</td>
    <td><ul><li>De juiste maatregelen zijn ingeregeld om controle van interne identiteiten te borgen.</li></ul></td>
</tr>  
<tr>  
    <td>F2</td>
    <td>Alle externe identiteiten krijgen uitsluitend toegang op basis van federatieve verbindingen, op grond van federatieconvenanten of -contracten of vertrouwde, gecontracteerde federatieve frameworks.</td>
    <td>Externe identiteiten kunnen afkomstig zijn van diverse bronnen en daarom dient er een standaard manier van toegang verkrijgen te zijn.</td>
    <td><ul><li>Federaties worden aangegaan op instigatie van een eigenaar binnen de bedrijfsvoering</li><li>Alle externe toegang (dus inkomend en uitgaand) wordt beheerst via security gateways, conform een centraal beheerde security policy</li><li>Federatieve verbindingen worden regelmatig gecontroleerd op validiteit.</li></ul></td>
</tr>  
<tr>  
    <td>F3</td>
    <td>De Identity Provider voldoet aan de AVG voor het beheer van persoonsgegevens.</td>
    <td>De basis van een Identity Provider is het verstrekken van een vertrouwde identiteit en de daarbij benodigde persoonsgegevens.</td>
    <td><ul><li>Bepalen welke persoonsgegevens nodig zijn voor de SP</li><li>Borgen dat de Identity Provider alleen die gegevens doorgeeft</li><li>Borgen dat de Identity Provider aan de juiste veiligheidsnormen voldoet</li></ul></td>
</tr>  
<tr>  
    <td>F4</td>
    <td>De Identity Provider voorziet in digitale identiteiten die via OpenID Connect, Oauth (>= v2) en SAML (>=v2) beschikbaar worden gesteld voor federatieve toegang.</td>
    <td>Standaard protocollen moeten worden gebruikt om leverancier lock-in of speciale ontwikkelingen te voorkomen.</td>
    <td><ul><li>Uitsluitend gebruik van moderne federatieve protocollen volgens open industriestandaarden</li></ul></td>
</tr>  
<tr>  
    <td>F5</td>
    <td>De Identity Provider beschikt over een multi-factor authenticatiefaciliteit (Multi Factor Authenticatie).</td>
    <td>Multi Factor Authenticatie is een extra validatie- & beschermingsmaatregel voor de authenticatie tot de Identity Provider.</td>
    <td><ul><li>Gebruikers kunnen hun Multi Factor Authenticatie kwijtraken wat leidt tot het niet verkrijgen van toegang.</li><li>Onderzoeken welke Multi Factor Authenticatie mogelijkheden geboden moeten worden.</li></ul></td>
</tr>  
<tr>  
    <td>F6</td>
    <td>Uitgangspunt federatieve toegang: Identiteiten worden beheerd door de Identity Provider, services worden geleverd door de Service Provider (ook wel Relying Party genoemd).</td>
    <td>De oplossing is verantwoordelijk voor een specifiek deel van de functionaliteiten.</td>
    <td><ul><li>Splitsing van verantwoordelijkheden zorgt voor focus van functionaliteiten.</li></ul></td>
</tr>  
<tr>  
    <td>F7</td>
    <td>De Service Provider verleent toegang aan identiteiten die voldoen aan de door de Service Provider opgestelde kaders.</td>
    <td>Kaders zorgen voor een lijst met eisen aan de Identity Providers voor toegang verlening.</td>
    <td><ul><li>Kaders moeten worden opgesteld.</li><li>Consequenties van te strenge kaders moet worden gerealiseerd.</li></ul></td>
</tr>  
<tr>  
    <td>F8</td>
    <td>De toegangskaders zijn vastgelegd in een federatiecontract, in een federatieconvenant of in een federatief stelsel (het trust framework, bijv. eIDAS).</td>
    <td>Het gebruik van een framework zorgt ervoor dat het voor alle partijen duidelijk is wat zij kunnen verwachten. Daarmee ook wat zij kunnen bieden.</td>
    <td><ul><li>Bij gebruik van specifieke frameworks kunnen daar consequenties aan vastzitten wat betreft mogelijke dienstverlening.</li><li>Afwijken van frameworks zorgt voor uitsluiting.</li></ul></td>
</tr>  
<tr>  
    <td>F9</td>
    <td>De Identity Provider moet voldoen aan de aansluitvoorwaarden</br>mn. kwaliteit in-, door- en uitstroomproces, security-eisen, kosten.</td>
    <td>De basis van een Identity Provider is het verstrekken van een vertrouwde identiteit en daar horen strenge eisen bij vanuit de SPs.</td>
    <td><ul><li>De Identity Provider moet bepalen welk niveau van eisen zij gaan invullen en welke SPs zij willen bedienen.</li></ul></td>
</tr>  
<tr>  
    <td>F10</td>
    <td>De Service Provider moet voldoen aan de aansluitvoorwaarden</br>mn. privacyborging, doelbinding etc.</td>
    <td>Net als bij een Identity Provider zijn er bepaalde eisen waar een Service Provider aan moet voldoen.</td>
    <td><ul><li>SPs moeten duidelijk stellen waarvoor zij de identiteit informatie gebruiken en de consequenties van veranderingen zijn.</li><li>Consent moet opnieuw worden gevraagd bij de identiteit bij een wijziging.</li></ul></td>
</tr> 
</table>