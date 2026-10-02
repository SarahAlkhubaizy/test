# Section 2: Components, Classes, and Database Design

## 1. Back-End Component & Class Descriptions

The back-end layer handles business logic, user authentication, trail management, and data query processing.

### Core Classes

#### 1. User
Represents standard users interacting with the platform.
* **Attributes:**
  * `id`: `int` — Unique identifier for the user.
  * `name`: `string` — Full name of the user.
  * `email`: `string` — Unique user email address.
  * `password`: `string` — Hashed user password.
  * `role`: `string` — Role identifier (e.g., `"USER"`).
* **Methods:**
  * `register()`: Registers a new user account.
  * `login()`: Authenticates the user.
  * `logout()`: Terminates the active session.
  * `updateProfile()`: Modifies personal details.
  * `viewFavorites()` / `addFavorite()` / `removeFavorite()`: Manages bookmarked trails.
  * `addReview()` / `viewMyReviews()`: Creates and views user feedback.

#### 2. Admin (Inherits from User)
Extends the `User` class to grant administrative privileges for content moderation and trail management.
* **Attributes:** Inherits all attributes from `User`.
* **Methods:**
  * `addTrail()` / `updateTrail()` / `deactivateTrail()`: Handles lifecycle operations for trails.
  * `uploadTrailImages()`: Uploads media files associated with a trail.
  * `updateTrailRoute()`: Updates geospatial coordinates for a trail route.

#### 3. Trail
Represents a hiking or walking route managed in the system.
* **Attributes:**
  * `id`: `int` — Unique identifier.
  * `name`: `string` — Name of the trail.
  * `description`: `string` — Detailed summary of the trail.
  * `region`: `string` — Geographic region/city.
  * `difficulty`: `string` — Difficulty rating (e.g., Easy, Moderate, Hard).
  * `distance`: `float` — Total length in kilometers.
  * `estimatedDuration`: `time` — Expected completion time.
  * `images`: `Array` — List of image URLs.
  * `isOfficial`: `boolean` — Flag indicating whether the trail is verified.
  * `startPoint` / `endPoint`: `Point` — Geographic coordinates for start and end locations.
  * `routeCoordinates`: `Route` — Array/Polyline of GPS points representing the path.
  * `status`: `string` — Current state (e.g., Active, Closed).
  * `createdAt`: `DateTime` — Record creation timestamp.
* **Methods:**
  * `getDetails()` / `getLocation()` / `getRoute()`: Retrieves trail metadata and GIS data.
  * `isActive()`: Checks if the trail is accessible.
  * `calculateAverageRating()`: Computes aggregate user score.
  * `getReviews()`: Retrieves associated reviews.

#### 4. Review
Encapsulates user feedback and ratings for specific trails.
* **Attributes:**
  * `id`: `int` — Unique review identifier.
  * `userId`: `int` — Foreign key referencing `User`.
  * `trailId`: `int` — Foreign key referencing `Trail`.
  * `rating`: `int` — Numeric rating score (1 to 5).
  * `comment`: `string` — Textual review body.
  * `createdAt`: `DateTime` — Timestamp of creation.
* **Methods:**
  * `addReview()` / `updateReview()` / `deleteReview()` / `getReview()`: CRUD operations for reviews.
  * `validateRating()`: Ensures submitted rating falls within 1–5 range.

#### 5. Favorite
Represents a junction/association entity for user bookmarks.
* **Attributes:**
  * `userId`: `int` — Foreign key referencing `User`.
  * `trailId`: `int` — Foreign key referencing `Trail`.
  * `createdAt`: `DateTime` — Timestamp when added.
* **Methods:**
  * `addFavorite()` / `removeFavorite()`: Toggles saved status.
  * `isFavorite()`: Checks if a trail is bookmarked by a specific user.
  * `getUserFavorites()`: Fetches all saved items for a user.

#### 6. TrailService
Utility service class handling query processing, filtering, and sorting logic for trails.
* **Methods:**
  * `searchByName(name: string)`
  * `filterByRegion(region: string)`
  * `filterByDifficulty(difficulty: string)`
  * `sortByRating()` / `sortByDate()`
  * `clearFilters()`

---

## 2. Class Diagram

