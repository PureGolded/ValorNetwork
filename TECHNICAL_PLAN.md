# ValorStone Network - Technical Plan

> This documentation serves engineers and developers to understand how the project will be coded, implemented, and how all systems work together.

## System Architecture Overview

ValorStone Network follows a microservices architecture with a central API acting as the data hub for all components. The system consists of four main technical components that communicate via RESTful API calls.

### Architecture Diagram
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Website       │    │  Discord Bot    │    │ Minecraft Plugin│
│ (Next.js/React) │    │   (Pycord)      │    │   (Spigot API)  │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          │              HTTP API Calls                 │
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌─────────────────┐
                    │   Central API   │
                    │(Node.js/Express)│
                    │                 │
                    │  PostgreSQL DB  │
                    └─────────────────┘
```

## Technology Stack

### Core Technologies
- **Backend API**: Node.js 18+ with Next.js API routes and Express.js
- **Frontend**: Next.js with React 18+, TypeScript, Tailwind CSS
- **Discord Bot**: Python with Pycord library
- **Minecraft Plugin**: Java 17+ with Spigot API 1.20+
- **Database**: PostgreSQL 15+ (production), SQLite3 (development)
- **Authentication**: NextAuth.js for OAuth2 Discord integration, JWT for Minecraft
- **Real-time Communication**: Socket.io for WebSocket functionality
- **AI Integration**: OpenAI API for content moderation

### Development Tools
- **Version Control**: Git with GitHub
- **Package Management**: npm/yarn for Node.js dependencies
- **Containerization**: Docker for API and Website deployment
- **Testing**: Jest for JavaScript components, JUnit for Java plugin
- **Documentation**: Markdown with GitHub Pages
- **CI/CD**: GitHub Actions for automated testing and deployment
- **Code Quality**: ESLint, Prettier, TypeScript for frontend/backend

## Database Schema Design

The database design uses **Prisma ORM** with **PostgreSQL** for production and **SQLite** for development. The schema is defined in `prisma/schema.prisma` and includes comprehensive relationships and modern features.

### Core Database Models

```prisma
// User accounts and authentication
model User {
  id              Int       @id @default(autoincrement())
  email           String    @unique
  username        String    @unique
  discordId       String?   @unique
  minecraftUuid   String?   @unique
  verified        Boolean   @default(false)
  strikes         Int       @default(0)
  banStatus       String    @default("none") // none, temp, permanent
  banExpiry       DateTime?
  applicationData Json?
  activityStatus  String    @default("offline")
  createdAt       DateTime  @default(now())
  updatedAt       DateTime  @updatedAt

  // Relations
  characters      Character[]
  loreEntries     Lore[]
  punishments     Punishment[]
  linkingCodes    LinkingCode[]
  logEntries      LogEntry[]

  @@map("users")
}

// Character information and roleplay data
model Character {
  id                Int       @id @default(autoincrement())
  userId            Int
  name              String
  surname           String?
  additionalName    String?
  age               Int?
  familyConnections Json?     // References to other character IDs
  lore              String    // Extensive character background
  nationId          Int?
  approvalStatus    String    @default("pending") // pending, approved, rejected
  stats             Json?     // Optional character stats
  healthStatus      String    @default("healthy")
  jobStatus         String?
  createdAt         DateTime  @default(now())
  updatedAt         DateTime  @updatedAt

  // Relations
  user              User      @relation(fields: [userId], references: [id])
  nation            Nation?   @relation(fields: [nationId], references: [id])
  ledNations        Nation[]  @relation("NationLeader")
  foundedTowns      Town[]    @relation("TownFounder")

  @@map("characters")
}

// Nation and political structure
model Nation {
  id               Int       @id @default(autoincrement())
  name             String    @unique
  leaderId         Int?
  lore             String?
  bankBalance      Int       @default(0)
  rotationSchedule Json?     // Leadership rotation data
  resources        Json?     // Nation resources
  createdAt        DateTime  @default(now())
  updatedAt        DateTime  @updatedAt

  // Relations
  leader           Character? @relation("NationLeader", fields: [leaderId], references: [id])
  members          Character[]
  towns            Town[]

  @@map("nations")
}

