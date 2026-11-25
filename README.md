# Student Management System (Netlify + database)

Admin and student logins, marks, attendance, student details and printable report cards.
Data is saved in Netlify's built-in database. You do not need another account or a connection string.

## Put it online (4 steps)
1. **GitHub:** create a repository and upload everything in this folder.
2. **Netlify:** Add new project > Import an existing project > pick that repository > Deploy.
   Netlify creates the database and its tables by itself during the deploy.
3. **Set your admin password:** Site configuration > Environment variables > add `ADMIN_PASS` with a password only you know.
   Then Deploys > Trigger deploy. (Without it the admin password is `admin123`.)
4. **Open your site.** Log in as `admin` and add your first student.

## Using it
- Admin: add students (extra details are optional), enter marks, take attendance by date, open a student's
  Profile, search, export to CSV, reset passwords, open report cards.
- Student: sees their own details, marks, attendance % and report card.
- Attendance % = (present + late) / days recorded. Grades: 90+ A+, 80+ A, 70+ B, 60+ C, 50+ D, else F.

## If something goes wrong
- **Deploy log says the database feature is not available for your account:** Netlify's database needs a
  credit-based plan. Use any free Postgres instead (Supabase or Neon), copy its connection string, add it as an
  environment variable named `DATABASE_URL`, and deploy again. Tables are created automatically.
- **Deploy fails while setting up the database on the very first deploy:** in a terminal in this folder run
  `npx netlify deploy` once, then `npx netlify deploy --prod`.
- **Pages load but login shows "Could not reach the database":** open the deploy log and look for
  "Netlify Database setup". Send me that part of the log.

## Run it on your own computer (optional)
Needs Node.js 20.12 or newer.
1. `npm install`
2. Copy `.env.example` to `.env`
3. `npx netlify database migrations apply`
4. `npx netlify dev` and open http://localhost:8888

`npm test` runs 30 checks on the backend logic (no database needed).

## Good to know
- Passwords are stored hashed. There is no limit on wrong password attempts, so keep `ADMIN_PASS` strong.
- Student data is personal information. Do not share your Netlify login.

## Folders
    public/                              pages (login, admin, student) + css + js
    netlify/functions/api.mjs            all server logic
    netlify/database/migrations/         the database tables (applied automatically on deploy)
    lib/                                 database queries, login security, grades, report card
