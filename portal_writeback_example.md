# Approval Form & Database Modification Tool

## Overview

This tool is a standalone HTML page providing an approval form that allows users to efficiently submit form data to a backend API. The form supports both approval and disapproval actions and integrates with a database modification call. When submitted, the data is sent to the `/db_modifications/run` endpoint, which executes a predefined SQL modification.

The associated database modification is defined by the following JSON configuration:

```json
[
  {
    "id": "d7f1e2f8-9216-49ae-a573-070afd7b3c7b",
    "name": "insert_validated_form_data_v2",
    "credentials_id": "bd49d048f3564cb8a91739861b2a8967",
    "sql": "INSERT INTO SUCCESS.COMMENTS.VALIDATED_FORM (COMMENT_ID, USERNAME, PAGE, TIMESTAMP, APPROVED, COMMENT)\nVALUES (:COMMENT_ID, :USERNAME, :PAGE, :TIMESTAMP, :APPROVED, :COMMENT);",
    "default_params": {},
    "updated_at": "2025-01-23T21:43:29.988104+00:00",
    "created_at": "2025-01-22T20:01:20.498047+00:00",
    "access": {
      "groups": [
        "Admin",
        "Editor"
      ]
    }
  }
]
This configuration details the database modification named insert_validated_form_data_v2 which inserts form submissions into the SUCCESS.COMMENTS.VALIDATED_FORM table. It requires parameters for the comment ID, username, page name, timestamp, an approval flag, and optionally the comment text. Only users in the Admin or Editor groups have access to execute this modification.

Prerequisites
Before using this tool, ensure the following are in place:

An API backend is available at https://patrick-portal.zuarbase.net/api/db_modifications/run to process the database modification.
The frontend environment exposes a global zPortal object which provides user information (i.e. username) and page name.
A modern web browser is used, one that supports ES6, CSS animations, and the Fetch API.
Getting Started
Save the Code:
Save the provided HTML, CSS, and JavaScript content as a single HTML file (for example, approval_form.html).
Deploy:
Host or open the file in your preferred web browser. The page displays an Approval Form with two buttons (“Approve True” and “Approve False”) along with the necessary input fields. The form data is built on the fly and submitted using the Fetch API.
How It Works
Approval Form Interface
Approve True Button:
When clicked, the form auto-generates a new comment ID. It hides the comment field since no additional comment is required for an approval. The request data is prepared with APPROVED set to true.
Approve False Button:
When clicked, the form reveals a comment input field. A non-empty comment is required in this mode, and the approval flag is set to false.
Submit Button:
This button remains hidden until either an approval is confirmed (with "Approve True") or, in the case of disapproval, a valid comment is provided. Once clicked, the button triggers the submission process.
Database Modification & API Call
The code sends a POST request to the /db_modifications/run endpoint when the form is submitted. The request body follows this structure:

json
Copy Code
{
  "db_modifications": [
    {
      "name": "insert_validated_form_data_v2",
      "params": {
        "COMMENT_ID": "generated_id",
        "USERNAME": "current_user",
        "PAGE": "current_page",
        "TIMESTAMP": "ISO_timestamp",
        "APPROVED": true_or_false,
        "COMMENT": "comment_or_empty"
      },
      "params_list": []
    }
  ],
  "autocommit": true,
  "ignore_sql_errors": false
}
This structure ensures that the backend knows which database modification to run, and it passes the required parameters for the SQL insert. The SQL statement itself is defined by the DB modification configuration, which inserts the data into SUCCESS.COMMENTS.VALIDATED_FORM.

Key Helper Functions
generateCommentId():
Creates a random 4-digit string to be used as the COMMENT_ID.
createRequestBody(params):
Constructs the request body for the API call. It bundles the parameters along with the required modification name (insert_validated_form_data_v2).
getFormattedTimestamp():
Retrieves the current date and time in ISO format.
validateComment(isApproved, comment):
Verifies that a non-empty comment is provided when the approval flag is false. An error is thrown if validation fails.
UI and State Management
resetForm():
Resets the interface to its initial state by hiding comment fields, cleaning up input values, and reverting button states.
updateStatusMessage(success, message):
Displays a status message to the user indicating whether the submission was successful or if an error occurred. The message auto-hides after 5 seconds.
setLoadingState(isLoading):
Manages the loading spinner and button disabled state during API calls.
Event Listeners
Approve True Button Listener:
Prepares request data immediately with an approval flag and displays the submit button.
Approve False Button Listener:
Reveals the comment input field and waits for the user to provide a comment before enabling submission.
Comments Input Listener:
Monitors user input and displays the submit button only when a valid comment is detected in disapproval mode.
Submit Button Listener:
Initiates the API call to send the form data to the backend. It uses the Fetch API and, depending on the response, updates the UI to show a success or failure status.
Database Modification Details
The backend database modification is defined by the configuration for insert_validated_form_data_v2. Key details include:

SQL Statement:
The SQL command performs an INSERT into the SUCCESS.COMMENTS.VALIDATED_FORM table with the following parameters:
COMMENT_ID
USERNAME
PAGE
TIMESTAMP
APPROVED
COMMENT
Access:
Only users within the "Admin" or "Editor" groups are permitted to run this database modification.
Usage:
This modification is executed by POSTing to the /db_modifications/run endpoint with a request body that identifies the modification by its name and supplies the appropriate parameters.
Troubleshooting and Customization
Ensure the zPortal global object is available before the script runs; it supplies user and page data.
Review the console logs for debugging information if the approval form does not behave as expected.
The inline CSS can be customized to better match your branding or UI requirements.
The request and parameter construction functions can be extended if additional validation or parameter processing is needed.
License and Contributions
Feel free to modify and extend this tool to meet your requirements. Attribution is appreciated where applicable. For any contributions or improvements, please adhere to your organization’s guidelines or open a pull request with the proposed changes.
