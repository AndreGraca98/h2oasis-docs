# H2Oasis: Project Overview & Technical Implementation

## Purpose

H2Oasis is a modern, full-stack application designed to manage and automate irrigation systems, such as garden watering setups. Its goal is to provide users with a user-friendly dashboard for monitoring, controlling, and configuring irrigation systems and their valves, supporting both manual and automated operation. The platform is intended for home automation enthusiasts, gardeners, and anyone seeking to optimize water usage and garden health.

---

## Technical Architecture

1. **Frontend (UI)**
   - **Framework:** [Quasar](https://quasar.dev/) (Vue.js-based)

   - **Features:**
     - Responsive, mobile-first design
     - Dark/light theme support with live color editing (admin color scheme page)
     - Internationalization (English and Portuguese)
     - System and valve management (add, edit, delete, view history)
     - API integration via Axios
     - Docker support for deployment

   - **Structure:**
     - [ui](ui/) folder contains all frontend code
     - Pages for systems, valves, admin, and debug tools
     - Components for navigation, dialogs, forms, and theme controls
     - State management via [Pinia](https://pinia.vuejs.org/) ([systems-store.js](ui/src/stores/systems-store.js), [valves-store.js](ui/src/stores/valves-store.js), ...)
     - Customizable theme via CSS variables in [pallettes.css](ui/src/css/pallettes.css) and [app.css](ui/src/css/app.css)
     - Boot files for initializing plugins (e.g., Axios, i18n)

   - **Customization:**
     - Theme colors can be edited live in the admin panel and are stored in localStorage for persistence
     - Translations are managed in [en-US/index.js](ui/src/i18n/en-US/index.js) and [pt-PT/index.js](ui/src/i18n/pt-PT/index.js)
     - UI communicates with the backend using Axios, with API base URL configured via environment variables

2. **Backend (API)**
   - **Framework:** [FastAPI](https://fastapi.tiangolo.com/)
   - **Features:**
     - RESTful API for systems and valves
     - Database models for systems, valves, and tasks (see ER diagram)
     - CORS support for frontend-backend communication
     - Static file serving for documentation and assets
     - Docker support for deployment
     - Automated testing and linting via Makefile and PDM scripts

   - **Structure:**
     - [api](api/) folder contains all backend code
     - [src/](api/src/) contains FastAPI app, routers, models, and utility functions
     - Database models for `System`, `Valve`, and `Task` with relationships
     - API endpoints for CRUD operations, history, and environment info
     - Configuration via `.env` and [pyproject.toml](api/pyproject.toml)
     - Scripts for setup, testing, and linting

   - **Customization:**
     - Database support is optional and can be enabled via setup scripts
     - API endpoints are documented and accessible via Swagger UI (`/docs`)
     - Environment variables control API host, port, and other settings

3. **DevOps & Deployment**
   - **Docker**: Both UI and API have Dockerfiles and Compose files for containerized deployment
   - **Makefile**: Project-wide and per-service Makefiles provide commands for setup, running, cleaning, and testing
   - **Environment**: `.env.template` files for both UI and API allow easy configuration of ports and API URLs

## Implementation Details

- **System Management**: Users can add, edit, and delete irrigation systems. Each system has a name, description, state, and associated valves.
- **Valve Management**: Each valve is linked to a system and can be controlled individually. Valves have states (e.g., OPEN/CLOSED) and history tracking.
- **Task Scheduling**: The backend supports tasks for automating valve operations (e.g., scheduled watering).
- **History & Monitoring**: Both systems and valves have history tracking, allowing users to view past actions and states.
- **Theme & UX**: The UI supports dark/light mode and live theme editing. Changes propagate across the app and persist between sessions.
- Internationalization: All UI text is translatable, supporting multiple languages.
- **Testing & Quality**: The backend uses pytest and coverage for testing, and ruff for linting. Pre-commit hooks enforce code quality.

## Example Workflow

1. User logs in to the dashboard.
2. Adds a new irrigation system via the UI, which sends a POST request to the API.
3. Adds valves to the system, configuring their pins and initial states.
4. Schedules tasks for automatic watering or manually toggles valves.
5. Monitors system and valve history to track watering events.
6. Customizes the UI theme and language as desired.
7. Deploys the app using Docker for production use.

## Summary

H2Oasis is a robust, extensible platform for smart irrigation management. It combines a modern Vue.js UI with a powerful FastAPI backend, supports real-time customization, and is ready for deployment in both development and production environments. The project is well-structured for collaboration, testing, and future expansion.

For more details, see the [ui/README.md](ui/README.md), and [api/README.md](api/README.md).

## To check the app from a different device

Run the following command in a terminal on the device the app is running on to get the IP address of the device.
Then, open a browser on the other device and type in the IP address followed by the port number the app is running on.

```bash
$ ipconfig getifaddr en0
192.xxx.x.xxx
```

## Implementation Plan

ok, lets start from scratch. the point of this project is to be able to control a few electro valves. each valve need to belong to a system. so i need to be able to:

- create a system
- update a system
- view a system
- view all the systems
- delete a system
- create a valve in a system
- update a valve
- view a valve
- view all the valves in a system
- view all the valves
- delete a valve

a system should have:

- id (int)
- name (str)
- description (str)
- state ("ON", "OFF")
- history (list of entries with name, description, state, when it happened. this should be automatic and should happen on table update)
- created_at (datetime utc)
- updated_at (datetime utc)
- valves (a list of valves -relationship)

- a method to turn on the system
- a method to turn off the system

a valve should have:

- id (int)
- name (str)
- pin (int)
- description (str)
- state ("OPEN", "CLOSED")
- history (list of entries with name, pin, description, state, system_id, when it happened. this should be automatic and should happen on table update)
- created_at (datetime utc)
- updated_at (datetime utc)
- system_id (fk to a system)

- a method to open the valve
- a method to close the valve

in the dashboard i want to be able to perform the following flow:

1. view the existing systems
2. create a new system
3. open the newly created system
4. view it's details, history and associated valves
5. add a new valve
6. open a valve
7. turn on the system

let's go step by step. first understand my request, then create a plan to make the changes, and then start making the changes properly

Final Endpoint Structure:
Systems:

- GET /v1/systems - List all systems with their valves
- GET /v1/systems/{id} - Get system details with its valves
- POST /v1/systems - Create a system
- PATCH /v1/systems/{id} - Update system
- DELETE /v1/systems/{id} - Delete system
- POST /v1/systems/{id}/state - Turn system on/off

Valves (all under systems):

- POST /v1/systems/{system_id}/valves - Create valve
- PATCH /v1/systems/{system_id}/valves/{valve_id} - Update valve
- DELETE /v1/systems/{system_id}/valves/{valve_id} - Delete valve
- POST /v1/systems/{system_id}/valves/{valve_id}/state - Open/close valve

Benefits of This Approach:

- Clear Ownership: Every valve operation is explicitly tied to its parent system
- Consistent URL Structure: All operations follow the pattern /systems/{system_id}/...
- Logical Access Control: Easy to check system-level permissions before allowing valve operations
- Better Data Integrity: Ensures valves can't be modified outside their system context
- Simpler Frontend: The system ID is always available from the URL context

This makes the API more intuitive and matches your user flow better:

- User views systems
- Selects a system
- Views/manages valves within that system context
- Controls valves within the same system context

### system page requirements
ok, lets start by organizing the system page first. here are some requirements:

1. page/card structure
2. header title and some actions like refresh/edit
3. section with system name, id, description, creted/updated and an action to turn on/off
4. section with a table for the valves. the table should follow the format of the systems table in the systems page (with actions search, add, refresh actions in it's header, columns for the valves - id, name,pin, description and state - , actions for each valve -edit/delete) and pagination
5. section for the history summary
