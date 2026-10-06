# Terminskompassen – projektplan

## Syfte
Minska lärarens administrativa börda så att mer tid går till undervisning.
Mått på framgång: **tid sparad per lärare och vecka**, inte antal funktioner.

## Målgrupp
- Först: enskild lärare, i första hand nya lärare (mest ovana vid planering och rättning).
- Senare: hela skolor. Inget i designen får förhindra det, men vi bygger inte för det än.

## Princip
Skolverkets centrala innehåll (Lgr22) är ryggraden. Allt läraren gör kopplas till samma punkter:
planering → bedömning → omdöme. Mata in en gång, återanvänd överallt.

Data: [Skolverkets Syllabus API](https://api.skolverket.se/syllabus/v1), öppet, ingen nyckel, CORS öppet.
Centralt innehåll kommer som HTML: `<h4>` rubrik + `<li>` punkter, per årskursspann (1-3, 4-6, 7-9).

## Principer
1. **UI utan kompromisser.** Proffsig, genomtänkt, tillgänglig (WCAG). Design tas fram med Claude Design inför fas 5.
2. **Elevdata stannar lokalt** (webbläsaren) tills annat beslutats. Export/backup tidigt. Inga elevnamn till extern tjänst.
3. **Enklast som fungerar.** Ren HTML/JS tills filen blir svårhanterlig; ramverk avgörs då.
4. **Verifiera med riktiga lärare** efter varje fas.

## Faser
| Fas | Innehåll | Klart när |
|---|---|---|
| 0 ✔ | Hämta ämne, visa centralt innehåll, bocka av punkter (`index.html`) | Fungerar lokalt |
| 1 ✔ | **Planering:** moment med veckointervall kopplade till punkter, täckning och luckor, flera klasser (7A/7B), kopiera plan, export/import som JSON-fil | Lärare kan lägga en hel termin och se luckor. Klart, men inte provat av riktig lärare ännu |
| 2 | **Rättning/bedömning:** uppgifter och prov kopplade till punkter, snabb inmatning av resultat per elev, översikt per elev och klass | Resultat per elev och punkt utan dubbelarbete |
| 3 | **Omdömen/betyg:** underlag från fas 2, utkast utan elevnamn, sammanställning att föra över till skolans system | Omdömesunderlag på minuter |
| 4 | **Anpassningar:** per elev och moment, återanvändbara | Anpassningar syns i planeringen |
| 5 | **Gold standard:** design, tillgänglighet, test, export/import, driftsättning (statisk sida) | Redo för riktiga användare |
| 6 | Eventuellt: delning/skola, inloggning/server, gymnasiet (GY25), AI-stöd efter integritetsbeslut | Beror på behov |

Fas 1 och 2 prioriteras (planering och rättning tar mest tid). Efter intervjun kan ordningen ändras.

## Öppna frågor
- [ ] Intervju med nyexaminerad lärare: hur ser en vecka ut, var försvinner tiden, vilka verktyg används idag (Google Classroom, Unikum, Schoolsoft, Excel)?
- [ ] "Täckt" – per termin eller över hela spannet 7–9?
- [ ] Momentets fält: nöjer vi oss med namn + veckor + kopplade punkter, eller även lektionsantal/bedömning/anteckningar?
- [ ] Hur går resultat från rättning in? Manuellt per elev, klistra in från kalkylblad, eller något annat?
- [ ] Integritet: vad kräver skolor om vi senare lagrar elevdata centralt?
- [ ] Hosting: statisk sida (t.ex. GitHub Pages) tills delning behövs.

## Kvar från fas 1
- `prompt`/`confirm` (kopiera plan, ta bort) ska bytas mot riktiga dialoger i designfasen.
- Provning med riktig lärare.
- Täckning mäts nu över hela det valda årskursspannet. Frågan om per termin eller över 7–9 är fortfarande öppen.

## Kända risker
- Skolverket kan ändra texten i centrala innehållet. Punkter saknar eget id (nyckel = ämneskod + spann + rubrikindex + punktindex + text). Nyckeln innehåller texten, och en varning visas när en koppling inte längre matchar. Kopplingen återställs inte automatiskt, så ändrad text kräver att läraren kopplar om.
- `localStorage` kan rensas av webbläsaren. Export/import finns; läraren måste själv komma ihåg att exportera.
- Import lägger till planer och skriver aldrig över befintliga. Dubbletter kan uppstå om samma fil importeras flera gånger.
- Skolans administrativa system har sannolikt ingen öppen import. Betyg förs över manuellt tills vidare.
