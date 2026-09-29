# Fletxes Lliures

Trencaclosques de lògica per al navegador. El tauler és ple de fletxes i has de fer-les sortir totes, en l'ordre correcte, abans que s'acabi el temps.

El joc és un sol fitxer HTML (`fletxes.html`) amb HTML, CSS i JavaScript propis, sense cap framework ni llibreria.

## Com jugar

1. Toca (o fes clic a) una fletxa.
2. Si el camí des del cap de la fletxa fins a la vora del tauler és lliure, la fletxa llisca seguint el seu traç i surt del tauler.
3. Si topa amb una altra fletxa, rebota i perds una vida.
4. Allibera totes les fletxes abans que s'acabi el temps per superar el nivell.

Cada fletxa que surt deixa lliure el camí d'altres fletxes. Per això has de pensar en quin ordre les toques.

## Regles

### Vides
- Tens **3 vides** per intent.
- Perds una vida cada vegada que una fletxa topa amb una altra.

### Temps
- Cada nivell té un temps límit que depèn del nombre de fletxes:
  `temps inicial = 15 s + 3,5 s × fletxes`, arrodonit a múltiples de 5 i amb un mínim de 30 s.
- El rellotge comença quan es carrega el nivell i s'atura quan toques l'última fletxa.
- Si canvies de pestanya, el rellotge es posa en pausa.

### Intents
- Si **s'acaba el temps** o et **quedes sense vides**, perds l'intent i el nivell torna a començar amb **més temps**.
- Cada intent nou afegeix un 25 % del temps inicial (com a mínim 10 s):

  | Intent | Temps disponible               |
  |--------|--------------------------------|
  | 1/3    | temps inicial                  |
  | 2/3    | temps inicial + 1 × temps extra |
  | 3/3    | temps inicial + 2 × temps extra |

- Si falles el tercer intent, el nivell torna a l'**intent 1** amb el temps inicial.
- El botó **Reinicia** torna a començar el tauler amb el temps de l'intent actual i no gasta cap intent.

### Pistes
- Tens **3 pistes** per nivell. Cada pista marca en taronja una fletxa que ja pot sortir.

### Pausa
- El botó **Pausa** atura el rellotge i amaga el tauler, perquè no el puguis estudiar amb el temps aturat.
- El joc també es posa en pausa sol si canvies d'aplicació o de pestanya.
- Per continuar, toca **Continua**.

### Començar de zero
- El botó **De zero** torna la partida al nivell 1 i esborra els punts, els rècords, els intents i els nivells desbloquejats.
- Abans d'esborrar-ho, el joc et demana confirmació. Mentre decideixes, el rellotge està aturat.

## Puntuació

Quan superes un nivell, es calculen els punts així:

| Concepte                     | Punts                  |
|------------------------------|------------------------|
| Cada fletxa alliberada       | +10                    |
| Cada segon que et sobra      | +5                     |
| Cada segon de temps extra    | −10                    |

- La puntuació d'un nivell mai baixa de 0.
- El total de la capçalera suma la **millor puntuació de cada nivell**. Repetir un nivell només millora el total si hi fas un rècord.

**Exemple:** un nivell de 16 fletxes té un temps inicial de 1:10 i un temps extra de 20 s.
- Si el superes al primer intent amb 25 s sobrants: 160 + 125 = **285 punts**.
- Si el superes al segon intent (1:30) amb 25 s sobrants: 160 + 125 − 200 = **85 punts**.

## Nivells

