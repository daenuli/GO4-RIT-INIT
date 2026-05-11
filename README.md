# GO4-Rit Project Structure

## Main Directory Structure

```text
app/
bootstrap/
config/
database/
lang/
lib/
public/
resources/
routes/
storage/
tests/
```

### `app/`

This folder contains the core backend logic of the application, separated by responsibility:

- `Models/`: Eloquent models for core entities such as user, team, workspace, board, chat, notification, and payroll.
- `Http/Controllers/`: controllers for web and API endpoints.
  - `Marketplace/`: controllers for marketplace features.
  - `Admin/`: controllers for the administration panel.
  - `Api/`: controllers for API endpoints.
- `Http/Requests/`: request validation before entering the business logic layer.
- `Policies/`: access rules per model or resource.
- `Services/`: application logic beyond the controller layer, typically for process orchestration.
- `Jobs/`: asynchronous processes such as sending notifications, data synchronization, or other background tasks.
- `Events/` and `Listeners/`: event-driven flow for domain actions.
- `Notifications/` and `Mail/`: in-app notifications and emails.
- `Observers/`: hooks triggered when a model is created, updated, or deleted.
- `Enums/`: enum types for status, role, visibility, priority, and other domain values.
- `Concerns/`, `Support/`, `Casts/`, `Contracts/`, `Pivot/`: helpers, interfaces, custom casts, abstractions, and pivot models.

### `bootstrap/`

Contains the Laravel application bootstrap files and helpers loaded when the application starts.

### `config/`

All application configuration lives here, including authentication, database, broadcasting, cache, mail, queue, tenancy, and third-party integrations.

### `database/`

Contains migrations, factories, and seeders.

- `migrations/`: database schema definitions.
- `factories/`: fake data for testing or seeding.
- `seeders/`: initial data for development or local setup.

### `lang/`

Translation files for the languages used by the application.

### `lib/`

Reusable code that is not directly part of the main Laravel domain. Libraries in this folder are designed to be shared across `Marketplace`, `Admin`, and `Api`.

- `payment/`: library for payment integration and processing.
- `auth/`: library for authentication and authorization.
- `otp/`: library for OTP generation and validation.

### `public/`

The web root of the application. The Laravel entry point file lives here along with public assets such as `robots.txt` and compiled frontend build assets.

### `resources/`

All frontend assets and views live here.

- `css/`: main stylesheets.
- `js/`: main frontend source based on Vue + Inertia.
- `views/`: Blade views still required by the application.
- `front/`: assets or source specific to certain frontend areas.

#### `resources/js/` Structure

```text
resources/js/
  Components/
  Composables/
  CustomElements/
  Layouts/
  Pages/
  plugins/
  stores/
  utils/
  types/
  enums/
  lib/
```

- `Components/`: reusable Vue components.
- `Layouts/`: page shells such as the app shell, dashboard, or auth layout.
- `Pages/`: Inertia pages mapped from backend routes.
- `Composables/`: reusable reactive logic.
- `stores/`: frontend state management.
- `plugins/`: Vue plugins or global integrations.
- `utils/`, `types/`, `enums/`: utilities, TypeScript types, and frontend enums.

### `routes/`

Application routes separated by area.

- `web.php`: main web routes.
- `auth.php`: authentication routes.
- `api.php`: API endpoints.
- `channels.php`: broadcast channel definitions.
- `console.php`: console commands.
- `team-routes.php`, `workspace-routes.php`, `profile-routes.php`, `admin-routes.php`, `lab-routes.php`: routes grouped by feature or domain.

### `storage/`

Laravel runtime storage for logs, cache, temporary files, and sample data.

### `tests/`

Contains automated tests.

- `Feature/`: feature-level or request-level tests.
- `Unit/`: unit tests for individual classes or functions.
- `Helper/`: helpers for test support.

## Application Flow

In general, the workflow in this repository follows these steps:

1. Routes in `routes/` receive the incoming request.
2. Controllers in `app/Http/Controllers/` process the request and call services or models.
3. Validation is handled via `app/Http/Requests/`.
4. Domain logic is delegated to `app/Services/`, models, events, jobs, or policies.
5. Web responses are typically sent to Inertia pages in `resources/js/Pages/`.
6. Real-time data interactions, notifications, and background processes are handled via events, jobs, queues, and broadcasting.

## Conventions

- Models and enums are used to keep domain terminology consistent.
- Complex business logic should not be placed directly in controllers.
- Reusable UI components are placed in `resources/js/Components/`.
- Inertia pages are stored in `resources/js/Pages/` and typically follow the route or feature name.
- Cross-cutting features such as tenancy, chat, payroll, and board are separated into appropriate layers for maintainability.

## Development Commands

- Backend and local processes: `composer dev`
- Frontend build: `npm run build`
- Frontend development: `npm run dev`
- Testing: `npm run test` or `php artisan test`

## Additional Notes

If you want to expand this README into more complete documentation, the most useful next steps would be to add:

1. a request flow diagram from route to Inertia page,
2. a feature list per domain folder,
3. a local environment setup guide and required `.env` variables.