// Town management within nations
model Town {
  id         Int       @id @default(autoincrement())
  name       String
  nationId   Int?
  founderId  Int
  members    Json      // Array of character IDs
  resources  Json?     // Town-specific resources
  createdAt  DateTime  @default(now())
  updatedAt  DateTime  @updatedAt

  // Relations
  nation     Nation?   @relation(fields: [nationId], references: [id])
  founder    Character @relation("TownFounder", fields: [founderId], references: [id])

  @@map("towns")
}

// Lore management with versioning
model Lore {
  id                   Int           @id @default(autoincrement())
  title                String
  content              String
  authorId             Int
  category             String        // Nation, Town, Character, Events
  tags                 Json          // Array of tags
  verified             Boolean       @default(false)
  public               Boolean       @default(true)
  aiModerationStatus   String        @default("pending")
  references           Json?         // Array of hyperlinks to other lore
  createdAt            DateTime      @default(now())
  updatedAt            DateTime      @updatedAt

  // Relations
  author               User          @relation(fields: [authorId], references: [id])
  versions             LoreVersion[]

  @@map("lore")
}

// Lore version control
model LoreVersion {
  id        Int      @id @default(autoincrement())
  loreId    Int
  content   String
  authorId  Int
  timestamp DateTime @default(now())
  status    String   @default("draft") // draft, approved, rejected

  // Relations
  lore      Lore     @relation(fields: [loreId], references: [id])

  @@map("lore_versions")
}

// Economic transactions
model Economy {
  id              Int       @id @default(autoincrement())
  userId          Int       @unique
  balance         Int       @default(0)
  lastTransaction DateTime?
  bankTransfers   Json?     // Array of transfer history
  createdAt       DateTime  @default(now())
  updatedAt       DateTime  @updatedAt

  @@map("economy")
}

// Moderation and punishment system
model Punishment {
  id          Int       @id @default(autoincrement())
  userId      Int
  type        String    // strike, tempban, permban, warning
  reason      String
  moderatorId Int
  active      Boolean   @default(true)
  expiresAt   DateTime?
  createdAt   DateTime  @default(now())

  // Relations
  user        User      @relation(fields: [userId], references: [id])

  @@map("punishments")
}

// Account linking system
model LinkingCode {
  id               Int      @id @default(autoincrement())
  userId           Int
  code             String   @unique
  platformsLinked  Json     // Array: discord, minecraft, website
  expiryDate       DateTime
  status           String   @default("active") // active, used, expired
  createdAt        DateTime @default(now())

  // Relations
  user             User     @relation(fields: [userId], references: [id])

  @@map("linking_codes")
}

