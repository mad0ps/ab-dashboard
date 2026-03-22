# Dashboard Prototype Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file HTML dashboard (`dashboard.html`) that shows Alessandro Borelli's business data in 5 interactive screens — for a meeting with the client on Monday.

**Architecture:** Single HTML file with inline CSS, Chart.js (embedded), and vanilla JS. No server, no build step, no dependencies. Double-click to open. 5 screens navigated via sidebar/tabs, 2 screens have before/after toggle.

**Tech Stack:** HTML5, CSS3, Chart.js (inline), vanilla JavaScript, system fonts (Georgia)

**Spec:** `docs/superpowers/specs/2026-03-23-dashboard-prototype-design.md`

---

### Task 1: Scaffold — HTML skeleton + CSS + navigation

**Files:**
- Create: `dashboard/dashboard.html`

- [ ] **Step 1: Create HTML skeleton with brand styling**

Create `dashboard/dashboard.html` with:
- DOCTYPE, meta charset, viewport for responsive
- `<style>` block with AB brand colors: gold `#cda04c`, cream `#f9f6f0`, dark `#1a1a1a`, green `#27ae60`, yellow `#f39c12`, red `#e74c3c`, gray `#95a5a6`
- System font stack: `Georgia, 'Times New Roman', serif` for headers, `-apple-system, Arial, sans-serif` for body
- CSS grid/flexbox layout: sidebar (240px) + main content area
- Media query for tablet (<1024px): sidebar collapses to bottom tab bar
- Base font size 18px (readable from 2m), headings 28-48px
- CSS transitions for smooth animations (0.3s ease)
- 5 `<section>` elements (one per screen), all hidden except active
- Sidebar with 5 nav items: icons (Unicode) + Russian labels
- Top bar: "Alessandro Borelli — Панель управления"

- [ ] **Step 2: Add navigation JS**

Add `<script>` at bottom with:
- `showScreen(screenId)` function — hides all sections, shows selected, updates active nav item
- Click handlers on all nav items
- Default: show screen 1 on load
- Smooth fade transition between screens (CSS opacity + JS classList)

- [ ] **Step 3: Verify skeleton works**

Open `dashboard.html` in browser. Verify:
- 5 nav items visible in sidebar
- Clicking each nav item shows corresponding empty section
- Brand colors applied
- Responsive: resize to tablet width, verify bottom tab bar appears
- No console errors

- [ ] **Step 4: Add toggle component for screens 1 and 2**

Add reusable toggle switch CSS + JS:
- CSS: styled toggle (pill shape, gold active color, smooth slide)
- Large click target (min 44px height for touch)
- JS: `toggleState` variable per screen, `onToggle(screenId)` callback
- Labels: "Сегодня" / "Через год"

---

### Task 2: Dummy data module

**Files:**
- Modify: `dashboard/dashboard.html` (add `<script>` block)

- [ ] **Step 1: Add all dummy data as JS objects**

Add `<script>` block before app logic with all data:

