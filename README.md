AURA (Activity, Utility, Routine, Analytics) is a full-stack fitness tracking web application.
Users may use it to log their workouts, track meals and nutrition. 
This application is also gamified - users earn points and may appear on a public leaderboard if they so desire.

## Features
- User registration, login, logout, and password changes
- Session-based authentication with Passport
- Password hashing with bcrypt
- Workout logging with searchable exercises, duration, calories burned, muscle group, and public/private status
- Meal logging with searchable food items, calories, protein, carbs and fat
- Dashboard totals for macros, calories, weekly progress, goal weight progress, and AURA points
- AURA points system to reward users for logged workouts and meals
- USDA FoodData Central search for nutrition data
- ExerciseDB import/search for exercise data and instructions
- Profile editing with metric/imperial unit conversion
- Profile image upload with file type and size validation
- Public/private profile visibility
- Leaderboard based on public users' AURA points
- Admin panel for managing users, roles, account status, and password resets
- Disabling users -- they will be unable to log in into their accounts

## Tech Stack

Frontend
- Angular
- TypeScript
- Angular standalone components
- Angular signals and computed signals
- Angular forms
- HttpClient

Backend
- Node.js
- Express
- MongoDB
- Mongoose
- Passport
- passport-local
- express-session
- bcrypt
- multer
- dotenv
- cors

## Project Structure

```text
final copy/
├── client/
│   ├── src/
│   │   ├── app/
│   │   │   ├── models/        # TypeScript interfaces
│   │   │   ├── pages/         # Admin, leaderboard, profile pages
│   │   │   ├── services/      # Angular HTTP services
│   │   │   ├── app.ts         # Main Angular component
│   │   │   ├── app.html       # Main UI template
│   │   │   └── app.css        # Main app styling
│   ├── angular.json
│   └── package.json
│
└── server/
    ├── config/
    │   ├── database.js        # Mongoose connection
    │   └── passport.js        # Passport local authentication
    ├── middleware/
    │   └── auth.js            # requireAuth and requireAdmin
    ├── models/
    │   ├── User.js            # User schema and bcrypt hashing
    │   ├── Workout.js         # Workout schema
    │   ├── Meal.js            # Meal schema
    │   ├── Exercise.js        # Exercise library data
    │   └── FoodItem.js        # Cached USDA nutrition results
    ├── routes/
    │   ├── auth.js            # Register, login, logout, me
    │   ├── workouts.js        # Workout CRUD
    │   ├── meals.js           # Meal CRUD
    │   ├── exercises.js       # Exercise search/import
    │   ├── nutrition.js       # USDA food search
    │   ├── user.js            # Profile and leaderboard
    │   └── admin.js           # Admin user management
    ├── scripts/
    ├── app.js                 # Express app configuration
    ├── package.json
    └── bin/www                # Server entry point

```

## API Reference

**Authentication**

| Method | Endpoint | Body | Description |
| ------ | -------- | ---- | ----------- |
| POST | /api/auth/register | username, email, password, fullName | Creates a new account |
| POST | /api/auth/login | username, password | Logs in and creates a session |
| POST | /api/auth/logout | - | Destroy the current session |
| GET | /api/auth/me | - | Get the logged-in user | 
| POST | /api/auth/change-password | oldPassword, newPassword | Change the current user's password|

**Workouts**

All workout routes require authentication

| Method | Endpoint | Body/Query | Description |
| ------ | -------- | ---- | ----------- |
| GET | /api/workouts | ?page=1&limit=10&sort=createdAt&order=desc&visibility=public | List current user's workouts |
| GET | /api/workouts/:id | - | Get one workout |
| POST | /api/workouts | name, exercises, duration, caloriesBurned, muscleGroup, isPublic | Create a workout |
| PUT | /api/workouts/:id | Partial workout fields | Update a workout | 
| DELETE | /api/workouts/:id | - | Delete a workout |

**Meals**

All meal routes require authentication

| Method | Endpoint | Body/Query | Description |
| ------ | -------- | ---- | ----------- |
| GET | /api/meals | ?page=1&limit=10&sort=loggedDate&mealType=breakfast | List current user's meals |
| GET | /api/workouts/:id | - | Get one meal |
| POST | /api/meals | mealType, foodName, calories, protein, carbs, fat | Create a meal |
| PUT | /api/meals/:id | Partial meal fields | Update a meal | 
| DELETE | /api/workouts/:id | - | Delete a meal |

**Exercises**

All exercise routes require authentication

| Method | Endpoint | Body/Query | Description |
| ------ | -------- | ---- | ----------- |
| GET | /api/meals | ?search=bench&muscleGroup=chest | List saved exercises |
| GET | /api/exercises/import | search=squat | Import exercises from ExerciseDB |

**Nutrition**

All nutrition routes require authentication

| Method | Endpoint | Body/Query | Description |
| ------ | -------- | ---- | ----------- |
| GET | /api/nutrition/search | ?q=chicken | Search USDA FoodData Central  |
| GET | /api/nutrition/cached | ?search=rice | Search cached food results |

**Users**

All user routes require authentication

| Method | Endpoint | Body/Query | Description |
| ------ | -------- | ---- | ----------- |
| GET | /api/users/leaderboard | ?page=1&limit=10 | List public leaderboard users  |
| POST | /api/users/profile-image | multipart/form-data | Upload a profile image|
| PUT | /api/users/profile | Profile fields | Update current user's profile |

**Admin**

All admin routes require an authentication with role "admin"

| Method | Endpoint | Body/Query | Description |
| ------ | -------- | ---- | ----------- |
| GET | /api/admin/summary | - | Get user, workout, and meal counts  |
| GET | /api/admin/users | ?page=1&limit=25&search=demo | List users |
| PATCH | /api/admin/users/:id | role, isEnabled, password | Update user role, status, or password |

**Utility**

| Method | Endpoint |  Description |
| ------ | -------- |  ----------- |
| GET | /api/health | Server health Check |# aura-fitness
