# Tournament Management API

A comprehensive REST API for creating, managing, and running different types of tournaments including single elimination, round-robin, group stage, and double elimination formats.

## Features

### 🔐 Authentication
- JWT-based authentication
- Admin-only registration and management
- Secure password hashing with bcrypt

### 🏆 Tournament Management
- Create, update, delete tournaments
- Support for multiple tournament types:
  - Single Elimination
  - Round Robin
  - Group Stage
  - Double Elimination
- Tournament status tracking (draft, running, finished)
- Flexible tournament settings

### 👥 Player/Team Management
- Add, update, remove players or teams
- Support for both individual and team-based tournaments
- Player seeding system
- Team name support

### ⚔️ Match System
- Auto-generate matches based on tournament type
- Manual match creation and editing
- Score submission and result tracking
- Automatic standings calculation
- Match scheduling support

### 📊 Standings & Analytics
- Real-time standings calculation
- Points system (3 for win, 1 for draw, 0 for loss)
- Win/loss/draw statistics
- Tournament progress tracking

## Tech Stack

- **Runtime**: Node.js with TypeScript
- **Framework**: Express.js
- **Database**: PostgreSQL
- **Authentication**: JWT (JSON Web Tokens)
- **Validation**: express-validator
- **Documentation**: Swagger/OpenAPI
- **Security**: Helmet, CORS, bcrypt

## Quick Start

### Prerequisites
- Node.js (v16 or higher)
- PostgreSQL (v12 or higher)
- npm or yarn

### Installation

1. **Clone and install dependencies**:
```bash
npm install
```

2. **Set up environment variables**:
```bash
cp .env.example .env
# Edit .env with your database credentials and JWT secret
```

3. **Set up PostgreSQL database**:
```bash
# Create database
createdb tournament_db

# Run the initialization script
psql -d tournament_db -f src/database/init.sql
```

4. **Start the development server**:
```bash
npm run dev
```

The API will be available at `http://localhost:8080`

## API Documentation

Once the server is running, visit `http://localhost:8080/api-docs` for interactive API documentation powered by Swagger UI.

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new admin user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user profile

### Tournaments
- `GET /api/tournaments` - Get all tournaments
- `POST /api/tournaments` - Create new tournament
- `GET /api/tournaments/:id` - Get tournament by ID
- `PUT /api/tournaments/:id` - Update tournament
- `DELETE /api/tournaments/:id` - Delete tournament
- `GET /api/tournaments/:id/standings` - Get tournament standings

### Players
- `POST /api/players` - Add player to tournament
- `GET /api/players/tournament/:tournamentId` - Get players in tournament
- `GET /api/players/:id` - Get player by ID
- `PUT /api/players/:id` - Update player
- `DELETE /api/players/:id` - Delete player

### Matches
- `POST /api/matches/generate` - Auto-generate tournament matches
- `POST /api/matches` - Create manual match
- `GET /api/matches/tournament/:tournamentId` - Get tournament matches
- `GET /api/matches/:id` - Get match by ID
- `PUT /api/matches/:id` - Submit match result
- `DELETE /api/matches/:id` - Delete match

## Usage Examples

### 1. Register and Login
```bash
# Register admin user
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "password123",
    "name": "Tournament Admin"
  }'

# Login
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "password123"
  }'
```

### 2. Create Tournament
```bash
curl -X POST http://localhost:8080/api/tournaments \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "name": "Summer Championship",
    "type": "single_elimination",
    "settings": {
      "team_based": false,
      "auto_generate_matches": true,
      "max_players": 16,
      "description": "Annual summer tournament"
    }
  }'
```

### 3. Add Players
```bash
curl -X POST http://localhost:8080/api/players \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "name": "John Doe",
    "tournament_id": 1,
    "seed": 1
  }'
```

### 4. Generate Matches
```bash
curl -X POST http://localhost:8080/api/matches/generate \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "tournament_id": 1
  }'
```

### 5. Submit Match Result
```bash
curl -X PUT http://localhost:8080/api/matches/1 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "score_a": 3,
    "score_b": 1
  }'
```

## Tournament Types

### Single Elimination
- Players are eliminated after one loss
- Winner advances to next round
- Continues until one player remains

### Round Robin
- Every player plays every other player once
- Points awarded: 3 for win, 1 for draw, 0 for loss
- Winner determined by total points

### Group Stage
- Players divided into groups
- Round-robin within each group
- Top players from each group advance

### Double Elimination
- Players must lose twice to be eliminated
- Winners and losers brackets
- More forgiving format

## Database Schema

The API uses PostgreSQL with the following main tables:

- **users**: Admin user accounts
- **tournaments**: Tournament information and settings
- **players**: Players/teams in tournaments
- **matches**: Individual matches and results

## Security Features

- JWT-based authentication
- Password hashing with bcrypt
- Input validation and sanitization
- CORS protection
- Helmet security headers
- SQL injection prevention

## Development

### Scripts
- `npm run dev` - Start development server with hot reload
- `npm run build` - Build TypeScript to JavaScript
- `npm start` - Start production server
- `npm run lint` - Run ESLint

### Project Structure
```
src/
├── config/          # Database and Swagger configuration
├── middleware/      # Authentication and validation middleware
├── models/          # Database models
├── routes/          # API route handlers
├── services/        # Business logic services
├── types/           # TypeScript type definitions
├── database/        # Database initialization scripts
└── server.ts        # Main application entry point
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License.