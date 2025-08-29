# This file will contain a structure of the network, how it communicates and works with APIs

# ValorStone Network Structure

## Overview
ValorStone Network is a role-playing system for Minecraft, integrated with a Discord community and a website. It manages lore, player data, economy, bans, and more, with strong account linking and verification.

## Main Components

### 1. Main API
- Central data hub, hosted with the website.
- RESTful API built with Node.js and Express.js for all network services.
- Database for lore, player data, economy, bans, etc. (PostgreSQL for production, SQLite3 for development)
- Account linking (Minecraft, Discord, Website) via unique daily codes.
- Handles verification, moderation, and data sync.
- Manages strikes (max 3), bans, and punishment system across all platforms.

### 2. Website (Next.js)
- Modern Minecraft-inspired UI (pixelated fonts, themed buttons, book textures, item hover context windows).
- Account registration with light application process (special tricks to confirm rule reading).
- Account verification (Discord & Minecraft linking, ALT detection, roleplay acknowledgment).
- Character creation form (Name, Surname, Additional name, Age, Family connections, extensive Lore description).
- Smart family connections system (Partner, mother, father - links to existing characters only).
- Wikipedia-style lore editing interface with draft system for verification-required changes.
- Nation browsing (staff-created initially), town creation by users, leader rotation system.
- Economy dashboard with public leaderboard for balances, private transaction handling.
- Read-only access for unregistered users (lore search and browsing with word/tag filtering).
- Moderator panel ("To verify" queue, character management, player ban/strike management).
- Maintenance mode system with database rollback capabilities.

### 3. Discord Bot (Pycord)
- Lore lookup commands (/loresee for characters, nations, events with neat embed responses).
- Account linking via unique daily code system.
- Moderation commands (/mod ban/tempban/unban/strike/removestrike for quick moderation).
- Notifications (lore verification, character approval, nation events, wars, system alerts).
- Error handling with staff notification and user feedback via tickets.
- Easy command extensibility through API endpoints.
- Integration with forum/discussion (handled via Discord server channels).
- Activity tracking for cross-platform user status sync.
- Moderation tools (lore verification, punishments, strikes management).
- Easy to scale with new commands and features.
- Integration with forum/discussion (handled via Discord server, not website).

### 4. Minecraft Plugin (Spigot API)
- Account linking enforced (cannot join without linking, shows unique daily code on join attempt).
- Join restrictions (verified character required for server access).
- Roleplay mechanics sync (job status, health status, bank balance, season data, current age).
- Hardcore mode (death by player triggers ban until new character created, toggleable feature).
- Nation and town integration (self-managed or external plugin sync with out-of-sync notifications).
- Manual bans/restrictions by moderators (report-based or moderator observation).
- Extensive event logging system (wars, deaths, status changes, new players).
- API failure handling (maintenance mode trigger, health checks, staff notifications).
- Moderation mode for staff (invisible actions, same roleplay rules as players normally).
- Performance monitoring integration (Spark plugin) with alert system.
- Moderation mode for staff (invisible actions, same roleplay rules as players normally).
- Integration with external plugins (towns system, economy, seasonal progression).

## Data Flow
- All components communicate via the Main API.
- Website and Discord bot handle most user interactions.
- Minecraft plugin enforces gameplay rules and syncs data.

## Moderation & AI
- AI moderation for lore creation and editing (prevents unfair advantages, inappropriate content, immortality/invincibility, real-life copies).
- Manual moderator review for character creation and major changes (hand review, not AI).
- User report mechanism via tickets and evidence for unfair advantages or inappropriate content.
- Strikes and punishments linked across all systems (max 3 strikes, then ban).
- External plugin assistance for detection and prevention.
- Common sense rules: no targeted/rude content, no character repetition, no excessive advantages.

## Extensibility
- Families prioritized for new player onboarding after server start (family-based joining system).
- Wars, professions, jobs, titles planned for future updates.
- System designed to be complete before usage, not modular during operation.
- Scalable database and API design for growth, but no scaling expected initially.
- Player-driven motivational content, with staff balance intervention when needed.

---

# Technical Stack
- Website: Node.js (Next.js + React)
- Discord Bot: Python (Pycord)
- Minecraft Plugin: Java (Spigot API)
- API: Node.js (Express.js + TypeScript)
- Database: PostgreSQL (production), SQLite3 (development)