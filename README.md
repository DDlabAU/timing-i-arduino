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
Der er altså både fordele og ulemper ved at bruge Delay(). Fordelen er at den er let at bruge, simpel at forstå og virker med det samme. Ulemperne er at Delay() "slukker" Arduinoen når den kører, så du kan ikke eksekverer andre funktioner imens, samt at det ikke er en så præcis funktion til timing og styring af tid. Her kan man med fordel bruge Millis().

## Millis()
### Generelt
Millis() er en lidt mere kompliceret funktion ifht. Delay(), men den åbner også op for flere muligheder i dit program. Millis() er en funktion der returnere et tal afhængig af hvor mange millisekunder der er gået siden Arduinoen blev tændt. F.eks vil variablen let tid = millis() være 0 når Arduinoen bliver tændt, og 10.000 efter 10 sekunder. Jo længere tid Ardunioen er tændt, jo højere et tal vil Millis() returnere. Efter 1 minut vil millis() returnere 60.000 (1000\*60), efter 1 time 3.600.000 (1000\*60\*60), efter 1 døgn 86.400.000 (1000\*60\*60\*24) og efter 50 dage 4.320.000.000 (1000\*60\*60\*24\*50). Afhængig af hvor lang tid din Arduino skal køre så ender Millis() med at returnere nogle meget høje tal. Efter 50 dage bliver tallet så højt at Arduinoen bliver nødt til at lave noget der hedder *Overflow*. 

### Overflow
Overflow er noget der sker når en variabel får en talværdi der er så stor at den fylder for meget på Arduinoen. Hvis tallet bliver nået overflower det, hvilket betyder at variablen vender tilbage til 0. Da vi ønsker at Arduinoen skal køre så lang tid som muligt uden at Millis() skal lave et Overflow skal variablen gemmes som datatypen *Unsigned Long*.
*Unsigned* betyder at tallet kun kan være positivt. Dette betyder at Arduinoen kan gemme et højere tal, da minus-tal ikke skal medregnes, og tiden aldrig bliver returneret som et minus-tal. Det højeste tal der kan gemmes i en Unsigned Long variabel er 4.294.967.295, som svarer til ca. 50 dage før at millis() variablen vil blive så høj at den vil lave et *Overflow*. Overflow gør at Millis variablen går forbi 4.294.967.295 og så vender tilbage til 0 og starter derfra igen.

### Millis() i praksis

