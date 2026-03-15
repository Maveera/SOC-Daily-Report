# SOC Daily Report Generator

An automated, web-based intelligence report generator designed specifically for Security Operations Centers (SOC). This static web application dynamically renders complex incident data into a clean, paginated, and print-ready report format without needing a backend server.

## Features & Dynamic Flow

### 1. Dynamic Pagination & Rendering
- **Endless Tables**: The `index.html` structure acts as a template. As SOC analysts enter data into the "Potential Incidents" table, the Javascript engine calculates the pixel height of the rows. 
- **Auto-Page Breaks**: If rows threaten to overflow the physical boundaries of an A4 page, the script intelligently clones the table headers, breaks the page, and creates a seamless continuation on the next page.
- **Floating Pins**: Critical elements like the `← End of Document →` pin dynamically detach and reattach themselves to the literal bottom of the final paginated content block to ensure they never overlap text.

### 2. Live Chart Generation
Provides real-time visual analytics using **Chart.js**.
- **Average EPS Chart**: Plots the Daily Average EPS across standard reporting devices using pre-configured datasets.
- **Incident Severity Chart**: A dynamic pie chart reading from user inputs.
- **Live BluPine Analytics**: If the Customer Name is set to **BluPine**, an exclusive script engages. It scans every row of the Potential Incidents table, groups identical incident titles, calculates their exact counts, and instantly renders a customized, fully-responsive clustered Bar Chart mapping out the findings.

### 3. Conditional Customer Logic
- **BluPine Integration**: Entering "BluPine" into the Cover Page name field actively rewrites the DOM. It automatically reveals the exclusive "Potential Incidents with Count" section, re-numbers the primary Incident section from #5 to #6, and injects the new sections seamlessly into the Table of Contents.
- **Conditional Section Split (No Overlap)**: For BluPine, the `Potential Incidents` section is only pushed to the next page when it contains **real incident data**. If the table is empty (or only contains placeholders like `-`, `#`, `TBD`), it stays directly below `Potential Incidents with Count` on the same page to prevent blank-space pages and footer overlap.
- **Smart Notes**: The application parses the statuses of all entered incidents. If any incident across any page is marked "Open," a bold "Pending Action" warning note is generated at the absolute bottom of the report. If all incidents are Closed/Legitimate, a "Confirmed Legitimate" note overrides it.

### 4. Automated Synchronization
- **Centralized Metadata**: Typing the Customer Name or Date on the Cover Page automatically broadcasts those values across the entire report (e.g., updating the Document Control title to read "SOC Daily Report for XYZ").
- **Live TOC**: As pages are dynamically created or destroyed by the pagination engine, the Table of Contents actively recalculates exactly which page number every section currently resides on.

### 5. Layout & Scaling Enhancements (Latest)
- **Exact Box Models**: The Incident charts (Severity, True Positive, False Positive) are wrapped inside strict `366px` height enclosures with exact `15px` padding constraints. This forces `Chart.js` to render them at perfectly identical physical dimensions regardless of the flex-box percentages or the existence of title elements.
- **Robust Table Pagination**: Added an intelligent `250px` buffer layout specifically for the final page to strictly guarantee bottom padding for Note injections and the "End of Document" pin. Injected live `.addEventListener('input')` bindings so that typing into any cell live-paginates the report dynamically, preventing users from pushing content silently off-page.
- **Flexible Data Matching**: The Customer logic matching was relaxed from strict equality to `.includes('blupine')` to support variations in the name (e.g., BluPine Energy, Blupine Systems, etc.) seamlessly.

### 6. Print & PDF Ready
- **A4 Styling**: The entire `style.css` is built around physical print dimensions (`210mm` x `297mm`). 
- **Targeted Branding**: Specific CSS classes inject custom branding, such as applying Navy Blue and Red footer strips exclusively to the Cover Page, while maintaining pastel colors for internal documents.
- **Print Report Button**: Click **PRINT FINAL REPORT** to open the print dialog (print or save as PDF). After the dialog is closed, the page refreshes automatically so you can generate another report.
- **High-Resolution PDF Charts**: Charts are rendered at **16× resolution** (~8K quality) when printing or saving as PDF. Axis labels, legends, and bar edges stay sharp and clear in the exported document. Print-specific CSS keeps canvas rendering crisp.

## Technology Stack
- **HTML5**: Semantic document structure and contenteditable fields.
- **CSS3**: Advanced flexbox layouts and `@media print` rules for perfect physical paper rendering.
- **Vanilla Javascript**: Core DOM manipulation, pagination calculations, and event broadcasting.
- **Chart.js**: Client-side rendering of responsive analytics charts.

## Getting Started
- **Run locally**: Open `index.html` in a browser, or serve the folder (e.g. `npx serve -l 3000`) and visit `http://localhost:3000`.
- **Repository**: [github.com/Maveera/SOC-Daily-Report](https://github.com/Maveera/SOC-Daily-Report)

### Pushing updates to GitHub
From the project folder in Git Bash or a terminal where Git is installed:

```bash
git init
git remote add origin https://github.com/Maveera/SOC-Daily-Report.git
git add .
git commit -m "Update README, print refresh, high-resolution PDF charts"
git branch -M main
git push -u origin main
```

If the repo already has a `main` branch and you have push access, use `git push -u origin main` (or `git push` after the first time).