// Comprehensive logging system
model LogEntry {
  id         Int      @id @default(autoincrement())
  timestamp  DateTime @default(now())
  severity   String   // debug, info, warning, error, critical
  plugin     String   // api, website, discord, minecraft
  action     String
  details    Json?    // Detailed information
  userId     Int?

  // Relations
  user       User?    @relation(fields: [userId], references: [id])

  @@map("log_entries")
}
```

### Key Features of the Schema:
- **Type Safety**: Full TypeScript support with generated types
- **Relationships**: Proper foreign key constraints and relations
- **JSON Support**: Flexible JSON fields for complex data structures
- **Migrations**: Automatic migration generation and management
- **Database Agnostic**: Works with PostgreSQL (production) and SQLite (development)
- **Modern Syntax**: Uses Prisma's declarative schema language

## API Endpoint Specification

### Authentication Endpoints
- `POST /auth/register` - User registration with application process
- `POST /auth/login` - User login (Discord OAuth2)
- `POST /auth/logout` - User logout
- `POST /auth/link` - Account linking with daily codes
- `GET /auth/verify` - Verify account status

### Character Management
- `GET /characters` - List characters (with pagination and filtering)
- `POST /characters` - Create new character
- `GET /characters/{id}` - Get character details
- `PUT /characters/{id}` - Update character (requires approval)
- `DELETE /characters/{id}` - Delete character (admin only)
- `POST /characters/{id}/approve` - Approve character (moderator)

### Lore Management
- `GET /lore` - List lore entries (public and user's private)
- `POST /lore` - Create new lore entry
- `GET /lore/{id}` - Get lore details
- `PUT /lore/{id}` - Update lore (draft system)
- `DELETE /lore/{id}` - Delete lore (author/admin only)
- `POST /lore/{id}/verify` - Verify lore (moderator)
- `GET /lore/{id}/versions` - Get lore version history

### Nation & Town Management
- `GET /nations` - List all nations
- `GET /nations/{id}` - Get nation details
- `POST /nations/{id}/join` - Join nation (if allowed)
- `GET /towns` - List towns
- `POST /towns` - Create new town
- `GET /towns/{id}` - Get town details

### Economy System
- `GET /economy/{user_id}` - Get user's economic status
- `POST /economy/transfer` - Transfer funds between users
- `GET /economy/leaderboard` - Public balance leaderboard
- `POST /economy/bank/{nation_id}` - Nation bank operations

### Moderation
- `GET /mod/queue` - Get moderation queue (characters, lore to verify)
- `POST /mod/strike` - Issue strike to user
- `POST /mod/ban` - Ban user across all platforms
- `GET /mod/logs` - Get moderation logs with filtering

### System Maintenance
- `GET /health` - API health check
- `POST /maintenance/enable` - Enable maintenance mode
- `POST /maintenance/disable` - Disable maintenance mode
- `POST /backup/create` - Create database backup
- `POST /backup/restore` - Restore from backup

## Component Implementation Details

### 1. Central API (Node.js + Express.js)

**Key Features:**
- RESTful API design with consistent response formats
- JWT-based authentication with role-based access control
- Comprehensive input validation and sanitization using Joi/Zod
- Rate limiting to prevent abuse (100 requests/minute per user) via express-rate-limit
- Extensive logging for all operations using Winston
- Health monitoring and automatic failover mechanisms

**Security Implementation:**
- All endpoints require authentication except public lore reading
- SQL injection prevention through parameterized queries (Prisma ORM)
- XSS protection with input sanitization using express-validator
- CORS properly configured for website and Discord bot origins
- API key rotation system for Minecraft plugin authentication
- Helmet.js for additional security headers

**Technology Stack:**
- **Runtime**: Node.js 18+ for optimal performance
- **Framework**: Express.js with TypeScript for type safety
- **ORM**: Prisma for database operations and migrations
- **Validation**: Zod for request/response validation
- **Authentication**: jsonwebtoken for JWT handling
- **Rate Limiting**: express-rate-limit and redis for distributed limiting
- **Logging**: Winston with log rotation and structured logging

### 2. Website (Next.js + React Frontend)

**UI/UX Design:**
- Minecraft-themed interface with pixelated fonts (Minecraft font)
- Book texture backgrounds for content areas using CSS/styled-components
- Item hover effects showing tooltips (similar to Minecraft inventory)
- Responsive design for mobile and desktop access using Tailwind CSS
- Dark/light theme toggle matching Minecraft aesthetics
- Component-based architecture for reusable UI elements

**Technical Implementation:**
- Server-side rendering (SSR) and static generation (SSG) with Next.js
- Client-side hydration for dynamic content updates
- Socket.io integration for real-time notifications
- Progressive Web App (PWA) capabilities with next-pwa
- Accessibility compliance (WCAG 2.1 AA) with react-aria
- TypeScript for type safety and better developer experience
- React Query for efficient data fetching and caching

**State Management:**
- React Context API for global state management
- React Hook Form for form handling and validation
- Local storage for user preferences and session data

**Styling & Animation:**
- Tailwind CSS for rapid UI development
- Framer Motion for smooth animations and transitions
- CSS-in-JS with styled-components for component-specific styles

### 3. Discord Bot (Pycord)

**Command Structure:**
```python
# Lore lookup commands
/loresee character <name>    # Character information with embed
/loresee nation <name>       # Nation details and members
/loresee event <name>        # Historical events

# Moderation commands
/mod ban <user> <reason>     # Ban user across all platforms
/mod strike <user> <reason>  # Issue strike to user
/mod verify <type> <id>      # Verify character or lore

