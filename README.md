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
        +
