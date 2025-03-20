# Tableau Selected Marks Extraction Tool

## Overview

This tool is a standalone HTML page that integrates with an embedded Tableau visualization to extract and display selected marks from worksheets and dashboards. It leverages the global `zPortal.tableau` object provided by the Tableau integration to obtain the visualization (viz) object and retrieve mark selections. The interface includes buttons for fetching selected marks and for debugging the viz structure, and it logs detailed debug information to help diagnose issues.

## Prerequisites

To use this tool, ensure that:
- Your environment exposes a global `zPortal` object with a `tableau` property that supplies the necessary Tableau API functions (such as `getViz()`).
- An embedded Tableau visualization is present on the page (typically within an `<iframe>` or a custom `<tableau-viz>` element) so that interactions such as mark selection are possible.
- You are using a modern web browser that supports ES6 features, CSS animations, and standard HTML5 APIs.

## Getting Started

1. **Setup:**  
   Save the provided code as an HTML file (for example, `index.html`). Open the file in a web browser.

2. **Interface:**  
   The page displays the viz ID along with two main buttons:
   - **Get Selected Marks:**  
     Click this to fetch the selected marks from the current active worksheet or dashboard.
   - **Debug Viz:**  
     Click this to output a detailed view of the viz structure (workbook, active sheet, worksheets, etc.) to the debug log.

3. **Initialization:**  
   On page load, the script logs initial debug information about the page and checks for the existence of required global objects.

## Usage

- **Fetching Selected Marks:**  
  The _Get Selected Marks_ button triggers the following sequence:  
  The script clears previous errors, shows a loading spinner, and retrieves the viz object using the `getTableauViz()` function. It then calls `getAllSelectedMarks()`, which determines whether the active sheet is a worksheet or a dashboard, and retrieves selected marks accordingly. Finally, the retrieved data is rendered as a table displaying column headers and cell values for each data table in the selection.

- **Debugging the Viz Structure:**  
  Clicking the _Debug Viz_ button logs detailed diagnostic information about the viz, including the workbook and active sheet properties, and—if available—a list of worksheet names within a dashboard. This assists in troubleshooting or understanding the structure of the embedded Tableau viz.

## Code Structure and Highlights

The code is organized into several key sections:

- **CSS Styling:**  
  Inline styles define the appearance of scrollable containers, iframes, spinners (with CSS animations), buttons, error messages, debug output, and tables for displaying marks. This ensures that the UI provides clear visual feedback during operations.

- **Debug Logging:**  
  The `debugLog(message, data)` function centralizes debug output by appending timestamped messages to the debug log (and writing to the browser console), which is essential for tracking progress and diagnosing issues.

- **Viz Retrieval and Caching:**  
  The `getTableauViz(vizId)` function retrieves the Tableau visualization object using the global `zPortal.tableau.getViz(vizId)` method. It caches the viz object to avoid redundant external calls and sets up the mark selection event listener if the `TableauEventType` is available from the Tableau API.

- **Mark Selection Handlers:**  
  - The `handleMarkSelectionChanged(event)` function listens for mark selection change events and processes these events by calling `getMarksAsync()` to fetch the selected marks asynchronously.
  - The `getSelectedMarksFromWorksheet(worksheet)` function retrieves selected marks from a specific worksheet by invoking its `getSelectedMarksAsync()` method.
  - The `getAllSelectedMarks(viz)` function determines whether the active sheet is a single worksheet or a dashboard. If it is a dashboard, it iterates over all worksheets to collect marks and then combines them, adding context (such as worksheet names) to the data tables.

- **Data Display:**  
  The `displaySelectedMarksData(marksCollection)` function processes the selected marks for display. It clears previous content, shows a debug summary of the number of data tables found, and for each non-empty data table, dynamically creates an HTML table that displays column names and rows. If there is no marked data, it notifies the user accordingly.

- **Event Handlers and Initialization:**  
  Click event listeners are attached to both the _Get Selected Marks_ and _Debug Viz_ buttons to trigger the respective functions. Additionally, the script logs initialization details on the `DOMContentLoaded` event.

## Troubleshooting

- **Missing Global Objects:**  
  If errors occur stating that `zPortal` or `zPortal.tableau` are not defined, verify that your Tableau integration or embedding platform provides these objects before this script is executed.

- **No Mark Data Returned:**  
  If no selected marks are shown, ensure that marks are actually selected in the visualization. The code handles both worksheets and dashboards, and additional debugging information is logged to help determine which part of the process may be failing.

- **Event Listener Issues:**  
  If mark selection events are not being captured, ensure that the Tableau API is properly loaded and that the event type, `TableauEventType.MarkSelectionChanged`, is available.

## Customization

- **Styling:**  
  Modify the inline CSS to tailor the appearance of buttons, debug logs, tables, and other UI components to your branding or usability requirements.

- **Polling and Caching:**  
  Adjust the caching logic or add additional error handling in the `getTableauViz()` function as needed for your specific Tableau environment or if there are performance considerations.

- **Debug Output:**  
  You can change the verbosity of the debug logs by modifying the `debugLog` function. This can be useful to simplify the output in production environments.

## License and Contributions

This tool is open for modification and extension. Attribution is appreciated where applicable. Contributions, modifications, or improvements are welcome. Please follow your organization’s guidelines for source code contributions or submit a pull request with suggested changes.
