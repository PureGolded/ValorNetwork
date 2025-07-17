# This documentation will serve for engineers to know how to project will be coded, how it will work etc.

# ValorStone Network Technical Plan

## Technologies
- **Website & API:** Python (Flask, Flask-RESTful)
- **Discord Bot:** Python (Pycord)
- **Minecraft Plugin:** Java (Spigot API)
- **Database:** SQLite3 (chosen for initial implementation, upgradeable)

## System Architecture
- Central API (Flask RESTful) for all data operations.
- Website and Discord bot are clients to the API.
- Minecraft plugin communicates with API via HTTP requests.
- All account linking and verification handled via API.

## Modules
### 1. API
- Endpoints for lore, player data, economy, bans, nations, characters.
- Authentication (OAuth2 for Discord, custom for Minecraft).
- Data validation and moderation (AI for lore, manual for characters).
- Account linking via unique daily code system (same code for one day per player).
- Handles strikes (max 3), bans, and punishment tracking.
- ALT detection and verification checks.
- Public/private data access control (historical lore public, personal data restricted).

### 2. Website
- Flask frontend with Minecraft-themed UI (pixelated fonts, themed buttons and graphics).
- User registration with light application process (special tricks to confirm rule reading).
- Account linking system (Discord & Minecraft via daily codes, ALT detection).
- Character creation interface (age, features, lore input, moderator review workflow).
- Nation management (staff-created nations, user towns, leader rotation system).
- Lore management interfaces (AI moderation for lore, browse/search functionality).
- Economy visualization (public/private data access, transaction history).
- Moderator/admin panel (moderation mode toggle, invisible actions, punishment tools).
- Read-only access for unregistered users (lore search and browsing only).

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