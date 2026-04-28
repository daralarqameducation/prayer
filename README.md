# Prayer Times Display

A full-screen prayer times display for mosques, built as a single HTML file. Pulls live data from a Google Spreadsheet — no backend required.

---

## Features

- Live prayer times from Google Sheets (auto-refreshes at midnight)
- Today's Begins + Jama'ah times, plus tomorrow's Jama'ah
- Asr shows both Hanafi and Shafi'i begin times
- Jumu'ah times cycle through all rows in the JummahTimes tab
- Sunrise time from sheet
- Active prayer row highlighted with gold accent
- Blackout screen during Jama'ah (10 min window)
- Countdown to next Iqamah
- Mosque logo, name, and address in top card
- Hijri date displayed

---

## Google Spreadsheet Setup

**Sheet ID:** `1ShUEYYtEFOEnqWf8Go9mK8aSJsTp4G8aQ0oAZTAl8uc`

The sheet must be **published to the web**:
> File → Share → Publish to web → Entire document → CSV → Publish

### Tab: `PrayerTimes`

| Column | Description |
|---|---|
| `month` | Month number (1–12) |
| `day_of_month` | Day number (1–31) |
| `fajr_start` | Fajr begins |
| `fajr_congregation_start` | Fajr Jama'ah |
| `sunrise_start` | Sunrise |
| `zuhr_start` | Zuhr begins |
| `zuhr_congregation_start` | Zuhr Jama'ah |
| `asr_first_start` | Asr begins (Shafi'i) |
| `asr_second_start` | Asr begins (Hanafi) |
| `asr_congregation_start` | Asr Jama'ah |
| `maghrib_start` | Maghrib begins |
| `maghrib_congregation_start` | Maghrib Jama'ah |
| `isha_start` | Isha begins |
| `isha_congregation_start` | Isha Jama'ah |

### Tab: `JummahTimes`

| Column | Description |
|---|---|
| `label` | e.g. `Jummah Khutbah`, `2nd Jummah` |
| `time` | e.g. `13:30` or `1:30 PM` |

Multiple rows supported — display cycles through them every 10 seconds.

### Tab: `Metadata`

| Column | Description |
|---|---|
| `key` | `name`, `logo_url`, `address`, `website` |
| `value` | Corresponding value |

> Note: Metadata is currently hardcoded in the file. See **Configuration** below.

---

## Configuration

Mosque details are hardcoded near the bottom of `display.html`:

```js
const mosque = {
  name:    'Dar Al Arqam',
  address: '1 Kitcat Terrace, Bow, London E3 2SA',
  logo:    'https://raw.githubusercontent.com/Mosque-Screens/mosque-logos/master/arqamlogo2_white.png',
  website: 'daralarqam.co.uk',
  ...
};
```

To use for a different mosque, update these four values and point `SHEET_ID` at your sheet.

---

## Deployment

Works as a plain HTML file — no build step, no dependencies.

**Options:**
- GitHub Pages: push to a repo, enable Pages, done
- Vercel: import repo, deploy as static
- Any web server or CDN
- Locally: open `display.html` directly in a browser

---

## Customisation

| What | Where |
|---|---|
| Sheet ID | `const SHEET_ID = '...'` near top of script |
| Mosque name/logo/address | `const mosque = { ... }` in `init()` |
| Blackout duration | `nowM < jM + 10` in `checkBlackout()` |
| Jumu'ah cycle speed | `10000` ms in `initJummahAnimator()` |
| Colour scheme | CSS variables in `:root { }` |
