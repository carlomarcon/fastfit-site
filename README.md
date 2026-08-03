# Sito istituzionale CG Innovation Srl / FastFit

Sito statico per `https://fastfitcg.net`. Nessun backend, nessuna build, nessuna dipendenza: solo file HTML/SVG/XML/TXT pronti per la pubblicazione.

## Struttura dei file

```
fastfit-site/
├── index.html            → homepage (https://fastfitcg.net/)
├── privacy/
│   └── index.html        → privacy policy (https://fastfitcg.net/privacy)
├── 404.html              → pagina non trovata
├── favicon.svg           → favicon PROVVISORIA (wordmark testuale "F")
├── robots.txt
├── sitemap.xml
└── README.md
```

## Avvio in locale

Da questa cartella:

```bash
python3 -m http.server 8080
```

Poi apri `http://localhost:8080` e `http://localhost:8080/privacy/`.

## Pubblicazione consigliata: Cloudflare Pages

1. Crea un repository Git con questi file (root del repo = questa cartella) e pushalo su GitHub.
2. Su Cloudflare: **Workers & Pages → Create → Pages → Connect to Git**, seleziona il repo.
3. Build settings: framework **None**, build command **vuoto**, output directory **/** (root).
4. Deploy. Il sito sarà subito online su `*.pages.dev` con HTTPS.

Alternative equivalenti (stessa logica "cartella statica, nessuna build"): Netlify, Vercel, GitHub Pages.

## Collegare il dominio fastfitcg.net

Con Cloudflare Pages:

1. Nel progetto Pages: **Custom domains → Set up a custom domain** → `fastfitcg.net`.
2. Se il DNS del dominio è già su Cloudflare, il record viene creato in automatico; altrimenti aggiungi il record CNAME indicato presso il tuo registrar.
3. Aggiungi anche `www.fastfitcg.net` come secondo custom domain, poi crea una **Redirect Rule** (o Bulk Redirect) da `www.fastfitcg.net/*` a `https://fastfitcg.net/$1` (301).
4. Il certificato HTTPS viene emesso automaticamente; il redirect HTTP→HTTPS è attivo di default (verifica in **SSL/TLS → Edge Certificates → Always Use HTTPS**).

Header di sicurezza consigliati (opzionali, via file `_headers` su Pages/Netlify):

```
/*
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
```

## Cookie, servizi esterni e richieste di rete

- **Nessun cookie** impostato dal sito.
- **Nessuna richiesta a terze parti**: niente font esterni (solo font di sistema), niente analytics, pixel, mappe, video, form, reCAPTCHA.
- L'unica interazione esterna è il link `mailto:info@fastfitapp.net`.
- Nota: l'hosting scelto (es. Cloudflare) può impostare cookie tecnici propri o registrare log di sicurezza. La sezione 10 della privacy policy copre già i soli cookie tecnici; verifica il comportamento reale dell'hosting dopo il deploy prima di dichiarare altro.

## Asset mancanti (da sostituire)

- **Logo ufficiale FastFit**: al momento è usato un wordmark testuale nell'header (`.wordmark` in `index.html`, `privacy/index.html`, `404.html`). Quando il logo è pronto, sostituisci il tag `<a class="wordmark">` con un `<img>` con `alt="FastFit"`.
- **Favicon ufficiale**: `favicon.svg` è un segnaposto testuale ("F" su verde bosco). Sostituiscila con la versione ufficiale mantenendo lo stesso nome file (o aggiorna i `<link rel="icon">`).
- **Immagine Open Graph** (`og:image`): non inclusa perché non esiste un asset ufficiale; quando disponibile, aggiungi `<meta property="og:image" content="https://fastfitcg.net/og.png">` alle due pagine.

## Checklist post-deploy

- [ ] `https://fastfitcg.net` raggiungibile con certificato valido
- [ ] `https://fastfitcg.net/privacy` funzionante
- [ ] Redirect `www` → apex e HTTP → HTTPS attivi
- [ ] `robots.txt` e `sitemap.xml` raggiungibili
- [ ] Nessun errore in console (verificato in locale: zero JS, zero richieste esterne)
- [ ] Email `info@fastfitapp.net` attiva e cliccabile
