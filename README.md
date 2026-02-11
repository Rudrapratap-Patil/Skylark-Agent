## Monday BI Agent

Conversational BI agent on top of monday.com Work Orders and Deals boards. It reads live data via the monday GraphQL API (read-only), normalizes messy fields, computes pipeline + execution metrics, and drafts founder-friendly answers **without requiring any LLM key**.

### 1. Prerequisites

- Node 18+ (you have Node 22 already)
- A monday.com API token with read-only access to the two boards
- No OpenAI key required

### 2. monday.com setup

1. Import the CSVs into monday.com as **two separate boards**:
   - Work Orders: import `Work_Order_Tracker_Data.csv`
   - Deals: import `Deal_funnel_Data.csv`
2. When importing, keep column titles close to the CSV headers (e.g. `Deal Status`, `Close Date (A)`, `Masked Deal value`, `Sector/service`, `Execution Status`, `Amount in Rupees...`).
3. Note the **board IDs** (recommended) or exact board names (fallback).

### 3. Environment variables

Create a `.env` file in `monday-bi-agent/`:

```bash
MONDAY_API_TOKEN=your_monday_token_here
WORK_BOARD_ID=5026564295
DEAL_BOARD_ID=5026566228

# Optional fallback (use if you prefer resolving by board name)
MONDAY_WORK_ORDERS_BOARD_NAME=Your Work Orders Board Name
MONDAY_DEALS_BOARD_NAME=Your Deals Board Name
PORT=4000
```

### 4. Run locally

```bash
cd monday-bi-agent
npm install
npm run server     # starts Express API on http://localhost:4000
npm run dev        # starts Vite UI on http://localhost:5173
```

Or run both in one go (requires `concurrently`, already in `package.json`):

```bash
npm run dev:full
```

Open `http://localhost:5173` in a browser.

### 5. Using the agent

- Ask founder-level questions, for example:
  - “How’s our pipeline looking for energy/renewables this quarter?”
  - “Which sectors are driving most of our open pipeline?”
  - “How are work orders progressing in mining vs powerline?”
- For a leadership-style brief, include **“leadership update”** or **“weekly update”** in your message.

### 6. Hosted prototype guidance

This app is a standard Node + Vite SPA. Recommended deployment:

- **API** (`server/`):
  - Deploy to Render / Railway / Fly.io / AWS (Node 18+).
  - Entry point: `server/index.mjs`
  - Set env vars as per `.env` above.
- **Frontend**:
  - `npm run build` → static assets from `dist/`
  - Serve via the same Node app or a static host (Netlify, Vercel static, etc.).

For quick sharing during development, you can expose the local API via a tunnel (e.g. cloudflared or ngrok) and point the frontend to that URL.

