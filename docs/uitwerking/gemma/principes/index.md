---
layout: page-with-side-nav
date: 23-03-2022
title: "Aanpassing GEMMA architectuurprincipes"
---
# Aanpassing en uitbreiding GEMMA architectuurprincipes

## Architectuurprincipes – Basis op orde

<table> 
<tr>
    <th><b>#</b></th> <th><b>Principe/b></th> <th><b>Rationale/b></th> <th><b>Implicatie</b></th>
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
    <td>Zonder classificatie is niveau maatregelen van specifieke data sets niet te bepalen.</td> <td><ul><li>Controleren huidige status</li><li>BIV-classificatie beleid opstellen (o.a granulariteit)</li><li>BIV-classificatie implementatie wijze bepalen</li><li>BIV-classificatie implementatie verrichten</li></ul></td>  
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
    <td><ul><li>Controleren huidige status</li><li>Opstellen van Zero Trust Architectuur (ZTA)</li><li>Acties bepalen voor implementatie ZTA</li></ul></td> 
</tr>  
<tr>  
    <td>B5</td> 
    <td>Veilige software code, minimaal conform OWASP standaarden</td> 
    <td>Wanneer eigen ontwikkeling plaatsvindt of ontwikkeling is uitbesteed moet dit volgens strakke standaarden gebeuren en moet hier op gecontroleerd worden om een goede score op BIV te realiseren.</td> <td><ul><li>Controleren huidige status</li><li>Softwareontwikkeling beleid opstellen</li><li>Softwareontwikkeling beleid gebruiken als controle wijze, voor gap analyse</li><li>Softwareontwikkeling gaps oplossen</td> 
</tr>  
<tr>  
    <td>B6</td> 
    <td>Voor beveiliging van communicatie wordt mTLS toegepast</td> 
    <td>Communicatie tussen Identity Provider en Service Provider (door tussenkomst van de 'user') is beveiligd</td> 
    <td><ul><li>De IdP is voorzien van een geldig TLS certificaat. Dat zal van een (in de browsers) bekende root-CA moeten zijn om externe werking te kunnen hebben.</li></ul></td>  
</tr> 
</table>