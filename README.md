# Stock Prices: Supply and Demand

*A living, self-balancing economy for your world. NeoForge 1.21.1, by barunn.*

Stock Prices is not a shop mod; it is the price engine behind one. Every obtainable item is given a Standard Retail Price (SRP) that is recomputed once per Minecraft day from the live state of the world: how much of the item exists (supply), how concentrated it is into a few hands (hoarding), how much money is circulating (inflation), and how much was bought lately (demand). Prices rise when things get scarce, get hoarded, or when money floods in, and they fall when the world is awash in supply. Players move the market by mining, farming, hoarding in vaults, flooding a good, or cutting off a supply line.

Press a keybind to open the Market Prices screen and read any item's true value, complete with a 7-day trend arrow. It works on its own, and it plugs into a shop when you have one.

## How the market moves

Everything is measured relative to opening day. On the first recompute the engine calibrates its supply and money references so that day-one prices equal the base prices you configured. You see movement only after you change the world (mine, hoard, spend, print money) away from that baseline. One easing rate governs every force, so a rise and a crash behave identically; a big shock still resolves within a day or two.

Four forces set the target price for each commodity:

- **Supply** pulls the price down. Circulate more of a good and it gets cheaper.
- **Scarcity** pushes the price up when effective supply runs low. Cut off a supply line and watch the price climb.
- **Hoarding** is folded into effective supply. A stockpile far above the per-location threshold discounts itself out of circulation, so the price climbs even when the raw total is high. A monopolist dumping their vault causes a hard crash; the very same total spread evenly across many players barely moves the price.
- **Inflation** lifts every price as the total money supply grows. Hoard or print currency and the whole market floats upward.
- **Demand** (buying pressure) pushes prices up. Live purchase tracking arrives through Spud's Shop; until real purchases are seen the demand factor stays neutral.

An item with zero supply clamps at the price ceiling, and all prices are held between a configurable floor and ceiling.

## The Market Prices screen

Open the screen with a keybind (default `\`, the backslash key, rebindable in Controls under the "Stock Prices" category) or with the `/stockprices market` command. Inside you get:

- A live search box to jump straight to any item.
- A scrollable list of every priced commodity.
- Name, Price, and Movers sort modes so you can find the biggest gainers and losers at a glance.
- One honest number per item in the base currency unit (no denomination breakdown), plus a 7-day trend glyph: green up, red down, or a gray flat mark when the change sits inside the negligible band.

## What counts as a commodity

A commodity is any obtainable item: everything except air, spawn eggs, and a short technical blacklist of un-obtainable blocks. Items can be grouped so that reversible packing and cross-mod synonyms price as a single thing (an iron block is worth nine ingots), while one-way processing (ore versus ingot) is deliberately kept separate so a smelting bottleneck actually shows up in the prices. Anything unlisted becomes its own commodity at weight 1.

## Supply scanning

The mod keeps its own per-chunk index of every item held in container inventories. Online players' inventories and ender chests count as distinct locations for the hoarding math, and both are configurable. Scanning is throttled: loaded chunks are queued and only a small budget of chunks is scanned per server tick, so joining a world never stalls the load screen. Only chunks that actually contain containers are queued. At the day rollover the loaded chunks are re-scanned before pricing, and the recompute is held until the catch-up scan has fully settled, so a half-built index can never corrupt good prices.

## Money supply and inflation

The money pool is the sum of two sources:

- **Physical currency:** every currency item in the world, counted at its configured value. Hoarding or printing currency inflates every price.
- **Bank balances (optional):** with SDM Economy installed, every account balance is totalled in and added on top, so stored bank wealth feeds inflation too. Locked balances are skipped.

## Bills and denominations

The screen always shows one honest base-unit number. When a price is written onto a shop stall, though, it is expressed as the fewest of the largest denomination that fits: a value of 1000 becomes 10 diamonds (with a diamond worth 100), rather than 1000 emeralds.

## Optional integrations

Every integration is an optional soft dependency. Nothing is bundled, and each hook is guarded so the mod runs perfectly well on its own.

- **Spud's Shop:** a checkbox in the stall owner screen toggles that stall to SRP mode. When it is on, the daily recompute writes the computed price (as denominated bills) straight onto the stall, and the toggle state persists. This is also how live demand tracking reaches the market.
- **SDM Economy:** account balances are totalled into the money pool, feeding inflation through the bank. On any error it falls back to a neutral value.
- **SDM Shop:** listed as an optional dependency in the mod metadata.

## Commands

| Command | Who can use it | What it does |
|---|---|---|
| `/stockprices market` | All players | Open the Market Prices screen. |
| `/stockprices info` | All players | Inspect the item in your main hand: weighted supply, hoarding level, effective supply, baseline reference, base price, current SRP, and the economy money pool. |
| `/stockprices recompute` | Operators | Re-scan loaded chunks and recompute the whole market immediately. |
| `/stockprices reload` | Operators | Rebuild the commodity registry from the config. |

## Configuration

Everything is tuned from a single server-authoritative config file, `config/stockprices-common.toml`. Server owners can set base prices per item, define commodity groups, choose currency denominations, adjust how hard each force pulls, tune the trend window, and dial in scanning performance. Highlights include:

- **Base prices:** a neutral-market price for every commodity, with per-item overrides.
- **Formula tuning:** sensitivity dials for scarcity, demand, money, and hoarding, plus the daily adjustment speed and the price floor and ceiling.
- **Commodity control:** grouping, force-include and force-exclude lists, and toggles for counting player inventories and ender chests toward supply.
- **Currency:** the base unit name, the two-way denomination table (used both for bills and for the money pool), and which bank currencies to total in.
- **Trend:** how many days the trend arrow compares against, and how small a change is treated as flat.

## Supported Versions

| Loader | Minecraft |
|---|---|
| NeoForge | 1.21.1 |

## Available on CurseForge

Find Stock Prices: Supply and Demand and the rest of barunn's mods on CurseForge:

https://www.curseforge.com/members/barunn/projects