```javascript
const DATA = {
  // Screen 1: Pulse
  revenue: {
    total: 737,
    channels: {
      today: [
        { name: 'Розница (19 магазинов)', value: 426, color: '#3498db', pct: '57.8%' },
        { name: 'Опт (по телефону)', value: 285, color: '#2980b9', pct: '38.7%' },
        { name: 'Онлайн (все сайты)', value: 25.5, color: '#e74c3c', pct: '3.5%' }
      ],
      future: [
        { name: 'Розница', value: 410, color: '#3498db', pct: '46.6%' },
        { name: 'Опт (цифровой кабинет)', value: 290, color: '#2980b9', pct: '33.0%' },
        { name: 'alessandroborelli.ru', value: 30, color: '#27ae60', pct: '3.4%', badge: 'Главный канал прибыли' },
        { name: 'imperiya-detstva.ru', value: 15, color: '#2ecc71', pct: '1.7%' },
        { name: 'Маркетплейсы (WB+Ozon)', value: 12, color: '#95a5a6', pct: '1.4%', badge: 'витрина бренда' },
        { name: 'Международные (GCC)', value: 8, color: '#cda04c', pct: '0.9%', flag: '🇦🇪' },
        { name: 'Borelli Family', value: 3, color: '#cda04c', pct: '0.3%', badge: 'подписка' }
      ]
    },
    futureTotal: 880,
    monthly: [43, 50, 65, 54, 52, 55, 68, 137, 58, 59, 41, 54],
    monthLabels: ['Янв','Фев','Мар','Апр','Май','Июн','Июл','Авг','Сен','Окт','Ноя','Дек']
  },

  // Screen 2: Stores
  stores: {
    today: [
      { name: 'ТРЦ Европейский', city: 'Москва', revenue: 5.2, rent: 1.84, margin: 1.1, sqm: 34, status: 'green' },
      { name: 'ТРЦ Океания', city: 'Москва', revenue: 2.9, rent: 0.72, margin: 0.8, sqm: 19, status: 'green' },
      { name: 'ТРЦ Саларис', city: 'Москва', revenue: 2.5, rent: 0.40, margin: 0.7, sqm: 17, status: 'green' },
      { name: 'ТРЦ Афимолл', city: 'Москва', revenue: 2.8, rent: 0.46, margin: 0.9, sqm: 22, status: 'green' },
      { name: 'ТРК Вегас', city: 'Москва', revenue: 2.2, rent: 0.43, margin: 0.5, sqm: 15, status: 'green' },
      { name: 'ТРЦ Ривьера', city: 'Москва', revenue: 1.8, rent: 0.15, margin: 0.5, sqm: 12, status: 'green' },
      { name: 'ТРЦ Галерея', city: 'СПб', revenue: 2.4, rent: 0.55, margin: 0.6, sqm: 16, status: 'green' },
      { name: 'МЦ Красная Площадь', city: 'Сочи', revenue: 2.1, rent: 0.77, margin: 0.4, sqm: 14, status: 'yellow' },
      { name: 'ТЦ Аэробус', city: 'Москва', revenue: 0.8, rent: 0.17, margin: 0.1, sqm: 5, status: 'yellow' },
      { name: 'ТЦ Красный Кит', city: 'Мытищи', revenue: 1.2, rent: 0.34, margin: 0.2, sqm: 8, status: 'yellow' },
      { name: 'ТРЦ РИО', city: 'Москва', revenue: 1.0, rent: 0.30, margin: 0.1, sqm: 7, status: 'yellow' },
      { name: 'МЦ Красная Площадь', city: 'Краснодар', revenue: 1.5, rent: 0.45, margin: 0.3, sqm: 10, status: 'yellow' },
      { name: 'ТЦ', city: 'Ростов-на-Дону', revenue: 1.1, rent: 0.35, margin: 0.15, sqm: 7, status: 'yellow' },
      { name: 'ТЦ', city: 'Иркутск', revenue: 0.9, rent: 0.30, margin: 0.1, sqm: 6, status: 'yellow' },
      { name: 'Аутлет Внуково', city: 'Москва', revenue: 1.3, rent: 0.41, margin: 0.2, sqm: 9, status: 'yellow' },
      { name: 'Аутлет Белая Дача', city: 'Москва', revenue: 1.1, rent: 0.35, margin: 0.15, sqm: 7, status: 'yellow' },
      { name: 'ЦДМ Лубянка', city: 'Москва', revenue: 3.8, rent: 1.87, margin: -0.2, sqm: 25, status: 'red' },
      { name: 'Бутик', city: 'СПб', revenue: 0.7, rent: 0.25, margin: 0.05, sqm: 5, status: 'yellow' },
      { name: 'Dubai Mall', city: 'Дубай', revenue: 0.1, rent: 1.15, margin: -1.1, sqm: 2, status: 'red' }
    ],
    closed: [
      { name: 'ТЦ Аэробус', city: 'Москва', reason: 'закрыта — убыточна' },
      { name: 'ТЦ', city: 'Иркутск', reason: 'переформатирована в пункт выдачи' }
    ],
    totalRent: 124.9
  },

  // Screen 3: Customers
  customers: {
    segments: [
      { name: 'VIP', desc: '100K+ в год', count: 847, share: '42%', check: '8 400', color: '#cda04c' },
      { name: 'Активные', desc: 'покупка < 6 мес', count: 3200, share: '35%', check: '4 200', color: '#27ae60' },
      { name: 'Спящие', desc: '6-12 мес без покупки', count: 5400, share: '—', check: 'Спят с вашими деньгами', color: '#f39c12' },
      { name: 'Потерянные', desc: 'год+ без покупки', count: 8100, share: '—', check: 'Ушли к конкурентам', color: '#e74c3c' }
    ],
    grownUp: { count: 1240, potential: '6.2M' },
    feed: [
      { time: 'Сегодня 14:32', text: 'Мария К. купила куртку р.140', tag: 'Повторная покупка', color: '#27ae60' },
      { time: 'Сегодня 11:15', text: 'Анна С. бросила корзину на 7 800 ₽', tag: 'Напоминание отправлено', color: '#f39c12' },
      { time: 'Вчера 19:40', text: 'Елена Д. открыла рассылку «новый сезон»', tag: 'Перешла на сайт', color: '#27ae60' },
      { time: 'Вчера 16:05', text: 'Ольга М. не покупала 90 дней', tag: 'Отправлено «мы скучаем» + промокод', color: '#f39c12' },
      { time: 'Вчера 10:20', text: 'Татьяна В. оформила возврат р.128', tag: 'Предложен обмен на р.134', color: '#3498db' }
    ]
  },

  // Screen 4: Inventory
  inventory: {
    zones: [
      { name: 'Свежий товар', desc: 'до 60 дней', units: 4100, value: 42, label: 'Продастся сам', color: '#27ae60' },
      { name: 'Залежался', desc: '60-90 дней', units: 2800, value: 28, label: 'Нужна скидка 20%', color: '#f39c12' },
      { name: 'Мёртвый сток', desc: '90+ дней', units: 2486, value: 25, label: 'Срочная уценка 40-60%', color: '#e74c3c' }
    ],
    brands: [
      { name: 'BORELLI', green: 14000, yellow: 3000, red: 1989 },
      { name: 'Antony Morato', green: 800, yellow: 600, red: 687 },
      { name: 'Bikkembergs', green: 500, yellow: 400, red: 310 },
      { name: 'DIESEL', green: 600, yellow: 350, red: 283 },
      { name: 'DSQUARED2', green: 100, yellow: 50, red: 90 },
      { name: 'Elisabetta Franchi', green: 50, yellow: 40, red: 94 },
      { name: 'Прочие', green: 1500, yellow: 800, red: 500 }
    ],
    totalFrozen: 95
  },

  // Screen 5: Marketing
  marketing: {
    totalBudget: 12.9,
    competitors: [
      { name: 'Вы', value: 12.9, pct: '1.5%' },
      { name: 'Choupette', value: 60, pct: '~8%', estimated: true },
      { name: 'Gulliver', value: 90, pct: '~5%', estimated: true },
      { name: 'Норма рынка', value: 55, pct: '5-10%', isNorm: true }
    ],
    channels: [
      { name: 'Свой сайт (реклама)', spent: 800, revenue: 2400, roi: 3.0, verdict: 'Усилить', color: '#27ae60' },
      { name: 'Блогеры', spent: 200, revenue: 1200, roi: 5.8, verdict: 'Лучший канал', color: '#27ae60' },
      { name: 'Площадки (внутр.)', spent: 300, revenue: 1500, roi: 5.0, verdict: 'Усилить', color: '#27ae60' },
      { name: 'Соцсети (таргет)', spent: 400, revenue: 1100, roi: 2.7, verdict: 'Норма', color: '#27ae60' },
      { name: 'Видео (Reels)', spent: 150, revenue: 600, roi: 4.0, verdict: 'Растёт', color: '#27ae60' },
      { name: 'СМС рассылки', spent: 290, revenue: 95, roi: 0.3, verdict: 'Убрать', color: '#e74c3c' },
      { name: 'Фото/продакшн', spent: 420, revenue: 0, roi: null, verdict: 'Оптимизировать', color: '#f39c12' }
    ]
  }
};
```

