![Zebra logo](https://user-images.githubusercontent.com/117364775/217061291-ccd2e077-b383-4c09-a904-79de1f90c017.png)

## Progetto Physical Computing - IAAD - SID3 ACD
## Lucrezia Acomo, Sofia Corippo, Gaia Durio

### Contesto 

In un mondo in cui la strada è solo un luogo di passaggio e non d’interazione abbiamo deciso di portare all’attenzione un tema di sicurezza e territorio. Progettato sulle persone per attraversare coscienziosamente sulle strisce pedonali e ricordare alle autovetture che la strada non appartiene solo a loro. 
Abbiamo preso come riferimento l'attraversamento pedonale tra Via S.Teresa e Piazza S. Carlo. Essendo senza semaforo abbiamo pensato ad un sistema di webcam che percepisca quando una persona sta attraversando così da far accendere le luci che sono direttamente collegate alle videocamere. Il movimento delle luci sarà fluido e risulterà molto piacevole alla visione. 
Per dare un'aggiunta al nostro progetto artistico ci saranno dei dati rilevati prima e durante l’installazione che faranno vedere il cambiamento di quante persone attraversino veramente sulle strisce e chi no.

### Concept 

Installazione artistica che consiste in uno show di led a lato delle strisce pedonali che accompagnano chi sta attraversando il corso senza semaforo e rendono il pedone più visibile alle macchine che si accingono a passare 

### Hardware 

Nel bordo della strada, vicino alle strisce pedonali vi è una colonnina che contiene un sensore di movimento.  
Rilevando la persona che passa sulle strisce in base alla sua posizione, il sensore invia l'input al led. Il led restituisce la luce bianca che si accende in modalità fade.
![Procedimento](https://user-images.githubusercontent.com/117364775/217050036-cb91cece-4a10-480a-8e7b-7be8cd95cb4a.png)


### Software 

Su Ardunino abbiamo scritto un codice per far si che il sensore mandi l’input di accendere la luce alla stripled abbiamo scritto il codice . 
Abbiamo calcolato lo spazio entro il quale far accendere la luce in modalità fade per la lunghezza dell’attraversamento pedonale. 
Infine per far in modo che il sensore rilevasse solo ciò che sta all’interno delle strisce pedonali abbiamo limitato il raggio di rilevamento.
Una volta scritto e testato il codice ci siamo rese conto però di una piccola discrasia nella percezione del sensore. Infatti è stato inserito un codice per la "smoothness" del sensore per correggere la poca precisione.

### Impatto

Una volta che è stato installato il meccanismo, le persone hanno iniziato ad attraversare sulle strisce pedonali.
Incuriositi dall'installazione artistica le persone tendono a usare l'attraversamento pedonale per vedere le luci accendersi e spegnersi. Quindi abbiamo osservarto come un comportamento possa essere corretto da semplici azioni di diletto.

### Comunicazione

- collaborazione con il comune di Torino
- campagna online social
- qr code che rimanda al concept del progetto
- rassegna stampa da parte di giornalisti d'arte  

### Codice 
#include "Ultrasonic.h"

Ultrasonic ultrasonic(3);

#include "FastLED.h"
#define N 42
CRGB leds[N];

long oldRange;

void setup()
{
  Serial.begin(9600);
  FastLED.addLeds<NEOPIXEL, 6>(leds, N);
  FastLED.addLeds<NEOPIXEL, 7>(leds, N);
  oldRange = 0;
}
void loop()
{
  long RangeInCentimeters;
  int numLed;

  RangeInCentimeters = ultrasonic.MeasureInCentimeters(); // two measurements should keep an interval
  
  long avgInCentimeters = (oldRange + RangeInCentimeters)/2;

oldRange = avgInCentimeters; 


  Serial.println(RangeInCentimeters);//0~400cm

  numLed = map(RangeInCentimeters, 8, 69, 0, 41);

  for (uint8_t i=0; i<N; i++) {
    if (i == numLed) {
      leds[i] = CRGB:: White;
    }
    else {
      leds[i].fadeToBlackBy(60);
    }
  }
  
  FastLED.show();
  delay(30);
}

