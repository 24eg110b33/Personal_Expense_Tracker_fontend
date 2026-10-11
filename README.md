<<<<<<< HEAD
# Pennywise — Personal Expense Tracker

A beginner-friendly full-stack personal finance tracker built with React, Vite, Node.js, and Express. It records income and expenses through a REST API and saves the transactions in a backend JSON file. **MySQL, MongoDB, XAMPP, and any separate database server are not required.**

> **Storage note:** JSON-file storage is ideal for learning, demos, and a single local server. Many cloud hosting services use temporary filesystems, and local JSON files are not a good fit for multiple server instances or concurrent writers. If deploying the backend, confirm that the host provides persistent disk storage and mount it for the data directory. Otherwise data may reset on redeploy/restart.

## Features

- Responsive dashboard for mobile, tablet, and desktop
- Total income, total expenses, remaining balance, and transaction count
- Add income or expenses with amount, date, category, and optional description
- Categories: Food, Travel, Shopping, Education, Bills, Entertainment, Health, Other
- Transaction history with search and income/expense filters
- Delete transactions with confirmation
- Category spending insights
- Success/error notifications and empty states
- Express REST API, CORS, validation, and error handling
- Health-check endpoint
- JSON-file persistence at `backend/data/transactions.json`

## Technologies

- Frontend: React, Vite, JavaScript, HTML, CSS, Lucide React icons
- Backend: Node.js, Express.js, CORS
- Storage: JSON file on the backend
- No MySQL, MongoDB, XAMPP, or database installation

## Project structure

```text
Personal_Expense_Tracker/
├── frontend/
│   ├── src/
│   │   ├── main.jsx
│   │   ├── App.jsx
│   │   └── style.css
│   ├── package.json
│   ├── vite.config.js
│   ├── index.html
│   └── .env.example
├── backend/
│   ├── data/
│   │   └── transactions.json
│   ├── server.js
│   └── package.json
├── README.md
├── .gitignore
└── LICENSE
```

## Requirements

Install **Node.js 18 or newer** (npm is included). Git is optional for local use and required to upload to GitHub. Download Node.js from <https://nodejs.org/>.

## Run locally

Open two terminals in the project folder.

### 1. Start the backend

```bash
cd backend
npm install
npm start
```

Expected message: `Personal Expense Tracker API running at http://localhost:5000`

Health check: <http://localhost:5000/api/health>

### 2. Configure and start the frontend

Open a second terminal:

```bash
cd frontend
```

Create a `.env` file by copying `.env.example` and keep this value for local development:

```ini
VITE_API_URL=http://localhost:5000/api
```

On Windows Command Prompt, you can run:

```bat
copy .env.example .env
```

On macOS/Linux:

```bash
cp .env.example .env
```

Then run:

```bash
npm install
npm run dev
```

Open <http://localhost:5173>.

Keep both terminal windows running while using the application. If you change `.env`, restart the Vite development server. Do not include secrets in frontend environment variables; `VITE_` variables are public in the browser.

## Expected local URLs

| Service | URL |
|---|---|
| Frontend | <http://localhost:5173> |
| Backend | <http://localhost:5000> |
| Health check | <http://localhost:5000/api/health> |

## REST API endpoints

All routes use the `/api` prefix.

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/health` | Checks API/storage availability |
| `GET` | `/api/transactions` | Returns transactions |
| `POST` | `/api/transactions` | Creates a transaction |
| `DELETE` | `/api/transactions/:id` | Deletes a transaction by ID |
| `GET` | `/api/summary` | Returns income, expenses, balance, and count |

### Example POST body

```json
{
  "type": "expense",
  "amount": 250,
  "date": "2026-10-11",
  "category": "Food",
  "description": "Lunch"
}
```

Valid `type` values are `income` and `expense`. Amount must be greater than zero. Dates use `YYYY-MM-DD`.

## Change the API URL

The frontend reads `VITE_API_URL` from `frontend/.env`. For local use:

```ini
VITE_API_URL=http://localhost:5000/api
```

For a deployed backend, set it to the deployed API base URL, ending in `/api`, for example:

```ini
VITE_API_URL=https://YOUR-BACKEND-URL/api
```

Replace `YOUR-BACKEND-URL` with the real URL supplied by your hosting provider. Do not use this example literally. For Vercel/Netlify, add `VITE_API_URL` in the project's environment-variable settings and redeploy/rebuild the frontend.

## Deployment overview

A live URL is **not included** with this source package. You must deploy the backend and frontend to create public URLs.

### Backend — Render (example)

1. Push the project to a GitHub repository.
2. Sign in to <https://render.com/> and create a **Web Service** from that repository.
3. Set the service's **Root Directory** to `backend`.
4. Set **Build Command** to `npm install`.
5. Set **Start Command** to `npm start`.
6. Render supplies the `PORT` environment variable; the server uses it automatically.
7. After deployment, open `https://YOUR-BACKEND-URL/api/health` and confirm the JSON status is `ok`.
8. **Persistent storage:** because the app writes to `backend/data/transactions.json`, configure a persistent disk if your Render plan supports it. Mount it at a writable path and update `DATA_FILE` in `backend/server.js` to a path on that disk (for example, `path.join(process.env.DATA_DIR || __dirname, 'data', 'transactions.json')`, then set `DATA_DIR` appropriately). Check your provider's current persistent-disk support, pricing, and mount-path instructions. Without persistent storage, data can be lost during redeploys/restarts.

