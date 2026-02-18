# PDF Server

Question Paper PDF Generator Server - Built with Express.js and Puppeteer

## Features

- Generate PDFs from HTML content
- Create formatted question papers
- Designed for high-quality document generation
- CORS enabled for frontend integration
- Ready for cloud deployment (Render)

## Setup & Installation

### Prerequisites
- Node.js (v16 or higher)
- npm or yarn

### Installation

```bash
npm install
```

### Environment Variables

Create a `.env` file in the root directory (or use environment variables in your deployment platform):

```dotenv
PORT=5001
NODE_ENV=development
FRONTEND_URL=https://qgen-mu.vercel.app/
PUPPETEER_HEADLESS=true
PUPPETEER_ARGS=--no-sandbox
```

## Running Locally

```bash
# Start the server
npm start

# Development mode with auto-reload
npm run dev
```

The server will start on `http://localhost:5001` (or the PORT specified in `.env`)

### Health Check

```bash
curl http://localhost:5001/
```

## API Endpoints

### 1. Generate PDF
**POST** `/api/pdf/generate`

Request body:
```json
{
  "htmlContent": "<h1>Your HTML</h1>",
  "filename": "document.pdf",
  "pdfOptions": {}
}
```

### 2. Generate Question Paper
**POST** `/api/pdf/question-paper`

Request body:
```json
{
  "htmlContent": "<h1>Question Paper</h1>",
  "paperTitle": "Question_Paper"
}
```

### 3. Health Check
**GET** `/api/pdf/health`

## Deployment on Render

### Step 1: Prepare Repository
Make sure your code is in a Git repository:

```bash
git add .
git commit -m "Initial commit"
git push
```

### Step 2: Connect to Render

1. Go to [render.com](https://render.com)
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Set the following:
   - **Name**: pdf-server
   - **Environment**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`

### Step 3: Set Environment Variables

In the Render dashboard, go to **Environment**:

```
FRONTEND_URL=https://qgen-mu.vercel.app/
NODE_ENV=production
PUPPETEER_HEADLESS=true
PUPPETEER_ARGS=--no-sandbox
```

### Step 4: Deploy

Click "Create Web Service" and Render will automatically build and deploy your server.

Your backend URL will be something like: `https://pdf-server-xxxx.onrender.com`

## Updating Frontend

Update your frontend's API base URL to point to your Render backend:

```javascript
const API_URL = 'https://pdf-server-xxxx.onrender.com/api/pdf';
```

## Troubleshooting

### Puppeteer Issues on Render
- Ensure `PUPPETEER_ARGS=--no-sandbox` is set
- The `PUPPETEER_EXECUTABLE_PATH` is not needed on Render (it uses system Chrome)

### CORS Issues
- Verify `FRONTEND_URL` is correctly set in environment variables
- The value should match your frontend's exact URL (including https://)

## Performance Notes

- PDF generation is memory-intensive; consider using Render's higher plans for production
- Puppeteer runs in headless mode for optimal performance
- Rendering large PDFs may take 5-10 seconds

## License

MIT
