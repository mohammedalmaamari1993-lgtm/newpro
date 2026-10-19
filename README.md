# Training Hub - trainers & trainees management (ASP.NET Core 8 MVC)

## Run it
1. Install the .NET 8 SDK.
2. In this folder run:
   ```
   dotnet restore
   dotnet dev-certs https --trust     # first time only
   dotnet run
   ```
3. Open https://localhost:7180 and sign in with the seeded administrator:
   - Email: `admin@sports.local`
   - Password: `Admin@12345`  (change this in `Data/SeedData.cs` before the first run, or create a new admin and stop using this one)

The SQLite database (`App_Data/training.db`) is created automatically on first run, with four sample employees and three departments.

## What is in it
| Area | Who can do what |
|---|---|
| Employees (ID, code, contact, first/last name, email, department ID, active status) | Everyone signed in can search and view; administrators add and edit |
| Departments (department ID, name) | Everyone views; administrators add, edit, delete |
| Sessions (type, details, trainer, capacity) | Everyone views; `ManageSessions` creates, edits, deletes |
| Internal session bookings (session type + details) | Everyone signed in can book; `ManageSessions` removes |
| Attendance | Everyone views; `RecordAttendance` records |
| Sports schedules | Everyone views; `AssignSchedules` assigns and removes |
| Official tournaments | Everyone views; `BookTournaments` books, confirms, cancels |
| File library | `UploadFiles` uploads and deletes; `DownloadFiles` downloads |
| Users & permissions | Administrators only: create users, tick which permissions each user has |

Employee search (Employees page): type an employee ID for an exact match, or part of a code, name, email or phone number. Filter by department and active status.

## How permissions work
`Models/AppPermissions.cs` lists the six permissions. They are stored as Identity claims on each user and enforced with one authorization policy each (`Program.cs`). The `Admin` role passes every policy. An administrator grants or removes them on the Users & permissions page; changes apply on the user's next page load.

## Images
- `wwwroot/images/uae-logo.svg` is a placeholder built from the UAE flag colours. Replace it with the official logo asset your organisation is licensed to use (keep the file name, or change it in `Views/Shared/_Layout.cshtml` and `Views/Home/Index.cshtml`).
- `wwwroot/images/athlete.svg` and `wwwroot/images/jiujitsu.svg` are simple illustrations. To use photographs, drop `athlete.jpg` and `jiujitsu.jpg` in the same folder and update the two `<img>` tags in `Views/Home/Index.cshtml`.

## Before going live
- Change the seeded admin password and host behind HTTPS.
- Switch from `EnsureCreated` to EF migrations (`dotnet ef migrations add Initial`) if you expect schema changes.
- Move SQLite to SQL Server/PostgreSQL if several people will write at once (change `UseSqlite` in `Program.cs` and the connection string).
- Uploaded files are stored outside `wwwroot` in `App_Data/uploads` and are only served through the Download action.
