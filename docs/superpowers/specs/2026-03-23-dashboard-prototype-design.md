# Dashboard Prototype: Alessandro Borelli Business Intelligence

## Purpose
Interactive Streamlit dashboard shown on a laptop during a meeting with a non-technical 50+ year old businesswoman. Goal: wow-effect (before/after transformation) + practical tool (her daily work in the future).

## Constraints
- Built in Streamlit (Python)
- Runs locally on laptop, no internet required at meeting
- Dummy data based on real БДДС structure
- 5 screens max, 10-15 minutes of attention
- Zero tech jargon in UI — everything in Russian, simple language
- Visual style: warm tones (gold #cda04c, cream #f9f6f0, dark #1a1a1a) matching AB brand
- Large fonts, readable from 2 meters
- Traffic light colors for status (green/yellow/red)

## Tech Stack
- **Single HTML file** — everything inline, zero dependencies
- **Chart.js** — minified, embedded inline (~200KB)
- **Vanilla JS** — no frameworks, no build step
- **CSS inline** — brand colors, responsive, animations
- **System fonts** — Georgia (matches AB brand), no external font loading
- **Icons** — Unicode symbols + inline SVG
- **Data** — JSON objects directly in JS

### Why single HTML file?
- Partner double-clicks → works. No Python, no terminal, no `npm install`
- Works on laptop (Chrome/Safari) and iPad (AirDrop the file)
- Works offline — zero internet required
- File size: ~300-400KB total
- No server, no localhost, no deploy needed
- Meeting-only tool: shown on laptop/tablet at personal meeting

## Data Source
Dummy data modeled on real structure from:
- `client_data/2025 УПРАВЛЕНКА империя.xlsx` (БДДС 2024 — revenue, expenses, stores)
- `client_data/fwd2024/` (inventory sell-through by brand)
- `additional_research/unit_economics.md` (margins by channel)
- `additional_research/marketplace_analytics.md` (WB/Ozon benchmarks)

---

## Screen 1: Pulse (Before/After Toggle)

### Layout
- Top: toggle switch "Сегодня" / "Через год"
- Below toggle: large revenue number (animated transition)
- Middle: channel cards in a row
- Bottom: monthly bar chart + insight text

### "Сегодня" State

**Big number:** 737M руб./год

**3 channel cards:**
| Card | Value | Color | Note |
|------|-------|-------|------|
| Розница (19 магазинов) | 426M | Blue | 57.8% |
| Опт (по телефону) | 285M | Blue | 38.7% |
| Онлайн (все сайты) | 25.5M | Red | **3.5%** |

**Monthly bar chart:** Jan-Dec from БДДС (43M, 50M, 65M, 54M, 52M, 55M, 68M, 137M, 58M, 59M, 41M, 54M). August highlighted gold.

**Bottom text:** "96 из 100 покупателей в интернете покупают у конкурентов"

### "Через год" State

**Big number:** ~880M руб./год (+19%)

**7 channel cards (new ones appear with animation):**
| Card | Value | Color | Note |
|------|-------|-------|------|
| Розница | 410M | Blue | Stable |
| Опт (цифровой кабинет) | 290M | Blue | +geography |
| alessandroborelli.ru | 30M | Green | **Main profit** |
| imperiya-detstva.ru | 15M | Green | Multi-brand online |
| Маркетплейсы (WB+Ozon) | 12M | Gray | "витрина бренда" subtitle |
| Международные (GCC) | 8M | Gold | UAE flag icon |
| Borelli Family (подписка) | 3M | Gold | New channel! |

**Same monthly chart** + additional line showing online growth trend. School season (Jul-Aug) highlighted gold with label "+37%".

**Bottom text:** "7 каналов продаж. Собственные сайты — главный источник прибыли. Бизнес не зависит от одной торговой точки."

---

## Screen 2: Store X-Ray (Before/After Toggle)

### Layout
- Top: toggle "Сегодня" / "Через год"
- Main: full-width table with all stores
- Bottom: summary row

### "Сегодня" State — Table

| Column | Description |
|--------|-------------|
| Магазин | Store name + city |
| Выручка/мес | Monthly revenue |
| Аренда/мес | Monthly rent (from БДДС) |
| Маржа | Revenue - Rent - Staff estimate |
| Руб/м² | Revenue per sq meter |
| Статус | Traffic light circle (green/yellow/red) |

Data rows (19 stores, from БДДС rent data + estimated revenue):
- Green: Европейский, Океания, Саларис, Афимолл, Вегас, Ривьера, Галерея (SPb)
- Yellow: Сочи, Аэробус, Красный Кит, РИО, Краснодар, Ростов, Иркутск
- Red: **Лубянка** (-0.2M/мес), **Dubai Mall** (-1.1M/мес)

Rows are clickable — expand to show detail card.

**Summary row:** Общая аренда: 124.9M/год | Точек в плюсе: X | В минусе: X

### "Через год" State

Same table but:
- Лубянка: renegotiated to % от оборота → yellow
- Dubai Mall: online channels connected → revenue 0.1M → 1.8M → yellow
- 2 worst-performing stores: shown at bottom in **gray, strikethrough text**, with label "закрыта — убыточна" or "переформатирована в пункт выдачи"
- NEW row at top: "Онлайн-каналы" — zero rent, revenue 80M+ → bright green
- Summary: rent reduced by 15-20M

---

## Screen 3: Customers (Practical)

### Layout
- Top: 4 segment cards in a row
- Middle: "Your child grew up" golden card
- Bottom: live activity feed

### Segment Cards

| Segment | Count | Revenue share | Avg check | Color |
|---------|-------|--------------|-----------|-------|
| VIP (100K+/год) | 847 | 42% | 8,400 | Gold |
| Активные (< 6 мес) | 3,200 | 35% | 4,200 | Green |
| Спящие (6-12 мес) | 5,400 | "Спят с вашими деньгами" | — | Yellow |
| Потерянные (год+) | 8,100 | "Ушли к конкурентам" | — | Red |

### "Your Child Grew Up" Card

Golden border, centered, prominent:
- Icon: baby → child transition
- Text: "1,240 клиентов купили одежду 6+ месяцев назад. Их дети выросли на 1-2 размера. Они готовы к покупке — но им никто не написал."
- Big number: "Потенциал одной рассылки: 6.2M руб."
- Fake button: "Отправить подборку по новому размеру" (not functional, illustrative)

### Activity Feed

Scrolling list of recent "events" (dummy):
```
Сегодня 14:32 — Мария К. купила куртку р.140 (повторная покупка)
Сегодня 11:15 — Анна С. бросила корзину на 7,800 руб. → напоминание отправлено
Вчера 19:40 — Елена Д. открыла рассылку "новый сезон" → перешла на сайт
Вчера 16:05 — Ольга М. не покупала 90 дней → отправлено "мы скучаем" + промокод
```

---

## Screen 4: Inventory — Frozen Money (Practical)

### Layout
- Top: 3 status indicators (green/yellow/red zones)
- Middle: horizontal bar chart by brand
- Bottom: action plan card

### Status Indicators

| Zone | Units | Value | Label | Color |
|------|-------|-------|-------|-------|
| Свежий (до 60 дней) | 4,100 | ~42M | "Продастся сам" | Green |
| Залежался (60-90 дней) | 2,800 | ~28M | "Нужна скидка 20%" | Yellow |
| Мёртвый сток (90+ дней) | 2,486 | ~25M | "Срочная уценка 40-60%" | Red, large font |

### Brand Breakdown (horizontal stacked bars)

Each bar = one brand, split into green/yellow/red portions:
- BORELLI (own brand): longest bar, 75% of inventory, mostly green
- Antony Morato: 687 units stuck (lots of red)
- DIESEL: 283 units
- Bikkembergs: 310 units
- DSQUARED2: 90 units — expensive, bad sell-through
- Elisabetta Franchi: 94 units
- Others grouped

### Action Plan Card

Green background, 3 steps:
1. Flash-sale красная зона (2,486 ед.) → скидка 40-60% → возврат ~10-15M за 2-4 недели
2. Автоуценка жёлтая зона (2,800 ед.) → скидка 20% → ещё ~20M за 4-6 недель
3. Правило на будущее: 60 дней → -20%, 90 дней → -40%, 120 дней → outlet

**Big number at bottom:** "До 30M руб. возвращается в оборот"

---

## Screen 5: Marketing ROI (Practical)

### Layout
- Top: current budget bar vs competitors
- Middle: channel ROI table
- Bottom: reallocation visual

### Budget Comparison (horizontal bars)

```
Вы:        ███                          12.9M (1.5%)
Choupette: ████████████████████         ~50-70M (оценка)
Gulliver:  ██████████████████████████   ~80-100M (оценка)
Норма:     ████████████████             37-74M (5-10%)
```

### Channel ROI Table

| Channel | Spent | Revenue | ROI | Verdict |
|---------|-------|---------|-----|---------|
| Свой сайт (реклама) | 800K | 2.4M | 3.0x | Green "Усилить" |
| Блогеры | 200K | 1.2M | 5.8x | Green++ "Лучший канал" |
| Площадки (внутр.) | 300K | 1.5M | 5.0x | Green++ "Усилить" |
| Соцсети (таргет) | 400K | 1.1M | 2.7x | Green "Норма" |
| Видео (TikTok/Reels) | 150K | 600K | 4.0x | Green "Растёт" |
| СМС рассылки | 290K | 95K | 0.3x | Red strikethrough "Убрать" |
| Фото/продакшн | 420K | — | — | Yellow "Оптимизировать" |

### Reallocation Visual

Simple arrow diagram:
- Left side: "СЕЙЧАС" — current allocation pie
- Right side: "ПРЕДЛОЖЕНИЕ" — reallocated pie
- Arrow between with text: "Тот же бюджет 12.9M → отдача с ~5M до ~12-15M"
- Below: "При бюджете 30M → ожидаемая отдача 80-100M"

---

## Navigation

Sidebar with 5 icons + labels (Russian):
1. Пульс бизнеса
2. Рентген магазинов
3. Клиенты
4. Склад
5. Маркетинг

Top of every screen: "Alessandro Borelli — Панель управления" with brand logo placeholder.

## File Structure

```
dashboard/
└── dashboard.html      # Single file. That's it.
```

Internal structure within the HTML:
- `<style>` — all CSS (brand colors, responsive, animations)
- `<script>` — Chart.js minified (~200KB)
- `<script>` — dummy data as JSON
- `<script>` — app logic (navigation, toggle, charts, interactions)
- `<body>` — 5 screens as `<section>` elements, toggled via JS

## Tablet/Mobile Optimization

**Primary target: laptop** (meeting, partner shows on screen)
**Secondary: iPad** (AirDrop the file, open in Safari)

### Approach
- CSS media queries for tablet breakpoint (768px+)
- Cards stack vertically on tablet (1-2 columns instead of 3-4)
- Charts: Chart.js responsive mode built-in
- Font sizes: minimum 16px on tablet
- Toggle switch: large touch targets (44x44px per Apple HIG)
- Tables: horizontal scroll on narrow screens
- Navigation: bottom tab bar on tablet, sidebar on desktop

---

## Success Criteria

1. Partner opens laptop, launches `streamlit run app.py`
2. Dashboard loads in 2 seconds
3. Partner clicks through 5 screens in 10-15 minutes
4. Elena sees her real store names, real brand names, real structure
5. Before/after toggle creates visible "wow" moment
6. "Your child grew up" card creates emotional reaction
7. Dead stock numbers create urgency
8. Marketing table makes reallocation obvious
9. Everything readable from across the table (large fonts)
10. No English, no tech terms, no loading spinners
