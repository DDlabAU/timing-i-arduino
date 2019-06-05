# Timing-i-Arduino
Denne guide forklarer forskellen mellen millis() og delay() funktionen i arduino, samt hjælper med hvordan man kan bruge millis() til at lave forsinkelser i koden der ikke timeouter arduinoen.

## Delay()
### Generelt
Delay() er en af de første funktioner man lærer når man skal programmere i Arduino. Du bruger den f.eks. til at styrre timingen af LED'er eller andre forskellige funktioner. Fordelen med Delay() er at den er simpel og let at bruge. Problemet med Delay() er at den rent funktionelt slukker Arduinoen i det interval der bliver specificeret i Arduinoen.

### Delay og Tid
Dvs. at hvis du skriver Delay(1000) i din kode, så betyder det i praksis at Arduinoen slukker hele sit system 1 sekund hver gang den når til Delay() funktionen. Dette kan være fint nok til simple programmer. Problemet opstår hvis du skal lave programmer hvor du ønsker at eksekvere noget samtidig med at noget andet er forsinket er det ikke muligt at bruge Delay(). Derudover, hvis du skal lave et program der er meget tidssensitivt, virker Delay heller ikke, da dens nedsluk af systemet betyder at Arduinoens interne ur bliver forskudt.
F.eks. hvis du ville lave et æggeur der blinker hvert sekund med brug af Delay(1000), så ville dit program fungere i starten, men jo længere tid Arduinoen kører jo mere forskudt bliver Arduinoens sekunder ifht. rigtige sekunder.

Der er altså et ujævnt forhold mellem Arduinoens tid den faktiske tid. Et abstrakt eksempel på dette forhold kunne være at hvis du forestiller dig to stopur. Det ene stopur repræsenterer Arduinoens tid, og det andet repræsenterer den faktiske tid. Arduinoens stopur har en mikroskopisk forsinkelse efter hvert sekund, da selve eksekveringen af kode også tager tid. Hvis du starter begge stopurer samtidig vil de i starten virke til at de går lige hurtigt. Men efter lang nok tid vil det blive tydeligt at det ene stopur bliver forskudt ifht. det andet. Følgende billeder illustrerer forholdet mellem Arduinos tid og den faktuelle tid. Det venstre stopur repræsenterer Arduinoen og det højre repræsenterer den faktuelle tid. Hvis du refresher (F5 på tastaturet) denne side bliver det tydeligt hvordan tiden virker ens i starten, men bliver forskudt over tid.

![alt text](https://github.com/DDlabAU/Timing-i-Arduino/blob/master/tid/ArduinoTid.gif "Arduino Tid") ![alt text](https://github.com/DDlabAU/Timing-i-Arduino/blob/master/tid/RigtigTid.gif "Rigtig Tid")

### Opsumeret om Delay()
Der er altså både fordele og ulemper ved at bruge Delay(). Fordelen er at den er let at bruge, simpel at forstå og virker med det samme. Ulemperne er at Delay() "slukker" Arduinoen når den kører, så du kan ikke eksekverer andre funktioner imens, samt at det ikke er en så præcis funktion til timing og styring af tid. Her kan man med fordel bruge Millis()

## Millis()
### Generelt
