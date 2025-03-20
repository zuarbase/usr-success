# Tableau Filters Extraction Tool

## Overview

This tool is a standalone HTML page that integrates with an embedded Tableau visualization to extract and display applied filters (global filters, parameters, and local filters) in a user-friendly format. The script uses a combination of Tableau’s API, event-based callbacks, polling, and even DOM extraction as a fallback to obtain filter information from the visualization.

The interface provides three primary buttons:

- **Get Tableau Filters:** Fetches filters using a standard (non-force) method.
- **Force Fetch Filters:** Forces a data reload when standard methods do not yield the desired results.
- **Debug Viz Structure:** Outputs a structured JSON view of the internal Tableau viz object to help diagnose any issues.

The tool attempts to retrieve the Tableau visualization (viz) object from a global `zPortal.tableau` JavaScript object. It monitors the viz until it's ready (using both the `firstinteractive` event and fallback polling) and then accesses key properties such as the workbook and active sheet to extract filters.

## Prerequisites

- **Tableau API Integration:** The tool relies on the environment exposing a `zPortal` object with a `tableau` property. This object should provide methods (such as `getViz()`) that return the visualization instance.
- **Tableau Viz Embedding:** An embedded Tableau visualization (for instance, within a `<tableau-viz>` element or an `<iframe>`) must be present on the page.
- **Modern Browser:** A modern browser that supports ES6 JavaScript features and CSS animations is required to run the page effectively.

## Getting Started

1. **Setup:**  
   Save the provided code as an HTML file (for example, `index.html`).

2. **Integration:**  
   Ensure that your environment defines the `zPortal` and `zPortal.tableau` objects. These objects are typically provided by your Tableau integration or embedding platform.

3. **Deployment:**  
   Open the HTML file in a web browser. The page will display the viz ID, a placeholder for the data month, and three buttons to interact with the tool.

## Usage

- **Fetching Filters:**  
  Click the **Get Tableau Filters** button to initiate the standard data retrieval process. The page will display a loading spinner while the script attempts to retrieve and parse the viz object. If successful, the applied filters will be organized into sections (Global Filters, Parameters, and Other Filters).

- **Force Fetch:**  
  If the standard process fails to load or retrieve filter details, click the **Force Fetch Filters** button. This forces the script to use fallback approaches, such as direct DOM extraction, to gather the filter information.

- **Debugging the Viz Structure:**  
  The **Debug Viz Structure** button outputs a structured JSON representation of the viz object. This is useful if you need to inspect available properties and methods for troubleshooting.

## Code Structure and Highlights

- **CSS Styling:**  
  Embedded CSS defines the layout and appearance of elements such as the spinner, buttons, filter cards, and error messages. This provides clear visual feedback during the filtering extraction process.

- **JavaScript Functions:**  
  - `getTableauViz(vizId, forceLoad)`: Retrieves the Tableau viz object. It attempts to use any cached version, listens for the `firstinteractive` event, and falls back on polling until the viz is deemed ready.
  - `fetchTableauData(vizId, forceLoad)`: Uses the viz object to access the workbook and active sheet. It gathers parameters and applied filters via asynchronous calls (such as `workbook.getParametersAsync()` and `worksheet.getFiltersAsync()`).
  - `extractFiltersFromDOM()`: Provides a fallback mechanism by inspecting the DOM of the embedded Tableau viz if direct API extraction is unavailable.
  - `inspectObject(obj, maxDepth, currentDepth)`: Inspects and serializes objects for debugging, helping to understand the structure of the viz object.
  - `displayFilters(data)`: Renders the final data (filters, parameters, and other details) into the respective HTML containers.

- **Event Handling:**  
  Click event listeners on the buttons trigger the appropriate functions. Error handling and UI feedback (like enabling/disabling buttons and showing/hiding a loading spinner) are incorporated to improve user experience.

## Troubleshooting

- **Undefined `zPortal.tableau`:**  
  If you encounter errors indicating that `zPortal` or `zPortal.tableau` is undefined, verify that your Tableau integration properly provides these objects before this script executes.

- **Viz Not Ready:**  
  In cases where the viz does not become interactive within the polling period, try using the **Force Fetch Filters** button. Check the browser’s developer console for detailed error messages and logging.

- **Cross-Origin Restrictions:**  
  The DOM extraction fallback may fail due to same-origin policy restrictions if the Tableau viz is hosted on a different origin without proper CORS configuration.

## Customization

- **CSS:**  
  You can modify the inline CSS to better match your site's design or to adjust the layout of filter cards and notifications.

- **Polling Configuration:**  
  Adjust the polling interval or maximum poll count in the `getTableauViz()` function if your Tableau viz generally requires a longer loading time.

- **Filter Logic:**  
  The classification of filters into global, parameters, or local (other) can be tailored by modifying the conditions within the `fetchTableauData()` function.

