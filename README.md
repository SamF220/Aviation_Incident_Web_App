# Aviation_Incident_Web_App
The Aviation Incidents App is a user-friendly interface designed to allow users to upload a text file containing a list of aviation incidents. The app parses   this file and displays the incidents categorized by year.

# Features
File Upload: Users can upload a .txt file that contains aviation incidents.See main branch for file: 'incidents.txt'
Yearly Categorization: Incidents are categorized by year, and users can select a year to view all incidents from that period.

# Installation:
1. Prerequisites: The application is built using Svelte. Ensure you have Node.js installed.
2. Clone Repository: `git clone https://github.com/SamF220/aviation-incidents-web-app.git`
3. Navigate to the respository: `cd aviation-incidents-web-app`
4. Install dependencies: `npm install`
5. Rund the application: `npm run dev`
   The app should now be running on `http://localhost:5000/` (or your specified port).

# Usage:
1. Navigate to the application's homepage.
2. Use the "Choose File" or "Browse" button to select your .txt file containing aviation incidents.
3. Once uploaded, the app will display buttons categorized by year based on the incidents in the file.
4. Click on a year button to view all incidents from that year.
5. To go back to the main page or to view incidents from another year, use the provided navigation options.

# Project Structure:
1. Public Directory: Contains assets and the root HTML file.
2. Src Directory: Contains the Svelte components and the main app logic.
3. In Src Directory: App.svelte - The main component that handles file uploading, parsing, and displaying the incidents.
