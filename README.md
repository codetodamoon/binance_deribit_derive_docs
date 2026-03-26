# Binance / Deribit / Derive Markdown Documentation Hub

> Looking for the Chinese version? See `README_ZH.md`.

## Overview
This repository aggregates the official documentation of Binance, Deribit, and Derive into clean Markdown files for offline access, full-text search, and custom referencing—ideal for quant developers and risk teams who need instant exchange API insights.

## Why It’s Attractive
- **Offline-ready**: Clone once and read anywhere—perfect for limited connectivity or local archiving.
- **Structured layout**: Each exchange keeps a tidy directory so API specs, params, and samples are easy to locate.
- **Fast search**: With editors or tools like `rg`, jump to any field, error code, or risk note instantly.
- **Up-to-date cadence**: Files mirror the official sources; pull updates anytime to stay aligned with upstream changes.

## Repository Structure
- `binance/`: Binance spot/derivatives REST, market streams, and user data stream guides.
- `deribit/`: Deribit perpetuals/options APIs, authentication workflows, and error references.
- `derive/`: Markdown port of Derive’s official docs covering trading, account, and more.

## How to Use
1. **Clone the repo**  
   `git clone https://github.com/<your-account>/<repo>.git`
2. **Explore subfolders**  
   Dive into `binance/`, `deribit/`, or `derive/` and open the Markdown files with your preferred viewer or IDE.
3. **Search quickly**  
   Use commands like `rg "keyword" -n binance` or your IDE’s global search for instant lookups.
4. **Stay updated**  
   Run `git pull` periodically; submit PRs whenever the exchanges update their docs.

## Contributing
- Spot mistakes or missing sections? File an issue or open a PR.
- Reference the source section and update date so everyone knows it matches the official text.

## License & Credits
All materials originate from the official public documentation of Binance, Deribit, and Derive; copyrights remain with the respective teams. This repo exists solely to simplify reference and learning—please respect each exchange’s terms of service.

---

If fast access to exchange docs helps your workflow, give it a Star/Fork and share it with fellow builders!