# Utility commands
/link <code>                 # Link Discord account with daily code
/status                      # Check account linking status
```

**Technical Features:**
- Asynchronous command processing
- Embed-rich responses with Minecraft-themed styling
- Error handling with user-friendly messages
- Automatic reconnection and failover
- Integration with ticketing system for user reports

### 4. Minecraft Plugin (Spigot API)

**Core Functionality:**
- Account verification enforcement on player join
- Real-time API synchronization for player status
- Hardcore mode implementation (configurable)
- Event logging for all player actions
- Integration with nation/town plugins

**Technical Implementation:**
```java
public class ValorNetworkPlugin extends JavaPlugin {
    // API client for HTTP requests to central API
    // Event listeners for player actions
    // Command handlers for in-game moderation
    // Database synchronization tasks
    // Health check and failover systems
}
```

## AI Moderation System

### OpenAI Integration
- **Model**: GPT-4 for content analysis
- **Temperature**: 0.3 for consistent, rule-focused responses
- **Prompt Engineering**: Custom prompts for lore moderation rules
- **Fallback System**: Manual review when AI confidence is low

### Moderation Rules Implementation
```python
AI_MODERATION_PROMPT = """
Analyze the following character lore for rule violations:

Rules to enforce:
1. No unfair advantages (superhuman abilities, unlimited resources)
2. No immortality or invincibility
3. No real-life person copies
4. No character repetition
5. Must fit within medieval fantasy setting
6. No inappropriate or offensive content

Respond with: APPROVED, REJECTED, or REVIEW_REQUIRED
Include brief explanation for rejections.

Lore to analyze: {lore_content}
"""
```

## Development Workflow

### Phase-Based Development
1. **Phase 1**: Core API and database implementation
2. **Phase 2**: Website frontend development
3. **Phase 3**: Discord bot implementation
4. **Phase 4**: Minecraft plugin development
5. **Phase 5**: AI moderation system integration
6. **Phase 6**: End-to-end testing and optimization

### Testing Strategy
- **Unit Tests**: 90%+ coverage for all API endpoints
- **Integration Tests**: Cross-component communication testing
- **Load Testing**: API performance under concurrent load
- **Beta Testing**: Closed beta with 20-50 users for 2-4 weeks
- **Security Testing**: Penetration testing and vulnerability assessment

### Deployment Architecture

```yaml
# docker-compose.yml
version: '3.8'
services:
  api:
    build: ./api
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/valorstone
      - DISCORD_CLIENT_ID=${DISCORD_CLIENT_ID}
      - DISCORD_CLIENT_SECRET=${DISCORD_CLIENT_SECRET}
      - NEXTAUTH_SECRET=${NEXTAUTH_SECRET}
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - REDIS_URL=redis://redis:6379
    volumes:
      - ./api/uploads:/app/uploads
    depends_on:
      - db
      - redis
    
  website:
    build: ./website
    depends_on:
      - api
    environment:
      - NEXT_PUBLIC_API_URL=http://api:3001
      - NEXTAUTH_URL=${NEXTAUTH_URL}
      - NEXTAUTH_SECRET=${NEXTAUTH_SECRET}
      - DISCORD_CLIENT_ID=${DISCORD_CLIENT_ID}
      - DISCORD_CLIENT_SECRET=${DISCORD_CLIENT_SECRET}
    
  discord-bot:
    build: ./discord-bot
    depends_on:
      - api
    environment:
      - DISCORD_TOKEN=${DISCORD_TOKEN}
      - API_URL=http://api:3001
  
  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=valorstone
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
  
  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

## Performance Considerations

### Database Optimization
- Indexed queries for common lookups (user_id, character names, nation membership)
- Query optimization for complex lore searches
- Database connection pooling
- Automated backup and recovery procedures

### Caching Strategy
- Redis for session management and frequently accessed data
- API response caching for public lore content
- Minecraft plugin local caching for player data

### Scalability Planning
- Database migration path from SQLite to PostgreSQL
- API horizontal scaling with load balancing
- CDN integration for static assets
- Monitoring and alerting system implementation

## Security Framework

### Data Protection
- GDPR compliance for EU users
- Data encryption at rest and in transit
- Regular security audits and vulnerability assessments
- Secure API key management and rotation

### Anti-Abuse Measures
- Rate limiting on all endpoints
- CAPTCHA for registration and sensitive operations
- IP-based blocking for repeated violations
- Automated detection of suspicious activity patterns

## Maintenance and Operations

### Monitoring Systems
- Application Performance Monitoring (APM)
- Database query performance tracking
- User activity analytics
- Error tracking and alerting

### Backup and Recovery
- Automated daily database backups
- Point-in-time recovery capabilities
- Disaster recovery procedures
- Data integrity verification

This technical plan provides the comprehensive framework for implementing the ValorStone Network system with proper engineering practices, security considerations, and scalability planning.