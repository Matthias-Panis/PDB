Beginnend met de uitleg van wat een GRAFCET is en waarom deze nodig is. Dit is dus nodig bij geautomatiseerde systemen die een sequentieel proces gevolgen(stap voor stap).
- De GRAFCET zal stap voor stap lopen.
- Het heeft een initiële stap waarin hij altijd begint.
- Een overgang zal met een wiskundige boolean expressie getoond worden.
- Het resultaat hiervan zal of terwijl TRUE of FALSE zijn.
- Er zal altijd 1 actieve stap uitgevoerd worden.
- Alleen stappen die verbonden zijn met actieve stap zullen kunnen uitgevoerd worden.
- Andere stappen kunnen geactiveerd worden op voorwaarden dat ze verbonden zijn met de actieve stap en als het resultaat van de overgang TRUE is.

Verder bespreekt men hoe je een GRAFCET opstelt. Hierbij leggen we de stappen, connecties, transities, acties en structuren en functie regels uit.


| **Begrip** | **Uitleg** |
| :---:      | :---            |
| GRAFCET diagram | Een GRAFCET diagram is een collectie van stappen, acties, transities, connecties, etc |
| Step   | Een stap weergeeft een bepaalde conditie van het sequentieel proces. Een stap is of terwijl actief of niet actief  |
| Connections | "Connections" zijn lijnen in het netwerk die stappen connecteert |
| Actions |  Een "Action" is toegewezen aan een stap. Deze zal verbonden zijn met de stap via een horizontale lijn. Het is toegestaan om meerdere acties per stap uit te voeren. Elk heeft hun eigen rechthoek. |
| Structure   |  Een "structure" is een serie van stappen waar elke stap maximaal een transitie conditie  heeft |
| Function rules   |  **De functie van een GRAFCET** is normaal stap voor stap. Als de voorwaarden voor de volgende stap voldaan worden zal de volgende stap geactiveerd worden en de vorige stap gedeactiveerd. |

Elk mogelijk symbool dat m'n kan gebruiken in een GRAFCET zal ook uitgelegd worden. Voorbeeld hiervan (alle mogelijke stappen uitgelegd): <p><p>

![Steps GRAFCET](../PDB/Images/Steps_Ex.jpg)

Ten slotte zal er ook een voorbeeld gegeven om verdere uitleg te geven over het onderwerp.
In dit voorbeeld zullen we een simpele transportband van links naar rechts en omgekeerd laten draaien. Door middel van een start stop sturing zullen we deze starten en stoppen. Telkens dat we op stop duwen zal de transportband meteen stoppen. Als we op start zal de GRAFCET en transportband verder gaan waar ze  gebleven waren. De fotocel sensoren zullen de aanweezigheid detecteren van de box. De status van de fotocellen (%I) zijn gelinked met de GRAFCET variabelen "iSenFw" en "iSenBw".
Het controleren van de transportband zal gebeuren in stap 1 en 2 op de conditie dat de installatie start.
De effectieve besturing van the transportband zal gebeuren de GRAFCET variabelen "oBeltFw en oBeltBw" die dat gelinked zijn aan de contactoren (%Q)
Het tellen van het aantal keer dat de transportband voorwaards en achterwaards beweegt door internal INT variabele "i".
De transportband : <p><p>
![Example Conveyorbelt](../PDB/Images/ConveyorBeltEx.jpg)
Geruik makend van dit start stop circuit: <p><p>
![Example Conveyorbelt](../PDB/Images/StartStopEx.jpg)
Hiervoor zullen we een GRAFCET als volgt bouwen: <p><p>
![Example GRAFCET](../PDB/Images/GRAFCET_Ex.jpg)

### 8-2-2 Programmatie in TIA PORTAL

Om dan deze GRAFCET in TIA Portal te programmeren zijn er verschillende methoden om dit te doen. Verder uitgelegd zullen deze methodes aanbod komen.

- GRAFCET programming in LAD/FBD using BOOL
- GRAFCET programming in LAD/FBD using INT
- GRAFCET programming in ST

### 8-2-2-1 GRAFCET programming in LAD/FBD using BOOL

De programmatie zullen in deze 3 netwerken verdeeld worden:
- Initialization
- Transition-conditions
- Actions

De volgende regels worden toegepast

