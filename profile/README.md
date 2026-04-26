# GG Rental 🏠

It's just a [sex thing](https://miro.com/app/board/uXjVMqXR_9w=/?moveToWidget=3458764562966748920&cot=14) in in [the cloud](https://console.cloud.yandex.ru/folders/b1geg6hpsbra4fp39d98).


**GG Rental** is an open-source project for apartment search through Telegram.

The idea is simple: instead of opening many rental websites, changing filters manually, and checking maps one by one, the user talks to a Telegram bot, chooses a country, city and price range, and receives relevant apartment listings with locations, prices and map links.

<img width="1200" height="1142" alt="photo_2026-04-26 12 43 09" src="https://github.com/user-attachments/assets/80c6760f-6cb2-4b12-b50a-9f7e6947c477" />
<img width="591" height="1280" alt="photo_2026-04-26 12 43 06" src="https://github.com/user-attachments/assets/2c105265-b9f3-4d71-85ba-62010607b0f8" />


> The screenshots above show the intended user experience: a Telegram-first rental search flow with country/city selection, price filtering, apartment cards and Google Maps previews.

---

## What the project does

GG Rental helps users search for flats in a conversational interface.

A typical flow looks like this:

1. The user opens the Telegram bot.
2. The bot asks for the country.
3. The user selects a city.
4. The user sets a maximum price.
5. The bot returns apartment listings with:
   - address or district,
   - monthly price,
   - listing identifier,
   - map link,
   - short listing preview,
   - source-specific details when available.

The project was designed as a practical relocation assistant for people looking for apartments in new cities, especially across different countries where rental platforms, languages and listing formats differ.

---

## Why Telegram?

Apartment search is often a repetitive task:

- open several websites,
- apply the same filters again and again,
- compare prices,
- check the district on the map,
- come back later to see whether something changed.

Telegram is a good interface for this type of workflow because it is fast, mobile-friendly and naturally supports notifications. GG Rental uses Telegram as the main user interface and keeps the product lightweight: no separate app, no heavy frontend, no complicated onboarding.

---

## Main features

- **Telegram-based apartment search**  
  Search and browse apartments directly from a bot.

- **Country and city selection**  
  The bot supports a multi-country flow and city-level filtering.

- **Price filtering**  
  Users can filter listings by monthly rent.

- **Map integration**  
  Listings include map links or map previews to help users quickly understand the location.

- **Listing aggregation**  
  The project contains services for collecting, parsing and storing apartment data.

- **User-facing services**  
  Separate services handle user preferences, saved filters and bot interactions.

- **Reminder services**  
  The architecture includes services intended to notify users about new apartments or price changes.

- **Price modelling**  
  The organisation contains a dedicated price model repository, intended for estimating or analysing apartment prices.

---

## Organisation structure

The project is split into several repositories. At a high level, the organisation contains the following components:

| Repository | Purpose |
|---|---|
| [`telegram_bot`](https://github.com/gg-rental/telegram_bot) | Main Telegram bot and search interface. |
| [`flats_etl`](https://github.com/gg-rental/flats_etl) | ETL pipeline for collecting and preparing apartment listings. |
| [`link_parsers`](https://github.com/gg-rental/link_parsers) | Parsers for extracting listing data from external links and sources. |
| [`link_gen`](https://github.com/gg-rental/link_gen) | Link generation utilities for supported sources and search flows. |
| [`user_services`](https://github.com/gg-rental/user_services) | Client-side services and cloud-function logic for user operations. |
| [`flats_users`](https://github.com/gg-rental/flats_users) | User-related database declarations and storage layer. |
| [`reminder-services`](https://github.com/gg-rental/reminder-services) | Services for reminders, new-listing alerts and price-change notifications. |
| [`price_model`](https://github.com/gg-rental/price_model) | Apartment price prediction / modelling experiments. |
| [`.github`](https://github.com/gg-rental/.github) | Organisation-level profile and documentation. |

---

## High-level architecture

```text
                  ┌────────────────────┐
                  │    Telegram User    │
                  └─────────┬──────────┘
                            │
                            ▼
                  ┌────────────────────┐
                  │    Telegram Bot     │
                  │ country / city /    │
                  │ price search flow   │
                  └─────────┬──────────┘
                            │
                            ▼
                  ┌────────────────────┐
                  │  User Services      │
                  │ preferences, state, │
                  │ saved filters       │
                  └─────────┬──────────┘
                            │
                            ▼
        ┌────────────────────────────────────────┐
        │             Search Layer                │
        │ filtering, ranking, listing formatting  │
        └─────────┬──────────────────────┬───────┘
                  │                      │
                  ▼                      ▼
        ┌─────────────────┐    ┌─────────────────┐
        │ Listings Storage │    │  Price Model     │
        │ flats / users    │    │ price estimates  │
        └────────┬────────┘    └─────────────────┘
                 │
                 ▼
        ┌─────────────────┐
        │   ETL / Parsers  │
        │ external sources │
        └─────────────────┘
```

---

## Example user flow

```text
User: /search
Bot:  Before we start, choose a country
User: Serbia
Bot:  Choose a city
User: Beograd
Bot:  Choose a maximum price
User: 800
Bot:  Here are matching listings:
      - Krivošijska, Voždovac — 220 EUR/month
      - Srpskih udarnih brigada, Rakovica — 250 EUR/month
      - map links and listing previews included
```

---

## Tech stack

The project is mostly written in **Python** and is organised as a set of small services.

Expected building blocks include:

- Telegram Bot API
- Python services
- ETL pipelines
- SQL-backed user/listing storage
- Cloud functions / service endpoints
- Web parsing utilities
- Google Maps links for location previews
- Optional ML-based price modelling

---

## Status

This project is an early-stage prototype / work-in-progress.

The codebase was built to explore a practical product idea: a Telegram-native apartment search assistant with listing aggregation, filtering, map previews, user preferences and reminder services.

Some repositories may require additional cleanup before production use. The current open-source version should be treated as a technical prototype rather than a fully maintained commercial service.

---

## Possible future improvements

- Add more countries and cities.
- Improve listing deduplication.
- Add better ranking by location, price and listing quality.
- Add saved searches.
- Add Telegram notifications for new listings.
- Add price-drop alerts.
- Add listing freshness checks.
- Add a lightweight web dashboard.
- Improve parser reliability and monitoring.
- Add tests and CI checks across repositories.
- Add Docker-based local development setup.
- Add documentation for deployment and environment variables.
- Improve the price prediction model.

---

## Local development

Each repository may have its own setup instructions. A typical Python service can usually be started with a flow similar to:

```bash
git clone https://github.com/gg-rental/<repository-name>.git
cd <repository-name>
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Some repositories may use Poetry:

```bash
poetry install
```

For the Telegram bot, you will normally need a bot token:

```bash
export TELEGRAM_BOT_TOKEN="your-telegram-bot-token"
```

Then run the relevant service entry point according to the repository-specific structure.

---

## Configuration

Depending on the service, configuration may include:

- Telegram bot token
- database connection string
- supported countries
- supported cities
- parser settings
- external source URLs
- cloud-function credentials
- map-link generation settings
- notification schedule

Do not commit real tokens, credentials or production secrets to the repository.

---

## Contributing

Contributions are welcome.

Good first areas to improve:

- documentation,
- parser cleanup,
- source adapters,
- Telegram bot UX,
- tests,
- deployment instructions,
- Docker setup,
- listing deduplication,
- price model experiments.

A simple contribution flow:

1. Fork the repository you want to improve.
2. Create a feature branch.
3. Make a focused change.
4. Open a pull request with a short explanation.

---

## Disclaimer

GG Rental is an independent open-source project. It is not affiliated with Telegram, Google Maps or any real estate platform.

Rental data may become outdated quickly. Always verify listing details, price, availability and rental terms directly with the property owner, agent or original source before making any decision.

---

## Author

Created by **Andrey Golda** and **Ismail Gadzhiev**.

GitHub: [@a-golda](https://github.com/a-golda), [@oldhroft](https://github.com/oldhroft)

---

## License

Add the applicable license for each repository. If the project is intended to be fully open source, consider adding an MIT, Apache-2.0 or GPL-compatible license depending on the intended usage model.
