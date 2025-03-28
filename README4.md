### Technical Writer Tasks API Developer Guide ###
### Introduction ###
* Welcome to the Technical Writer Tasks API developer guide. This API is designed to help your team manage and organize documentation-related tasks efficiently. Whether you're integrating it into a broader task management system or building internal tools, this guide will help you get started quickly and securely.
### Overview ###
* The Technical Writer Tasks API enables users to: *
- **Retrieve** a list of existing tasks based on filters like status, component, or update date. -
- **Create** new tasks for technical writing documentation projects. -
* This lightweight and easy-to-use API is perfect for documentation managers, technical writers, and tools that support agile content development. *
* This API uses **API key authentication.** *
## Header ##
* All requests must include an API key in the header: *
* X-API-KEY: your_api_key_here *
* Requests without this key, or with an invalid key, will return a 401 Unauthorized error. *
________________________________________
API Endpoints
1. GET /tasks
Description: Retrieves a list of tasks. Supports filtering by status, component, or update date.
Method & URI: GET https://api.techwriter.xyz/tasks
Required Headers:
•	X-API-KEY: Your API key
Query Parameters (Optional):
•	status: OPEN, IN_PROGRESS, COMPLETED
•	component: API_DOCS, HELP_CENTER, SDK_DOCS, OAS_FILE
•	updatedAfter: Date in YYYY-MM-DD format
Success Response Example (200):
[
  {
    "task_id": "123",
    "title": "Update SDK docs",
    "description": "Revise SDK documentation for v2.1",
    "status": "IN_PROGRESS",
    "component": "SDK_DOCS",
    "connected_tasks": []
  }
]
Error Response Example (401):
{
  "error": "Unauthorized. API key missing or invalid."
}
Status Codes:
•	200 OK: Successful fetch
•	401 Unauthorized: Missing or invalid API key
________________________________________
2. POST /task
Description: Creates a new technical writer task.
Method & URI: POST https://api.techwriter.xyz/task
Required Headers:
•	Content-Type: application/json
•	X-API-KEY: Your API key
Request Body:
{
  "title": "Document new API endpoint",
  "description": "Write documentation for the /login endpoint.",
  "status": "OPEN",
  "component": "API_DOCS",
  "connected_tasks": [
    {
      "task_id": "321",
      "title": "User authentication docs",
      "description": "Cover token-based login",
      "status": "IN_PROGRESS"
    }
  ]
}
Success Response (201):
{
  "task_id": "789",
  "message": "Task successfully created."
}
Error Response (400):
{
  "error": "Missing required field: title"
}
Status Codes:
•	201 Created: Task successfully created
•	400 Bad Request: Invalid or missing fields
•	401 Unauthorized: Missing or invalid API key
Python Code Example:
import requests

url = "https://api.techwriter.xyz/task"
headers = {
    "X-API-KEY": "your_api_key_here",
    "Content-Type": "application/json"
}
data = {
    "title": "Create onboarding guide",
    "description": "Initial draft for new user onboarding.",
    "status": "OPEN",
    "component": "HELP_CENTER",
    "connected_tasks": []
}
response = requests.post(url, json=data, headers=headers)
print(response.status_code, response.json())
JavaScript (fetch) Example:
const url = 'https://api.techwriter.xyz/task';
const data = {
  title: 'Create onboarding guide',
  description: 'Initial draft for new user onboarding.',
  status: 'OPEN',
  component: 'HELP_CENTER',
  connected_tasks: []
};

fetch(url, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-API-KEY': 'your_api_key_here'
  },
  body: JSON.stringify(data)
})
.then(res => res.json())
.then(data => console.log(data))
.catch(err => console.error(err));
________________________________________
Usage and Best Practices
•	Rate Limits: Although not specified, always assume reasonable rate limits (e.g., 100 requests/min). Avoid making unnecessary requests.
•	Error Handling: Use status codes to detect and respond to issues:
o	400s: Fix your request
o	500s: Try again later or contact support
•	Security:
o	Keep your API key secure—never hard-code it in frontend applications.
o	Use environment variables or secret managers.
•	Validation: Validate all inputs before making requests to avoid 400 Bad Request errors.
•	Logging: Log both request and response payloads in development for easier debugging.
