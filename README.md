# Driftopolis

3D závodní hra do prohlížeče, inspirovaná Ultimate Car Driving Simulator. Žádné peníze, žádné odemykání: všechna auta, díly i úpravy jsou dostupné hned. Jen si vybereš auto, naladíš ho v garáži a jedeš. Rozhraní je česky.

Hra je jediný soubor (`index.html`), běží na Three.js a nepotřebuje instalaci ani build.

## Co je uvnitř

- **11 aut** s vlastními rozměry, siluetami a interiérem: Nissan Skyline GT-R (R34), Toyota Supra (A80), Mazda MX-5 (NA), Honda Civic Type R, VW Golf GTI, BMW M3 (E46), Ford Mustang GT, Porsche 911 GT3, Lamborghini Huracán, Toyota Land Cruiser a Ford F-150 Raptor.
- **Kamery**: za autem, z kokpitu (vidíš palubní desku, volant a budíky) a z kapoty.
- **Garáž**: výkon (motor, turbo/kompresor, převodovka, brzdy, náhon FWD/RWD/AWD), podvozek (výška, sklon kol, tuhost tlumičů) a kola (design disků, velikost, šířka a profil pneumatik), barva laku.
- **Světy**: uzavřené město s mostem a hustým provozem, dálnice a písečná poušť. Mezi světy se přejíždí sjezdy a branami, jízda se přitom nepřerušuje.
- **Den a noc** ve městě.
- **Multiplayer**: společný otevřený svět a závody (městský okruh a dálniční sprint) se šipkami a zábranami, které ukazují trasu.
- **Fyzika** bez věčného driftu: reálné pružení, přenos váhy a grip pneumatik.

## Ovládání

| Akce | Klávesnice | Mobil |
|---|---|---|
| Plyn / brzda | `W` `S` nebo šipky nahoru/dolů | tlačítka na obrazovce |
| Zatáčení | `A` `D` nebo šipky vlevo/vpravo | tlačítka na obrazovce |
| Ruční brzda | `Mezerník` | tlačítko na obrazovce |
| Kamera | `C` | tlačítko Kamera |
| Postavit auto | `R` | tlačítko Postavit |
| Menu | `Esc` nebo `M` | tlačítko Menu |
| Garáž | `G` | Menu → Garáž |
| Den / noc | `T` | Menu → Svět |

Typ ovládání (klávesnice / dotyk) se přepíná v menu.

## Spuštění

Stačí otevřít `index.html` v prohlížeči. Potřebuje WebGL (každý moderní prohlížeč). Pro telefon je hra odlehčená, ale na starších telefonech může být výkon nižší.

Lokálně přes server (volitelné):

```bash
npx serve .
```

## Multiplayer

Hra podporuje dva způsoby, jak propojit hráče:

1. **Uvnitř Claude** (jako artifact): funguje sám, nic se nenastavuje.
2. **Vlastní server** (pro hraní mimo Claude): potřebuješ malý WebSocket server ze složky `server/` a adresu zapsat do `index.html`.

### Nasazení serveru (Render, zdarma)

1. Složku `server/` (`server.js`, `package.json`) nahraj do vlastního repozitáře na GitHubu.
2. Na [render.com](https://render.com) vytvoř **New → Web Service**, vyber repozitář, **Language: Node**, Build Command `npm install`, Start Command `npm start`, Instance Type **Free**.
3. Po nasazení dostaneš adresu typu `https://nazev.onrender.com`.

### Propojení hry

V `index.html` najdi řádek

```html
<script>window.G3D_SERVER = "";</script>
```

a vepiš adresu serveru s `wss://`:

```html
<script>window.G3D_SERVER = "wss://nazev.onrender.com";</script>
```

Pak hru nahraj na GitHub Pages (nebo jiný statický hosting) a v menu v záložce **Online** zapni multiplayer.

Poznámky:

- Zdarma plán Renderu po cca 15 minutách bez provozu usne, první připojení pak trvá asi minutu.
- Každý svět (Město, Dálnice, Poušť) je samostatná místnost, v jedné je maximálně 24 hráčů.
- Během závodu jsou přejezdy mezi světy vypnuté.

## Struktura projektu

```
index.html          hra (všechny skripty vložené do jednoho souboru)
js/data.js          auta, díly, rozměry
js/vehicle.js       fyzika auta
js/carmodel.js      3D modely aut a interiéry
js/world_*.js       město, dálnice, poušť, garáž, provoz
js/multi.js         multiplayer a závody
js/wsroom.js        připojení na vlastní WebSocket server
js/ui.js, main.js   rozhraní, ovládání, hlavní smyčka
server/             multiplayer server (Node.js + ws)
```
