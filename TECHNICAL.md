# This documentation will serve for engineers to know ho## Data Model (Draft)
-## AI Moderation
- Custom AI prompts for lore moderation (low temperature, experimentation-based).
- Rules enforcement: no unfair advantages, no immortality/invincibility, no real-life copies, no character repetition.
- Manual moderator review required for character creation (not AI).
- Appeals process via Discord notifications and tickets.
- Repeat offender handling: permanent ban across all systems.
- Content formatting: Markdown support with character/lore hyperlinking and referencing.:** id, discord_id, minecraft_uuid, verified, characters, nations, strikes, ban_status, application_data, activity_status
- **Character:** id, name, surname, additional_name, age, family_connections, lore, nation, approval_status, stats
- **Nation:** id, name, members, resources, lore, leader, towns, rotation_schedule, bank_balance
- **Town:** id, name, nation_id, members, resources, founder, creation_date
- **Lore:** id, title, content, author, verified, public/private, ai_moderation_status, category, tags, references
- **LoreVersion:** id, lore_id, content, timestamp, author, status
- **Economy:** id, user_id, balance, transactions, bank_transfers
- **Punishment:** id, user_id, type, reason, moderator, timestamp, active
- **LinkingCode:** user_id, code, expiry_date, platforms_linked, status
- **LogEntry:** id, timestamp, severity, plugin, action, details, user_idject will be coded, how it will work etc.

# ValorStone Network Technical Plan

## Technologies
- **Website & API:** Node.js (Next.js, Express.js, TypeScript)
- **Discord Bot:** Python (Pycord)
- **Minecraft Plugin:** Java (Spigot API)
- **Database:** PostgreSQL (production), SQLite3 (development)

## System Architecture
- Central API (Node.js/Express.js) for all data operations.
- Website (Next.js/React) and Discord bot are clients to the API.
- Minecraft plugin communicates with API via HTTP requests.
- All account linking and verification handled via API.

## Modules
### 1. API
- Endpoints for lore, player data, economy, bans, nations, characters.
- Authentication (NextAuth.js for Discord OAuth2, custom JWT for Minecraft).
- Data validation and moderation (OpenAI for lore, manual for characters).
- Account linking via unique daily code system (same code for one day per player).
- Handles strikes (max 3), bans, and punishment tracking.
- ALT detection and verification checks.
- Public/private data access control (historical lore public, personal data restricted).

### 2. Website
- Next.js frontend with Minecraft-themed UI (pixelated fonts, themed buttons, book textures, item hover context).
- User registration with light application process (special tricks to confirm rule reading).
- Character creation interface (Name, Surname, Additional name, Age, Family connections, extensive Lore).
- Smart family connection system (links only to existing verified characters).
- Wikipedia-style lore editing with draft system (owner edits, verification workflow).
- Economy visualization (public balance leaderboards, private transaction handling).
- Search and filtering system (word search, tag filtering, lore categories).
- Moderator panel (verification queue, character/player management, ban/strike tools).
- Maintenance mode with database rollback capabilities.
- Extensive logging system with web interface (severity filtering, plugin grouping).

### 3. Discord Bot
- Pycord bot with commands for character info, nation data, public information retrieval.
- Account linking commands using daily code system.
- Event notifications (lore verification, bans, nation events, wars, character approval).
- Moderation commands (lore verification, punishments, strikes management).
- Easy to extend with new commands and features.
- Integration point for forum/discussion (handled via Discord server channels).

### 4. Minecraft Plugin
- Spigot plugin for account linking enforcement (shows daily code on join attempt).
- Join restriction system (verified character required for server access).
- Event listeners for player death, join, etc. (hardcore mode toggleable, death by player only initially).
- API sync for job status, health status, bank balance, season data, current age.
- Manual ban/restriction system for moderators (report-based or observation-based).
- Moderation mode for staff (invisible actions, same roleplay rules as players).
- Resource and nation mechanics (player-driven economy, special world designs).
- Integration with external plugins (towns, economy, seasonal progression).

## Data Model (Draft)
- **User:** id, discord_id, minecraft_uuid, verified, characters, nations, strikes, ban_status, application_data
- **Character:** id, name, status, health, economy, nation, history, lore, age, features, approval_status
- **Nation:** id, name, members, resources, lore, leader, towns, rotation_schedule
- **Lore:** id, title, content, author, verified, public/private, ai_moderation_status
- **Economy:** transactions, balances, resources, bank, job_data
- **Punishment:** id, user_id, type, reason, moderator, timestamp, active
- **LinkingCode:** user_id, code, expiry_date, used_platforms

## Security
- Secure API endpoints (authentication, authorization).
- Data validation and sanitization.
- Rate limiting and abuse prevention.

## AI Moderation
- Integrate OpenAI for content moderation (lore, nation creation).
- Manual override for moderators.
+- AI rules: no unfair advantages, no immortality/invincibility, no real-life copies, no character repetition.
+- Manual moderator review required for character creation (not AI).
+- User report mechanism via tickets and evidence for inappropriate content.
+- Common sense rule enforcement: no targeted/rude content, balanced character creation.

## Extensibility
- Families prioritized for new player onboarding after server start.
- Wars, professions, jobs, titles planned for future updates.
- System designed to be complete before launch, not modular during operation.
- Player-driven economy balance with staff intervention when needed.
- Modular API for new features, but most features prepared before usage.

## Deployment
- Docker for API and website.
- CI/CD pipeline for automated testing and deployment.
- Documentation for setup and contribution.