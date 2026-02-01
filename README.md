# Beach-Safety-Haskell-

Beach Safety Tips Web Application (Haskell + Scotty)

This is a lightweight web application built using the Scotty web framework (a Haskell DSL for building simple web servers). It serves both static HTML pages and a RESTful JSON API that provides essential beach safety information to users.

Functional Architecture for Safety
.Backend system implemented in Haskell using Scotty.
.RESTful API endpoints and static page rendering.
.Functional design ensures maintainability and safety.
.Focus on strong typing, immutability, and composability.

A Haskell-based web application powered by the **Scotty** framework that provides essential beach safety information, conservation resources, and interactive guides.

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Installation & Running](#installation--running)
- [Project Structure & File Analysis](#project-structure--file-analysis)
  - [Backend Configuration](#backend-configuration)
  - [Source Code](#source-code)
  - [Frontend Assets](#frontend-assets)
- [API Endpoints](#api-endpoints)

## 🌊 Project Overview
This project, often referred to as "SafeShores" or "Beach Safety Program," is a web server designed to educate users about coastal safety. It combines a robust Haskell backend with a responsive HTML/CSS/JavaScript frontend to deliver:
- Real-time safety tips (General, Rip Currents, Weather, Marine Life).
- Interactive video resources.
- Information on beach conservation initiatives in India.
- Dynamic content fetching via a JSON API.

## 🛠 Tech Stack

### Backend & Core
*   **Language:** [Haskell](https://www.haskell.org/) (GHC 9.x+)
*   **Build System:** [Cabal](https://www.haskell.org/cabal/)
*   **Web Framework:** [Scotty](https://hackage.haskell.org/package/scotty)
    *   *Why:* A lightweight, Sinatra-inspired framework perfect for building simple, robust web servers.

### Key Haskell Libraries
*   **`wai` & `warp`:** The underlying Web Application Interface and high-performance web server that powers Scotty.
*   **`aeson`:** The standard library for JSON parsing and encoding, used here to power the `/api/tips` endpoint.
*   **`text`:** Provides an efficient packed Unicode text type, essential for performant string handling in Haskell.
*   **`wai-middleware-static`:** Middleware used to serve static assets (HTML, CSS, Images, JS) from the `static/` directory.
*   **`wai-extra`:** Provides additional utilities like `logStdoutDev` for request logging.
*   **`http-types`:** Defines standard HTTP types (Status 200 OK, 404 Not Found, etc.).

### Frontend Technologies
*   **HTML5:** Semantic structure using `<header>`, `<nav>`, `<section>`, and `<article>`.
*   **CSS3:** Modern styling features including:
    *   CSS Variables (`:root`)
    *   Flexbox & Grid Layouts
    *   Media Queries for Mobile Responsiveness
    *   CSS Animations & Transitions
*   **JavaScript (ES6+):** Pure "Vanilla" JavaScript without heavy frameworks.
    *   **Fetch API:** For making asynchronous calls to the Haskell backend.
    *   **DOM Manipulation:** For dynamically updating the UI based on API responses.

## 🚀 Installation & Running

### Prerequisites
- **GHC** (Glasgow Haskell Compiler)
- **Cabal** (Haskell Build Tool)

### Steps
1. **Clone/Navigate to the directory**:
   ```bash
   cd c:/Users/sathw/Desktop/every/myScottyApp
   ```

2. **Update dependencies**:
   ```bash
   cabal update
   ```

3. **Build the project**:
   ```bash
   cabal build
   ```

4. **Run the application**:
   ```bash
   cabal run
   ```

5. **Access the App**:
   Open your browser and navigate to: `http://localhost:3000`

## 📂 Project Structure & File Analysis

Here is a detailed breakdown of "each and every file" and how it works:

### 1. Backend Configuration

#### `myScottyApp.cabal`
**Role:** The Project Configuration File.
- **Function:** Defines the package metadata (name, version, author).
- **Dependencies:** Lists all Haskell libraries required (`scotty`, `aeson`, `text`, etc.).
- **Build Instructions:** Tells Cabal that the source code is in `app/` and the entry point is `Main.hs`.

#### `CHANGELOG.md`
**Role:** Version History.
- **Function:** Tracks changes and updates made to the project over time.

#### `LICENSE`
**Role:** Legal.
- **Function:** Specifies the BSD-3-Clause license terms under which this software is distributed.

### 2. Source Code

#### `app/Main.hs`
**Role:** The Core Application Logic (The "Brain").
- **Server Setup:** Initializes the Scotty web server on port 3000.
- **Data Model:** Defines the `SafetyTip` data structure used to store title and content for safety advice.
- **Middleware:** 
  - `logStdoutDev`: Logs requests to the console for debugging.
  - `staticPolicy`: configured to serve files from the `static/` directory (making HTML/CSS accessible).
- **Routes:**
  - `GET /` -> Reads and returns `static/menu.html`.
  - `GET /welcome` -> Reads and returns `static/welcome.html`.
  - `GET /api/tips/:topic` -> **JSON API**. It takes a topic parameter (e.g., "rip", "weather"), looks it up in the `safetyTips` list, and returns the data as JSON. If the topic isn't found, it returns a 404 error.
  - `404 Handler` -> Returns a custom "Page Not Found" message.

### 3. Frontend Assets (`static/` directory)

#### `static/menu.html`
**Role:** The Home / Landing Page.
- **Function:** serves as the entry point (`/`).
- **Features:** 
  - Displays the "Beach Safety Program" branding.
  - Provides navigation links to About Us, Contact, etc.
  - Features a Hero section with a welcoming message.
  - Links to the detailed info page via `Access Detailed Safety Information`.

#### `static/welcome.html`
**Role:** The Main Interactive Dashboard (`/welcome`).
- **Function:** A Single-Page Application (SPA) feel that displays specific safety content.
- **JavaScript Logic:** 
  - Uses `fetch('/api/tips/...')` to get data from the Haskell backend dynamically.
  - Has a fallback mechanism (`clientSideTips`) to show data even if the API fails.
- **UI:** Contains tabs for switching between categories (General, Rip Currents, Weather, Marine Life) and embeds educational YouTube videos.

#### `static/about.html`
**Role:** "About Us" Page.
- **Function:** A static informational page describing the "SafeShores" mission.
- **Content:** Lists the team members, mission statement, and project statistics (e.g., "98% Prediction Accuracy"). Note: This is purely static content for display.

#### `static/getinvolved.html`
**Role:** Conservation & Action Page.
- **Function:** Focuses on beach conservation efforts in India.
- **Features:** 
  - Lists major Indian beaches (Juhu, Marina, etc.).
  - Contains forms for "Reporting Incidents" and "Volunteering". 
  - *Note:* The forms point to `.php` scripts (`submit-report.php`), which are placeholders and will not function without a PHP server or a corresponding Haskell route handler.

#### `static/menu.css`
**Role:** Styling.
- **Function:** Provides the visual styles (colors, layout, fonts) specifically for the `menu.html` page to ensure it looks professional and responsive.

## 🔌 API Endpoints

The application exposes a simple JSON API for fetching safety data data programmatically:

### Get Safety Tip
- **URL:** `/api/tips/:topic`
- **Method:** `GET`
- **URL Params:** `topic` = `general` | `rip` | `weather` | `marine`
- **Success Response:**
  - **Code:** 200
  - **Content:** `{ "title": "...", "content": ["tip 1", "tip 2"] }`
- **Error Response:**
  - **Code:** 404 NOT FOUND
  - **Content:** `{ "error": "Topic not found..." }`


SafeShores demonstrates how functional programming in Haskell can power a clean, reliable web server for real-world use.
Built using Scotty, it combines declarative routing, type-safe JSON APIs, and static content delivery in one lightweight backend.
The project promotes modularity, maintainability, and clarity—core principles of Haskell-based web development.
With its current structure, SafeShores can be easily extended to support live data feeds, mobile applications, and larger deployments.



Final Thought:By leveraging Haskell’s strengths—type safety, purity, and simplicity—SafeShores proves that functional programming isn’t just academic, but practical and production-ready.

make sure you have haskell and scooty through cabal
run in localhost:3000
run these in powershell : cabal build
                          cabal run myScottyApp
