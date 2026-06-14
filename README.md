# Verdikt

Verdikt is a single-page web application that provides instant website intelligence. It combines Google's PageSpeed Insights API with AI (Google Gemini or an OpenAI-compatible provider) to analyze web pages and provide actionable business insights based on performance, accessibility, SEO, and best practices.

## Features

- **Authentication**: Firebase Authentication is used to manage user access (currently designed as an invite-only flow).
- **Core Analysis Engine**: Integrates with the Google PageSpeed Insights API to fetch raw metrics (Lighthouse scores) for any URL.
- **AI-Powered Business Insights**: Passes the raw PageSpeed data to an AI model (configurable to use either Google Gemini or an OpenAI-compatible API) to generate human-readable business explanations, prioritize top problems, and estimate the business impact/cost of those problems.
- **Batch Processing**: Supports analyzing multiple URLs sequentially with a progress indicator.
- **Customizable AI Providers**: Users can configure their own Google PageSpeed API key, Gemini API key, or an OpenAI-compatible endpoint (like OpenAI, Groq, DeepSeek) through the settings modal. This data is securely stored locally in the browser's `localStorage`.
- **Export**: Results can be downloaded directly as a CSV file.

## How It Works

1. **User Authentication**: The user is presented with a Firebase email/password login screen. 
2. **API Configuration**: Before analyzing, the user must provide a Google PageSpeed API Key and an AI Provider Key (Gemini or OpenAI) via the "API Settings" dropdown menu.
3. **Fetching Data**: When a URL is submitted, the app first calls the PageSpeed Insights API to get metrics like First Contentful Paint (FCP), Largest Contentful Paint (LCP), Cumulative Layout Shift (CLS), and Lighthouse categorical scores.
4. **AI Generation**: The app constructs a prompt containing the core metrics and sends it to the selected AI provider. The model is instructed to return a strictly valid JSON response containing a business-focused explanation and top problems.
5. **Displaying Results**: The UI renders a verdict badge (Critical, Needs Work, or Excellent) based on scores, updates progress rings for each category, lists the top problems identified by the AI, and shows actionable next steps.

## Running the App Locally

Since the application architecture has been consolidated into a single `index.html` file, running it is incredibly straightforward. You do not need `npm run dev` or a complex build process.

1. **Option 1: Live Server (Recommended)**
   If you have VS Code, simply install the "Live Server" extension, open `index.html`, right-click, and select "Open with Live Server".

2. **Option 2: Simple Python Server**
   If you have Python installed, open your terminal in the directory containing `index.html` and run:
   ```bash
   python3 -m http.server 3000
   ```
   Then navigate to `http://localhost:3000` in your browser.

## Git & Deployment

Currently, this environment doesn't allow direct pushes to your personal GitHub repository. 

**To use Git and deploy:**
1. Export the project by clicking the **"Export to GitHub"** or **"Download ZIP"** button located in the top-right settings menu of Google AI Studio. 
2. If you downloaded the ZIP, extract it on your local machine, run `git init`, `git add .`, and `git commit -m "Initial commit"` to store it locally, then push to your own repository.