Railway can also host the backend. Set the service root directory to `backend`, use `npm install` as the build command and `npm start` as the start command, and configure a persistent volume if available. Set any data directory environment variable to the volume's mounted path and adjust the storage path in `server.js` accordingly.

### Frontend — Vercel (example)

1. Sign in to <https://vercel.com/> and import the GitHub repository.
2. Set **Root Directory** to `frontend`.
3. Framework preset should be **Vite**. Build command: `npm run build`; output directory: `dist`.
4. Add environment variable `VITE_API_URL` with value `https://YOUR-BACKEND-URL/api`, using your actual deployed backend URL.
5. Deploy. If you change the variable, redeploy so Vite rebuilds the frontend.
6. Open the Vercel frontend URL on your phone, tablet, or computer.

For Netlify, import the repository, set the base directory to `frontend`, build command to `npm run build`, publish directory to `dist`, and add the same `VITE_API_URL` environment variable before deploying.

**Important:** The backend must be publicly reachable over HTTPS, and the frontend's `VITE_API_URL` must point to that backend. JSON-file storage is not a managed database; use a persistent mounted disk and a single backend instance if you want to preserve data. For reliable production multi-user use, replace JSON-file storage with a proper persistent database.

## Upload to GitHub

1. Create a **new empty repository** on <https://github.com/new>. Do not add a README or license there because this project already contains them.
2. Extract the ZIP file.
3. Open a terminal in the extracted `Personal_Expense_Tracker` folder (the folder containing `README.md`).
4. Run the commands below, replacing the placeholder with your own repository URL copied from GitHub:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin YOUR_GITHUB_REPOSITORY_URL
git push -u origin main
```

`YOUR_GITHUB_REPOSITORY_URL` is a placeholder, not a real repository URL. Use the URL GitHub shows for the repository you created. If Git asks you to authenticate, follow GitHub's sign-in instructions.

## Troubleshooting

- **`npm` is not recognized:** install Node.js LTS from <https://nodejs.org/> and reopen the terminal.
- **Frontend cannot connect:** make sure the backend is running, visit `/api/health`, and check `frontend/.env` for the correct `VITE_API_URL`.
- **Port 5000 is in use:** set a different `PORT` for the backend and update `VITE_API_URL` to match.
- **Port 5173 is in use:** Vite will show the port it selected; use that URL or stop the conflicting process.
- **Changes to `.env` do not apply:** restart Vite. For deployed builds, trigger a new deployment.
- **Data disappears online:** check that the backend writes to a mounted persistent disk; the default JSON file may be ephemeral on cloud hosting.
- **Invalid or damaged JSON:** stop the backend and check `backend/data/transactions.json`. It should contain a JSON array, such as `[]`.

## License

MIT. See [LICENSE](LICENSE).
# Personal Expense Tracker Frontend

React and Vite frontend for the Personal Expense Tracker.
# Frontend
React + Vite frontend for Personal Expense Tracker.

Run:
`npm install`
`npm run dev`
"# personal_frontend" 
=======
# personal-expenses-tracker-frontend
A frontend application for tracking personal expenses
>>>>>>> fd91b234d93e61564bbf92e0e23401fbc4db7945
>>>>>>> 2a97ef02ee7c789c4a3a918b47fe5fc5055416e6
"# Personal_Expense_Tracker_fontend" 
