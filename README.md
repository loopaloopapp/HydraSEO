# 🚀 NVMS - Next.js Metadata Visibility Scanner
### *Dual-Phase Technical SEO Auditor & Core Web Vitals Performance Suite*

**NVMS** is a professional, high-performance technical SEO auditing platform built with **Next.js**. Its core mission is to crawl, identify, and report pages in any Next.js site where critical SEO metadata (such as `<title>`, `description`, `canonical`, `robots`, `Open Graph`, and `Twitter Cards`) are missing from the initial server-rendered HTML response and are instead injected later on the client-side via JavaScript. 

This behavior poses a severe risk for technical SEO, as search engine crawlers (like Googlebot) may fail to accurately index or interpret client-rendered tags due to hydration delays.

Additionally, **NVMS** integrates a robust, Google PageSpeed Insights-inspired diagnostic suite that measures real-time browser performance metrics (Core Web Vitals) and performs accessibility and best practices checks.

---

## 🌟 Core Features

### 🔍 1. Dual-Phase Visiblity Scanner (SSR vs CSR)
*   **Server-Side Phase**: Scrapes the raw HTML payload returned directly from the server, mimicking the official `Googlebot/2.1` User Agent.
*   **Client-Side Phase**: Spawns a headless **Playwright (Chromium)** browser to execute all Javascript logic, wait for `networkidle`, and capture the post-hydration rendered DOM.
*   **Interactive Diff Viewer**: Performs a head-to-head field comparison to instantly pinpoint missing, mutated, or delayed metadata tags.

### ⚡ 2. Core Web Vitals & Timing Diagnostics
Measures real-user experience timings using in-browser APIs (`window.performance`):
*   **Time to First Byte (TTFB)**: Server responsiveness (Optimal: < 200ms).
*   **First Contentful Paint (FCP)**: The duration before the first visual content renders on screen (Optimal: < 1s).
*   **DOM Interactive Time**: The time taken for the DOM structure to become fully clickable and interactive.
*   **Cumulative Layout Shift (CLS)**: Visual layout stability score during page load.

### 🎨 3. Lighthouse Gauges & Dynamic Audits Checklist
*   **Lighthouse CSS Gauges**: Four premium, animatable conic-gradient rings indicating scores (0-100) for *Performance*, *Accessibility*, *Best Practices*, and *SEO*.
*   **Passed & Failed Audits**: A collapsible, interactive checklist showing specific diagnostic audits (e.g. single `H1` tag presence, viewport configurations, HTML `lang` attributes, and target character limits).
*   **Adaptive Button Highlights**: The audit inspection buttons dynamically change color (Green, Yellow, Red) based on the overall quality scores of the analyzed page.

### 💾 4. Seamless Data Export
Export technical scan results in one click to either raw **JSON** or clean **CSV** files for offline distribution or stakeholder reporting.

---

## 🛠️ Technical Architecture & Modules

The project follows highly modular software engineering standards under `/src`:

1.  **Crawler & Queue Management (`src/lib/crawler/`)**:
    *   `discovery.ts`: Scrapes the initial Cheerio-parsed DOM to extract, clean, and queue internal anchor links for Breadth-First Search (BFS) crawling.
2.  **Analysis Engine (`src/lib/analyzer/`)**:
    *   `fetchInitialHtml.ts`: Simple raw network client for standard SSR extraction.
    *   `renderWithBrowser.ts`: Playwright engine equipped with **self-healing browser exception handling**. If the server's OS lacks updated dynamic graphics libraries (like legacy `ffmpeg` on macOS), it gracefully catches the error and applies simulated metrics, preventing `500 Server Errors`.
3.  **Metadata Extraction (`src/lib/extractors/`)**:
    *   `headMetadata.ts`: Parses and collects `<head>` elements (og, twitter, hreflang, robots, canonical, JSON-LD, etc.).
    *   `lighthouseAudits.ts`: An heuristic execution module to generate scores for Lighthouse suites.
4.  **Risk Scoring Engine (`src/lib/scoring/`)**:
    *   `seoRiskScore.ts`: Translates metadata failures and excessive CSR reliance into a risk index ranging from 0 (Perfect) to 10+ (Critical).

---

## 🚀 Installation & Local Setup

Make sure you have **NodeJS >= 20** installed on your local machine.

### 1. Clone the repository and install dependencies
```bash
git clone https://github.com/loopaloopapp/NVMS.git
cd NVMS
npm install
```

### 2. Install Playwright Headless Browsers
Install Playwright's headless Chromium binaries:
```bash
npx playwright install chromium
```

### 3. Spin up the development server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) inside your browser and start scanning pages!

---

## 🐳 Cloud Deployment & Docker

The repository includes a production-ready [Dockerfile](file:///Users/lucaperini/Desktop/NMVS/Dockerfile) that leverages Microsoft's official Playwright image (pre-configured with all system and browser dependencies).

### Deploy to Railway (Recommended for Active Scans)
1. Go to [Railway.app](https://railway.app/) and log in with your GitHub account.
2. Click **New Project** and import this repository.
3. Railway will automatically pick up the `Dockerfile`, build the Next.js bundle, and expose a fully functional Playwright crawler online in seconds!

### Deploy to Vercel (Recommended for Demos & UI Hosting)
1. Import your GitHub repository to [Vercel.com](https://vercel.com/).
2. Click **Deploy**.
3. *Vercel hosts the front-end for free. When performing scans, the app's self-healing fallback will automatically activate the interactive Demo Mode to showcase all performance gauges and UI components.*

---

## 📄 License
This project is licensed under the MIT License. Feel free to modify and adapt.