- [ ] **Step 2: Verify data loads**

Open in browser, open console, type `DATA.revenue.total` — should return `737`. No errors.

---

### Task 3: Screen 1 — Pulse (before/after)

**Files:**
- Modify: `dashboard/dashboard.html`

- [ ] **Step 1: Build "Сегодня" state**

Inside Screen 1 section:
- Large animated number: 737M (use CSS `font-size: 64px`, gold color)
- 3 channel cards in a flexbox row (card = white box with shadow, colored left border)
- Each card: channel name, value in M, percentage
- Online card: red left border, pulsing subtle glow to draw attention
- Monthly bar chart using Chart.js (12 bars, August highlighted gold)
- Bottom insight text in italic

- [ ] **Step 2: Build "Через год" state**

Same layout but:
- Number animates from 737 → 880 (counter animation via JS `requestAnimationFrame`)
- 7 cards appear (new ones fade in with 0.3s delay cascade)
- New cards have green/gold borders
- Marketplace card: gray, smaller, subtitle "витрина бренда"
- Chart: same bars + overlaid green line showing online growth
- School season (Jul-Aug bars) get gold highlight + label "+37%"
- Bottom text changes

- [ ] **Step 3: Wire toggle to switch states**

Toggle click:
- Swap big number (with counter animation)
- Swap cards (fade out old, fade in new)
- Swap chart data (Chart.js `update()`)
- Swap bottom text