```mermaid
classDiagram
    class User {
        +int id
        +string name
        +string email
        +string password
        +string role
        +register()
        +login()
        +logout()
        +updateProfile()
        +viewFavorites()
        +addFavorite()
        +removeFavorite()
        +addReview()
        +viewMyReviews()
    }

    class Admin {
        +id
        +name
        +email
        +password
        +addTrail()
        +updateTrail()
        +deactivateTrail()
        +uploadTrailImages()
        +updateTrailRoute()
    }

    class Trail {
        +int id
        +string name
        +string description
        +string region
        +string difficulty
        +float distance
        +time estimatedDuration
        +Array images
        +boolean isOfficial
        +Point startPoint
        +Point endPoint
        +Route routeCoordinates
        +string status
        +DateTime createdAt
        +getDetails()
        +getLocation()
        +getRoute()
        +isActive()
        +calculateAverageRating()
        +getReviews()
    }

    class Review {
        +int id
        +int userId
        +int trailId
        +int rating
        +string comment
        +DateTime createdAt
        +addReview()
        +updateReview()
        +deleteReview()
        +getReview()
        +validateRating()
    }

    class Favorite {
        +int userId
        +int trailId
        +DateTime createdAt
        +addFavorite()
        +removeFavorite()
        +isFavorite()
        +getUserFavorites()
    }

    class TrailService {
        +searchByName()
        +filterByRegion()
        +filterByDifficulty()
        +sortByRating()
        +sortByDate()
        +clearFilters()
    }

    %% Relationships
    User <|-- Admin : inherits
    User "1" -- "0..*" Review : writes
    User "1" -- "0..*" Favorite : saves
    Trail "1" -- "0..*" Review : receives
    Trail "1" -- "0..*" Favorite : saved_in
    TrailService ..> Trail : manages/searches
3. Database Structure & Schema
Option A: Relational Database Schema (SQL)
users Table:

id (INT, Primary Key, Auto Increment)

name (VARCHAR(100), Mandatory)

email (VARCHAR(150), Unique, Mandatory)

password (VARCHAR(255), Mandatory)

role (VARCHAR(20), Mandatory, Default: 'USER')

created_at (TIMESTAMP, Default: CURRENT_TIMESTAMP)

trails Table:

id (INT, Primary Key, Auto Increment)

name (VARCHAR(150), Mandatory)

description (TEXT, Optional)

region (VARCHAR(100), Mandatory)

difficulty (VARCHAR(50), Mandatory)

distance (DECIMAL(5,2), Mandatory)

estimated_duration (TIME, Mandatory)

images (JSON / TEXT[], Optional)

is_official (BOOLEAN, Default: TRUE)

start_point (POINT / GEOMETRY, Mandatory)

end_point (POINT / GEOMETRY, Mandatory)

route_coordinates (LINESTRING / JSON, Mandatory)

status (VARCHAR(20), Default: 'Active')

created_at (TIMESTAMP, Default: CURRENT_TIMESTAMP)

reviews Table:

id (INT, Primary Key, Auto Increment)

user_id (INT, Foreign Key referencing users(id), Mandatory)

trail_id (INT, Foreign Key referencing trails(id), Mandatory)

rating (INT, Check: rating >= 1 AND rating <= 5, Mandatory)

comment (TEXT, Optional)

created_at (TIMESTAMP, Default: CURRENT_TIMESTAMP)

favorites Table:

user_id (INT, Foreign Key referencing users(id))

trail_id (INT, Foreign Key referencing trails(id))

created_at (TIMESTAMP, Default: CURRENT_TIMESTAMP)

Primary Key: (user_id, trail_id)

Option B: Document-Oriented Database Schema (MongoDB)
Collection: users
JSON
{
  "_id": "ObjectId",        // Mandatory
  "name": "String",          // Mandatory
  "email": "String",         // Mandatory, Unique
  "password": "String",      // Mandatory
  "role": "String",          // Mandatory (USER / ADMIN)
  "createdAt": "Date"        // Mandatory
}
Collection: trails
JSON
{
  "_id": "ObjectId",        // Mandatory
  "name": "String",          // Mandatory
  "description": "String",   // Mandatory
  "region": "String",        // Mandatory
  "difficulty": "String",    // Mandatory
  "distance": 0.0,           // Mandatory (in KM)
  "estimatedDuration": "00:00:00", // Mandatory
  "images": ["String"],      // Optional
  "isOfficial": true,        // Mandatory
  "startPoint": {            // Mandatory (GeoJSON Point)
    "type": "Point",
    "coordinates": [0.0, 0.0]
  },
  "endPoint": {              // Mandatory (GeoJSON Point)
    "type": "Point",
    "coordinates": [0.0, 0.0]
  },
  "routeCoordinates": [      // Mandatory
    [0.0, 0.0]
  ],
  "status": "String",        // Mandatory
  "createdAt": "Date"        // Mandatory
}
Collection: reviews
JSON
{
  "_id": "ObjectId",        // Mandatory
  "userId": "ObjectId",     // Mandatory (Ref: users)
  "trailId": "ObjectId",    // Mandatory (Ref: trails)
  "rating": 5,               // Mandatory (1-5)
  "comment": "String",       // Optional
  "createdAt": "Date"        // Mandatory
}
Collection: favorites
JSON
{
  "_id": "ObjectId",        // Mandatory
  "userId": "ObjectId",     // Mandatory (Ref: users)
  "trailId": "ObjectId",    // Mandatory (Ref: trails)
  "createdAt": "Date"        // Mandatory
}
4. Front-End Main Components and Interactions
Authentication Components (AuthForm, LoginModal, RegisterModal)

Purpose: Manages user authentication and registration.

Interactions: Calls User.login() and User.register(), updates overall application authentication context upon success.

Navigation Bar (Navbar, UserProfileMenu)

Purpose: Provides quick system navigation, user profile settings, and conditional controls based on role (Admin vs Standard User).

Interactions: Displays user info, links to user favorites/reviews, and triggers logout().

Search and Filter Bar (TrailSearch, FilterBar, SortDropdown)

Purpose: Provides UI controls to query and sort trails.

Interactions: Interacts with TrailService to filter results in real time by region, difficulty, rating, or keyword search.

Trail Listing View (TrailList, TrailCard)

Purpose: Displays overview cards for discovered trails.

Interactions: Clicking a card redirects to TrailDetailsView. Provides a quick toggle to trigger addFavorite() / removeFavorite().

Trail Details View (TrailDetailsView, MapContainer, ImageGallery)

Purpose: Displays comprehensive details for a selected trail, including interactive route map and photos.

Interactions: Triggers Trail.getDetails(), renders geospatial polyline data on MapContainer, and loads associated user reviews.

Review Section (ReviewList, ReviewForm, StarRatingInput)

Purpose: Displays community feedback and enables user review submissions.

Interactions: Validates rating score via validateRating(), posts feedback using addReview(), and triggers Trail.calculateAverageRating().

Admin Dashboard (AdminDashboard, TrailManagementForm, RouteEditor)

Purpose: Admin-only UI for content operations.

Interactions: Allows creating trails (addTrail()), updating routes (updateTrailRoute()), uploading images, and updating trail status (deactivateTrail()).
