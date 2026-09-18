<div align="center">

# VELO STOR

**An e-commerce storefront for bikes, e-bikes and scooters — connected to a WhatsApp AI assistant for conversational shopping.**

[**🌐 Live Demo**](https://velo-stor.netlify.app/) · [**🤖 WhatsApp Bot**](https://github.com/El-Tousy/Meta-API-python-whatsapp-bot) · [**📸 Screenshots**](#screenshots)

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Netlify](https://img.shields.io/badge/Deployed_on-Netlify-00C7B7?style=flat&logo=netlify&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

[🇬🇧 English](README.md) · [🇩🇪 Deutsch](README.de.md)

</div>

<!-- TODO: replace with a 10-20s GIF of someone browsing the catalogue and opening a product page.
     Record with Kap (macOS) or ScreenToGif (Windows), save to docs/demo.gif -->
![VELO STOR demo](docs/demo.gif)

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Screenshots](#screenshots)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Deployment](#deployment)
- [Technical Challenges and Learnings](#technical-challenges-and-learnings)
- [Roadmap](#roadmap)
- [Author](#author)
- [License](#license)

---

## Overview

VELO STOR is a multi-page online store for bicycles, electric bikes and scooters, built from scratch with vanilla HTML, CSS and JavaScript — no framework, no build step.

The project has two goals. The first is to build a complete storefront by hand: catalogue, category filtering, product detail pages and an admin view, without relying on a template or a CMS. The second is to connect that storefront to a [WhatsApp bot](https://github.com/El-Tousy/Meta-API-python-whatsapp-bot) built with the Meta Cloud API, so a customer can browse the same catalogue through a conversation instead of a web page.

Together the two repositories form one system: a web storefront and a conversational front-end sharing the same product data.

> **Note on the catalogue.** The products and prices are sample data used to test the store's behaviour. This is a technical project, not a live commercial site.

---

## Features

### Storefront
- **Category catalogue** — products split across mountain bikes, electric bikes and scooters
- **Product detail pages** — dedicated page per model with specifications and images
- **Responsive layout** — usable from mobile through desktop
- **Static informational pages** — About, Contact, Privacy Policy and Terms

### Admin
- **Product management view** — catalogue overview from an administrative interface
- **Control panel** — <!-- TODO: describe in one line what control.html actually does -->

### WhatsApp integration
- **Conversational browsing** — customers explore the catalogue through WhatsApp messages
- **Automated replies** — product information served by the bot without human intervention
- **Shared catalogue** — the bot answers from the same product set shown on the site

---

## Screenshots

| Homepage | Catalogue |
|---|---|
| ![Homepage](images/screenshots/screenshot1.png) | ![Catalogue](images/screenshots/screenshot2.png) |

| Product detail | About |
|---|---|
| ![Product detail](images/screenshots/screenshot3.png) | ![About](images/screenshots/screenshot4.png) |

| Contact |
|---|
| ![Contact](images/screenshots/screenshot5.png) |

[View all screenshots →](images/screenshots)

---

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Markup | HTML5 | Semantic, one page per product and per category |
| Styling | CSS3 | Custom stylesheet — no framework, to keep full control over the layout |
| Behaviour | Vanilla JavaScript | Navigation, filtering and interactions without a build step |
| Hosting | Netlify | Continuous deployment straight from the `main` branch |
| Conversational layer | Python, Flask, Meta WhatsApp Cloud API | See the [bot repository](https://github.com/El-Tousy/Meta-API-python-whatsapp-bot) |
| Versioning | Git & GitHub | — |

---

## Architecture

```mermaid
flowchart LR
    A[Customer] --> B[VELO STOR website<br/>HTML / CSS / JS<br/>Netlify]
    A --> C[WhatsApp]
    C --> D[Meta Cloud API]
    D --> E[Flask webhook<br/>Python]
    E --> F[OpenAI API]
    E --> G[(Product catalogue)]
    B --> G
    F --> E
    E --> D
    D --> C
```

The website is fully static: every page is served as-is, with no server-side rendering. The conversational path runs through a separate Flask service that receives Meta webhooks, enriches the reply with OpenAI, and answers from the same catalogue the site displays.

---

## Getting Started

The site is static, so there is nothing to build and no dependencies to install.

### Prerequisites

- Any modern browser
- Git
- Optionally, Node.js 18+ if you want to serve the site over HTTP rather than `file://`

### Installation

```bash
git clone https://github.com/El-Tousy/VELO-STOR-Online-Store.git
cd VELO-STOR-Online-Store
```

### Running locally

Open `index.html` directly in your browser, or serve the folder over HTTP:

```bash
npx serve .
# then open http://localhost:3000
```

Serving over HTTP is recommended: relative paths and any `fetch` calls behave differently under the `file://` protocol.

### Connecting the WhatsApp bot

The bot lives in its own repository and runs independently. Follow the setup instructions in [Meta-API-python-whatsapp-bot](https://github.com/El-Tousy/Meta-API-python-whatsapp-bot) — the storefront needs no configuration on its side.

---

## Project Structure

```
VELO-STOR-Online-Store/
│
├── index.html                  # Homepage
│
├── pages/
│   ├── products.html           # Full catalogue
│   ├── products_vtt.html       # Category — mountain bikes
│   ├── products_electrique.html# Category — electric bikes
│   ├── products_trotinette.html# Category — scooters
│   │
│   ├── ciclista.html           # Product — Ciclista
│   ├── sport_bike.html         # Product — Sport Bike
│   ├── shine_s.html            # Product — Shine S
│   ├── tank-m41.html           # Product — Tank M41
│   ├── dualtron-togo.html      # Product — Dualtron Togo
│   │
│   ├── admin.html              # Admin view
│   ├── control.html            # Control panel
│   │
│   ├── About.html
│   ├── contact.html
│   ├── privacy.html
│   └── terms.html
│
├── assets/
│   ├── css/
│   ├── js/
│   └── images/
│
├── docs/
│   ├── demo.gif
│   └── screenshots/
│
├── LICENSE
└── README.md
```

> This is the target structure. The repository currently keeps every page at the root — see [Roadmap](#roadmap).

---

## Deployment

The site is deployed on Netlify with continuous deployment: every push to `main` triggers a new build.

| Setting | Value |
|---|---|
| Build command | *(none — static site)* |
| Publish directory | `.` |
| Production URL | https://velo-stor.netlify.app/ |

---

## Technical Challenges and Learnings

- **Consistency across a multi-page static site.** With no framework and no templating engine, the header, footer and navigation are duplicated in every file. Keeping them synchronised by hand taught me concretely why component-based frameworks exist — and what problem they actually solve.

- **Structuring a catalogue without a database.** Products are represented as static pages rather than records. This works for a small catalogue but does not scale: adding a product means creating a file. The next iteration moves the catalogue into a JSON file consumed by JavaScript.

- **Sharing one catalogue across two interfaces.** The website and the WhatsApp bot must describe the same products. Keeping both in sync showed why a single source of truth matters more than either interface on its own.

- **Responsive design from scratch.** Building the breakpoints by hand, without a utility framework, forced me to understand flexbox and grid properly instead of composing prebuilt classes.

---

## Roadmap

- [x] Static storefront with category pages and product detail pages
- [x] Netlify deployment
- [x] WhatsApp bot integration
- [ ] Move the catalogue into a single `products.json` consumed by JavaScript
- [ ] Reorganise the repository into `pages/` and `assets/`
- [ ] Shopping cart with `localStorage`
- [ ] Client-side search and filtering
- [ ] Accessibility pass (alt text, keyboard navigation, contrast)
- [ ] Lighthouse audit and performance budget

---

## Author

**Leila El-Tousy** — Computer Science student, Morocco

[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/El-Tousy)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:leilaeltousy@gmail.com)

**Related project:** [WhatsApp AI Bot](https://github.com/El-Tousy/Meta-API-python-whatsapp-bot) — the conversational front-end for this store.

---

## License

Released under the MIT License. See [LICENSE](LICENSE) for details.

---

<div align="center">

⭐ If this project is useful to you, consider leaving a star.

</div>
