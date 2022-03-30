---
layout: page-with-side-nav
date: 23-03-2022
title: "Aanpassing GEMMA architectuurprincipes"
---
# Aanpassing en uitbreiding GEMMA architectuurprincipes

## Architectuurprincipes – Basis op orde

| **#** | **Principe** | **Rationale** | **Implicatie** |
| --- | --- | --- | --- |
| B1 | Elke IT oplossing is passend binnen het IT- landschap | Een applicatie die afwijkt van de standaarden zorgt voor extra beveiligingsrisico’s, extra onderhoud en breder benodigd specialisme. | <ul><li>Controleren huidige status</li><li>IT-architectuur onderdeel laten zijn van inkoopprocessen ongeacht de grootte van de IT-oplossing</li></ul>
| B2 | Data is altijd geclassificeerd | Zonder classificatie is niveau maatregelen van specifieke data sets niet te bepalen. | <ul><li>Controleren huidige status</li><li>BIV-classificatie beleid opstellen (o.a granulariteit)</li><li>BIV-classificatie implementatie wijze bepalen</li><li>BIV-classificatie implementatie verrichten</li></ul> | 
| B3 | Elke IT oplossing is voorzien van een baselinetoets en pre DPIA en afhankelijk daarvan een Baseline niveau / risicoanalyse en/of DPIA | Zonder de juiste documentatie per IT oplossing zijn de achtergrond en details niet bekend, waardoor impact op de organisatie, maar ook mogelijke gaps niet bekend zijn. | <ul><li>Controleren van aanwezigheid templates</li><li>Opstellen van benodigde templates</li><li>Controleren huidige status</li><li>Controle proces opzetten voor uitvoering documentatie</li><li>Borgen van documentatie bij IT oplossing</li></ul> | 
| B4 | Beveiligde communicatie is standaard | Inbreuken op integriteit en vertrouwelijkheid zijn niet acceptabel en communicatiebeveiliging dient standaard te zijn tussen interne en externe applicaties. | <ul><li>Controleren huidige status</li><li>Opstellen van Zero Trust Architectuur (ZTA)</li><li>Acties bepalen voor implementatie ZTA</li></ul> | 
| B5 | Veilige software code, minimaal conform OWASP standaarden | Wanneer eigen ontwikkeling plaatsvindt of ontwikkeling is uitbesteed moet dit volgens strakke standaarden gebeuren en moet hier op gecontroleerd worden om een goede score op BIV te realiseren. | <ul><li>Controleren huidige status</li><li>Softwareontwikkeling beleid opstellen</li><li>Softwareontwikkeling beleid gebruiken als controle wijze, voor gap analyse</li><li>Softwareontwikkeling gaps oplossen | 
| B6 | Voor beveiliging van communicatie wordt mTLS toegepast | Communicatie tussen Identity Provider en Service Provider (door tussenkomst van de 'user') is beveiligd | <ul><li>De IdP is voorzien van een geldig TLS certificaat. Dat zal van een (in de browsers) bekende root-CA moeten zijn om externe werking te kunnen hebben.</li></ul> | 