- Elke stap heeft een unieke BOOL variabelen
- Deze variabelen is een ARRAY van BOOL startend met 0 en eindigd op het maximum stap nummer.
- Als de juiste variabelen TRUE is zal de gelinkte stap actief worden.
- Input "ilnit" is altijd aanweezig wat de activatie van de intiale stap kan activeren bij een stijgende flank.
- Input "iStarted" wat het resultaat is van de start stop sturing


| **Advantages** | **Disadvantages** |
| :---:          | :---:             |
| Simplicity (1 step = 1 variable) | Initial step is not activated during the first download of the program |
|                                | Monitoring of active steps is complicated        |
### 8-2-2-2 GRAFCET programming in LAD/FBD using INT

De programmatie zullen in deze 3 netwerken verdeeld worden:
- Initialization
- Transition-conditions
- Actions

De volgende regels worden toegepast

- Alleen de actieve stap moet gekend zijn
- De actuele stap is een STATIC INT variabelen (step)
- De initiële waarde van de variabelen is het decimale waarde 0
- Input "ilnit" is altijd aanweezig wat de activatie van de intiale stap kan activeren bij een stijgende flank.
- Input "iStarted" wat het resultaat is van de start stop sturing


| **Advantages** | **Disadvantages** |
| :---:          | :---:             |
| Initial step is activated during the first download of the program | More complex, advanced programming then with LAD/FBD BOOL method |
| Monitoring of active steps is easier | Programming of AND-convergence is more complex then with LAD/FBD BOOL variant |

### 8-2-2-3 GRAFCET programming in ST

De programmatie zullen in deze 3 netwerken verdeeld worden:
- Initialization
- Transition-conditions
- Actions

De volgende regels worden toegepast

- Het gebruik van CASE .. OF .. ELSE control structure voor het verwerken van de overgangs condities.
- Alleen de actieve stap moet gekend zijn
- De actuele stap is een STATIC ANY_INT variabelen (step)
- De initiële waarde van de variabelen is het decimale waarde 0
- De actuele waarde van deze variabelen is gelinked aan de actieve GRAFCET stap.
- De intiale stap is automatisch geactiveerd de eerste keer het programma gedownload wordt naar de PLC.
- Input "ilnit" is altijd aanweezig wat de activatie van de intiale stap kan activeren bij een stijgende flank.
- Input "iStarted" wat het resultaat is van de start stop sturing

| **Advantages**| **Disadvantages** |
| :---: | :---: |
| Initial step is not activated while the the first download of the program | More complex programming than LAD/FBD variant |
| Smaller programming then LAD/FBD variant | Programming of AND-convergence is more complex than the LAD/FBD BOOL method |
| Monitoring of active steps are easier | Debugging in ST is harder than in FBD/LAD |

## 8-3 Addendum 05 controllers

In dit hoofdstuk worden regelaars of terwijl "controllers" besproken. Waaronder:
- ON-OFF circuit ("Aan-uit schakeling")
- PID controller ("PID regelaar")
- Singular control circuit (Enkelvoudige regelkring)
- Cascade control
- Ratio controller ("Verhoudingsregeling")
- Mix ratio controller ("Mengverhouding regeling")
- Split range controllers

Eerst zijn de eigenschappen van controllers uitgelegd. Deze bevatten de karakterestieken die de controllers die hier besproken worden.
![Example Conveyorbelt](../PDB/Images/Controller-properties.jpg)

### 8-3-1 Aan-uit schakeling

Aan-uit schakeling zal er voor zorgen dat een actuator niet constant aan-uit geschakeld zal worden. Dit gebeurt door 2 grenswaarden, in- en uitschakeldrempel. Het verschil tussen deze 2 grenswaarden wordt de hysteresis genoemd.

### 8-3-2 PID regelaar

Een PID regelaar wordt gebruikt om processen te regelen via een analoge actuator. Een PID regelaar bestaat uit verschillende deelfuncties.
- P of Proportionele actie
- I of Integrerende actie
- D of Differentiërende actie

### 8-3-3 Compact PID regelaar

In deze cursus gebruiken we de compact PID regelaar van TIA-Portal (S7-1200). Een heel groot deel van de paramaters kunnen ingesteld worden gebruik maken van TIA-Portal.



## 8-4 Addendum 06 Software modeling ANSI/ISA-S88

S88 software model is een norm die omschrijft hoe een machine/installatie (batch)proces kan onderverdeeld worden in verschillende onderdelen.

