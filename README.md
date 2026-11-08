# Challenge Deel 4 — TikTok demo met reacties

Een TikTok-demo in Vue 3 (Vite), uitgebreid met een `Chat`-component voor reacties.

## Functies
- Verticale feed met video's die je per scherm doorscrolt (`v-for` + scroll-snap).
- Like-knop met teller per video.
- Reacties in `src/components/Chat.vue`:
  - alle berichten ophalen bij het laden van de component (`onMounted`, GET);
  - een nieuw bericht versturen (POST) en het meteen bovenaan zetten;
  - de berichten zitten in een `reactive()`-array, dus de lijst past zich aan zonder de pagina te herladen.

## API
`https://lab5-p379.onrender.com/api/v1/messages/` — elk bericht heeft een `user` en een `text`.
De server slaapt als hij een tijdje niet gebruikt wordt. Het eerste laden kan dan tot een minuut duren.

## Starten
```bash
npm install
npm run dev
```

De video's zijn CC0-voorbeeldvideo's van MDN.
