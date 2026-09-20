# Neonspeelhal

Een speelhal-website met elf browsergames. Alles draait in de browser: geen server,
geen database, geen account. Twee dingen zijn genoeg:

```
index.html          de speelhal zelf, met tien ingebouwde games
games/hexdrift.html Hexdrift, die als eigen pagina in een frame draait
```

## Lokaal bekijken

Dubbelklikken op `index.html` werkt voor de meeste games, maar Hexdrift heeft een
kleine webserver nodig (browsers mogen geen frames laden van `file://`):

```bash
python3 -m http.server 8000
```

Open daarna http://localhost:8000 in je browser.

## Online zetten

Kies er één — alle drie zijn gratis en je hoeft niets te installeren behalve waar
het staat.

### 1. Netlify Drop (snelst, geen account nodig om te testen)

1. Ga naar https://app.netlify.com/drop
2. Sleep de hele map (met `index.html` en de map `games/`) in het vak.
3. Je krijgt meteen een adres zoals `https://willekeurige-naam.netlify.app`.
4. Maak een gratis account aan als je de site wilt houden en een eigen naam wilt.

### 2. GitHub Pages (handig als je vaker wilt aanpassen)

1. Maak een repository aan op github.com, bijvoorbeeld `speelhal`.
2. Upload `index.html` en de map `games/` (Add file → Upload files).
3. Ga naar Settings → Pages.
4. Bij "Source" kies je `Deploy from a branch`, branch `main`, map `/ (root)`.
5. Na een minuut staat de site op `https://<jouwnaam>.github.io/speelhal/`.

### 3. Vercel

1. Installeer de CLI: `npm i -g vercel`
2. Draai `vercel` in deze map en volg de vragen.

## Eigen domein

Bij alle drie kun je een eigen domeinnaam koppelen (bijvoorbeeld via TransIP of
Namecheap, zo'n tien euro per jaar). In de instellingen van de hostingdienst staat
welke DNS-regels je bij je domeinprovider moet invullen.

## Een game toevoegen

Ingebouwde game: voeg onderaan `index.html` een blokje toe aan de lijst `GAMES`:

```js
GAMES.push({
  id:'mijngame',
  title:'Mijn Game',
  sub:'Korte omschrijving.',
  tags:['Arcade'],
  color:'#FFC63D',
  keys:['WASD rijden','Klik vuren'],
  thumb:function(c,w,h,t){ /* bewegend voorbeeldplaatje */ },
  create:function(api){
    return {
      update:function(dt){ /* spellogica */ },
      draw:function(){ api.clear('#070613'); /* tekenen */ }
    };
  }
});
```

Losse pagina als game: zet het HTML-bestand in `games/` en geef het blokje
`src:'games/mijngame.html'` in plaats van `create`.

Records en het aantal keer gespeeld worden per bezoeker in de browser bewaard
(localStorage) — er gaat niets naar een server.