Het S88 software model deelt een machine/installatie proces op in 3 grote delen nl.:

- Het fysisch gedeelte (The physical part)
- Het procedure gedeelte (The procedure part)
- Het recepten gedeelte (The recipe part)

![Example Conveyorbelt](../PDB/Images/S88_Softwaredesign.jpg)

De volgende hoofdstukken zal de verschillende bouwstenen bespreken die in een library zijn opgenomen. Deze zullen in de programmatie van oefeningen kunnen gebruikt worden.

### 8-4-1 Fysich gedeelte - Control modules
**Control modules** zijn software bouwstenen die

- Ingangssignalen (%I) afkomstig van de sensoren verwerken
- Uitgangssignalen (%Q) naar de actuatoren aansturen

In de library zijn er sensor control modules aanweezig, waaronder een digitale sensor(digital sensor) en analoge sensor(analog sensor).

Een **digitale sensor** verwerkt voornamelijk de aanwezigheid van een product, voorwerp, persoon, enz. en heeft 2 toestanden die weergeven of deze producten, voorwerpen en personen al dan niet aanwezig zijn.
Functioneel gezien verzorgen deze sensoren:
- De correcte automatische werking al dan niet met de nodige vertragingen (bijv. openen van een deur m.b.v. een fotocel waarna de deur even open blijft)
- De beveiliging tegen defecten (bijv. droogloop beveiliging)

Zo'n bouwsteen ziet er als volgt uit:
![Object sensor](../PDB/Images/ObjectDigitalSensor.jpg)

Met de omschrijving van de werking kan er vervolgens een werkingsschema opgesteld worden.

![Werkingschema digitale sensor](../PDB/Images/OperationschemeCMFB_CM_DI_Sensor.jpg)

In TIA zal dit overeen komen met:
![Tia digitale sensor](../PDB/Images/TIA-FB_CM_DI_Sensor.jpg)
Of simpel weg in de S88 vorm:
![Tia digitale sensor](../PDB/Images/SimpleFB_CM_DI_Sensor.jpg)

Dit wordt dan ook helemaal uitgelegd voor een
- **Analoge sensor (FB_CM_AI_Sensor)**
- **Motor aandrijving (FB_CM_DOL/FB_CM_DOLRev)**
- **Lamp (FB_CM_Lamp)**
- **Contactor/relay (FB_CM_Relay)**
- **Ventiel (FB_CM_Valve)**

### 8-4-2 Fysisch gedeelte - Equipment modules

Een equipment module is een verzameling van meerdere control modules en/of andere equipment modules. Hierbij wordt de verzameling opgebouwd volgens hun onderlinge fysische relatie tot elkaar.

M.a.w. een equipment module is een software bouwsteen die minimum 2 bouwstenen bevat van het type controle module en/of equipment module.

Equipment modules worden bij voorkeur geprogrammeerd in “Functies” waarbij de TAG-benaming wordt uitgebreid met de letters EM.

### 8-4-3 Procedure gedeelte - Procedure element
Een procedure is een strategie, denkwerk om een probleem op te lossen. Hierbij wordt een strategie eerst op papier uitgewerkt volgens een bepaald methode. Vervolgens wordt de strategie vertaald naar software bouwsteen welke men het procedure element noemt.
Het uitwerken van een strategie kan op volgende manieren:
- Door het ontwerpen van een GRAFCET tekening
- Door het ontwerpen van een flowchart tekening
- Door het bepalen van de nodige wiskundige formules
- Door het tekenen van een werkingsschema
- Door de selectie van een regelaar en het bepalen van de bijhorende parameters in de vorm van een tabel

### 8-4-4 Eigen ontwerp
Niet al het denkwerk kan men verzamelen in standaard procedures. Vaak is het noodzakelijk om eigen procedures en procedure elementen te ontwerpen. Hierbij wordt één van de volgende analyse methode toegepast om een strategie te bepalen:

- GRAFCET
- Flowchart
- Wiskundige formule
- Werkingsschema
- Selectie regelaar & toelichting regelparameters



Voor elke procedure dient men een vereenvoudigde voorstelling van het procedure element te ontwerpen zodat men dit kan toevoegen aan het S88 software ontwerp.

Tot slot vormt men de procedure om naar een procedure element door deze te programmeren in de meest geschikte programmeertaal.