- [ ] **Step 4: Test screen 1**

Open in browser. Toggle back and forth. Verify:
- Numbers animate smoothly
- Cards appear/disappear with animation
- Chart updates without glitch
- Responsive: check at 768px width

---

### Task 4: Screen 2 — Store X-Ray (before/after)

**Files:**
- Modify: `dashboard/dashboard.html`

- [ ] **Step 1: Build store table for "Сегодня"**

HTML table, styled:
- Header row: Магазин | Город | Выручка/мес | Аренда/мес | Маржа | Руб/м² | Статус
- 19 data rows from `DATA.stores.today`
- Status column: colored circle (●) — green/yellow/red
- Negative margin: red text, bold
- Row hover: light highlight
- Summary row at bottom: bold, total rent 124.9M/год
- Rows clickable: `onclick` toggles detail panel (extra info row below)

- [ ] **Step 2: Build "Через год" state**

Same table but:
- Лубянка: margin changes to +0.3M, status → yellow, note "% от оборота"
- Dubai Mall: revenue 0.1 → 1.8, margin → +0.3M, status → yellow, note "онлайн подключён"
- NEW first row: "Онлайн-каналы" — rent 0, revenue 80M, bright green, full width highlight
- 2 closed stores at bottom: gray text, strikethrough, label in italic

- [ ] **Step 3: Wire toggle**

Same pattern as Screen 1: toggle switches table data and re-renders.

- [ ] **Step 4: Test screen 2**

Toggle, verify table changes. Click rows, verify detail panels. Check iPad width.

---

### Task 5: Screen 3 — Customers

**Files:**
- Modify: `dashboard/dashboard.html`

- [ ] **Step 1: Build segment cards**

4 cards in a row (flexbox, wrap on tablet):
- Each card: colored top border, segment name, count (large number), share/check info
- VIP: gold border + subtle shimmer
- Потерянные: red border, count in red

- [ ] **Step 2: Build "Your child grew up" card**

Centered, wide card with gold border:
- Icon: 👶→🧒 (large Unicode)
- Text block with count (1,240) and potential (6.2M) highlighted
- Fake button styled as gold pill: "Отправить подборку по новому размеру"
- Button has hover effect but no real action (or shows a tooltip "Демонстрация")

- [ ] **Step 3: Build activity feed**