- Els nivells es generen automàticament a partir del número de nivell. En una mateixa pantalla, un nivell sempre té el mateix tauler.
- La mida del tauler creix amb el nivell (de 30 caselles al nivell 1, 12 més per nivell, fins a un màxim de 420), amb fletxes cada vegada més llargues.
- **El tauler s'adapta a la pantalla:** les caselles es reparteixen segons la proporció de l'espai disponible (més files que columnes al mòbil, més columnes a l'ordinador) i cada casella fa entre 30 i 52 px, perquè les fletxes siguin fàcils de tocar i no es vegin massa grans. Per això, als mòbils els nivells alts tenen taulers una mica més petits que a l'ordinador. Si gires el mòbil a mig nivell, el tauler no canvia fins al nivell següent o fins que el reinicies.
- **Tots els nivells tenen solució.** El generador col·loca les fletxes en ordre invers: cada fletxa nova ha de tenir el camí lliure respecte de les que ja hi són. Retirant-les de l'última a la primera, sempre es pot resoldre.
- Amb els botons ‹ i › al costat del número de nivell pots tornar a nivells anteriors o avançar fins al més alt que hagis desbloquejat.

## Controls

| Acció               | Ratolí / tàctil          | Teclat |
|---------------------|--------------------------|--------|
| Fer sortir una fletxa | Clic o toc a la fletxa | —      |
| Pausa / continuar   | Botó **Pausa**           | `Espai` o `Esc` |
| Demanar una pista   | Botó **Pista**           | `P`    |
| Reiniciar el tauler | Botó **Reinicia**        | `R`    |
| Començar de zero    | Botó **De zero**         | —      |
| Canviar de nivell   | Botons ‹ i ›             | —      |

## Com executar-lo

No cal instal·lar res ni fer servir cap servidor. Obre `fletxes.html` amb qualsevol navegador modern (Chrome, Firefox, Safari o Edge), a l'ordinador o al mòbil.

Si el vols penjar a internet, puja el fitxer a qualsevol allotjament estàtic (GitHub Pages, Netlify, el teu servidor web…).

## Detalls tècnics

- **Un sol fitxer:** HTML, CSS i JavaScript dins de `fletxes.html`.
- **Sense dependències:** no fa servir cap framework ni llibreria. L'única cosa externa són les tipografies *Unbounded* i *Figtree* de Google Fonts. Si no es poden carregar, el joc fa servir les tipografies del sistema.
- **Dibuix:** el tauler es dibuixa amb `<canvas>` i s'adapta a la mida de la pantalla i a la densitat de píxels.
- **Mòbil:** en pantalles de 560 px d'amplada o menys, el tauler ocupa tota l'amplada, sense marges laterals, i la capçalera i el peu es compacten (dues files a dalt i una barra de quatre botons a baix).
- **Tema:** s'adapta automàticament al mode clar o fosc del sistema.
- **So:** efectes curts generats amb Web Audio API. Al mòbil, vibra quan hi ha un error (si el dispositiu ho permet).
- **Desament:** el nivell actual, el nivell màxim desbloquejat, els intents i els rècords es desen al `localStorage` del navegador. Si el navegador no el permet (per exemple, en mode privat), el joc funciona igual però no recorda el progrés.

### Claus de `localStorage`

| Clau               | Contingut                                  |
|--------------------|--------------------------------------------|
| `fletxes.level`    | Nivell actual                              |
| `fletxes.max`      | Nivell més alt desbloquejat                |
| `fletxes.attempts` | Intent actual de cada nivell               |
| `fletxes.best`     | Millor puntuació de cada nivell            |

Per començar de zero, esborra aquestes claus des de les eines de desenvolupador del navegador.

### Paràmetres que es poden ajustar

Al principi del codi JavaScript hi ha les constants i funcions que controlen el joc:

| Nom                  | Valor per defecte | Què controla                         |
|----------------------|-------------------|--------------------------------------|
| `MAX_LIVES`          | 3                 | Vides per intent                     |
| `MAX_HINTS`          | 3                 | Pistes per nivell                    |
| `MAX_ATTEMPTS`       | 3                 | Intents abans de tornar al temps inicial |
| `PTS_ARROW`          | 10                | Punts per fletxa                     |
| `PTS_SECOND`         | 5                 | Punts per segon sobrant              |
| `PEN_EXTRA_SECOND`   | 10                | Punts restats per segon extra        |
| `baseTimeFor()`      | 15 + 3,5 × fletxes | Temps inicial del nivell            |
| `extraFor()`         | 25 % (mín. 10 s)  | Temps extra per intent               |
| `levelParams()`      | 30 + 12 per nivell (màx. 420) | Nombre de caselles del nivell i llargada de les fletxes |
| `MIN_CELL`           | 30                | Mida mínima de casella, en píxels    |
| `MAX_CELL`           | 52                | Mida màxima de casella, en píxels    |
| `boardDims()`        | —                 | Adapta columnes i files a la pantalla |