Scrollable container (max-height 250px, overflow-y auto):
- Each entry: time (gray) + text + colored tag badge
- New entries have subtle slide-in animation on page load
- Auto-scroll to show latest

- [ ] **Step 4: Test screen 3**

Verify all 4 segments render, gold card is prominent, feed scrolls. Tablet layout: cards 2x2.

---

### Task 6: Screen 4 — Inventory

**Files:**
- Modify: `dashboard/dashboard.html`

- [ ] **Step 1: Build 3 zone indicators**

3 large cards in a row:
- Each: colored background (light tint), large number (units + value in M), label text
- Red zone: largest font, pulsing border animation to draw attention
- Total frozen: 95M below all three, red, bold

- [ ] **Step 2: Build brand breakdown chart**

Horizontal stacked bar chart (Chart.js, horizontal bar):
- Each brand = one bar
- 3 segments per bar: green/yellow/red
- Labels on left: brand name
- BORELLI bar is longest (visually dominant)
- Tooltip on hover: exact units per zone

- [ ] **Step 3: Build action plan card**

Green-tinted card at bottom:
- 3 numbered steps with checkmark icons
- Each step: action + expected result
- Big number at bottom: "До 30M руб. возвращается в оборот" — green, bold

- [ ] **Step 4: Test screen 4**

Verify zones render, chart shows correct proportions, action plan readable. iPad layout: zones stack vertically.

---

### Task 7: Screen 5 — Marketing ROI

**Files:**
- Modify: `dashboard/dashboard.html`

- [ ] **Step 1: Build competitor budget comparison**

Horizontal bar chart (Chart.js):
- 4 bars: Вы (short, red tint), Choupette, Gulliver (long), Норма рынка (dashed)
- Labels include absolute value + percentage
- "Вы" bar is visually tiny compared to others — the gap is the message

- [ ] **Step 2: Build channel ROI table**

Styled table:
- Columns: Канал | Потрачено | Принесло | ROI | Вердикт
- ROI column: colored (green for >2x, yellow for 1-2x, red for <1x)
- Verdict column: colored badge (green "Усилить", red strikethrough "Убрать", yellow "Оптимизировать")
- СМС row: entire row has light red background + strikethrough on name
- Блогеры row: double green badge, subtle gold highlight

- [ ] **Step 3: Build reallocation visual**

Two side-by-side doughnut charts (Chart.js):
- Left: "СЕЙЧАС" — current allocation
- Right: "ПРЕДЛОЖЕНИЕ" — redistributed (SMS removed, shifted to Bloggers/Video/Site)
- Arrow between them (CSS or SVG)
- Text below: "Тот же бюджет 12.9M → отдача с ~5M до ~12-15M"
- Second line: "При бюджете 30M → ожидаемая отдача 80-100M"

- [ ] **Step 4: Test screen 5**

Verify budget bars show dramatic gap. ROI table sorts visually. Doughnuts render. iPad: doughnuts stack vertically.

---

### Task 8: Polish and final testing

**Files:**
- Modify: `dashboard/dashboard.html`

- [ ] **Step 1: Add page load animation**

On first load:
- Brand name fades in (0.5s)
- Screen 1 elements cascade in (staggered 0.1s delays)
- Big number counts up from 0 to 737M

- [ ] **Step 2: Add print-friendly styles**

`@media print` CSS:
- Hide navigation sidebar
- Show all screens sequentially (not hidden)
- Remove animations
- White background, black text

- [ ] **Step 3: Cross-browser test**

Test in:
- Chrome (Mac) — primary
- Safari (Mac) — partner may use this
- Safari (iPad) — secondary target
- Check: fonts render, charts render, toggle works, navigation works

- [ ] **Step 4: File size check**

Verify total file size < 400KB. If over: minify Chart.js more aggressively, or use Chart.js lite.

- [ ] **Step 5: Copy to ready_docs**

```bash
cp dashboard/dashboard.html ready_docs/Дашборд_прототип.html
```

- [ ] **Step 6: Final human review**

Open final file, click through all 5 screens, toggle both before/after screens, verify all Russian text is correct, no English visible, no tech terms.
