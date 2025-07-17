# This file will contain a structure of the network, how it communicates and works with APIs

# ValorStone Network Structure

## Overview
ValorStone Network is a role-playing system for Minecraft, integrated with a Discord community and a website. It manages lore, player data, economy, bans, and more, with strong account linking and verification.

## Main Components

### 1. Main API
- Central data hub, hosted with the website.
- RESTful API for all network services.
- Database for lore, player data, economy, bans, etc. (SQLite3 chosen for initial implementation)
- Account linking (Minecraft, Discord, Website) via unique daily codes.
- Handles verification, moderation, and data sync.
- Manages strikes (max 3), bans, and punishment system across all platforms.

### 2. Website (Flask)
- Modern Minecraft-inspired UI (pixelated fonts, themed graphics and buttons).
- Account registration with light application process (special tricks to confirm rule reading).
- Account verification (Discord & Minecraft linking, ALT detection, roleplay acknowledgment).
- Character creation with lore, features, age, etc. (moderator review required, Discord notifications).
- Nation browsing (staff-created initially), town creation by users, leader rotation system.
- Lore browsing, creation, and editing (AI moderation for lore, manual review for characters).
- Economy dashboard with public/private data access (historical lore public, personal data restricted).
- Read-only access for unregistered users (lore search and browsing only).
- Moderator panel with moderation mode (invisible actions, punishment tools).

### 3. Discord Bot (Pycord)
- Quick lore lookup and info retrieval (character info, nation data, public information).
- Account linking via unique daily code system.
- Notifications (lore verification status, nation events, wars, bans, character approval).
- Character info commands (view own character details, limited public character info).
- Moderation tools (lore verification, punishments, strikes management).
- Easy to scale with new commands and features.
- Integration with forum/discussion (handled via Discord server, not website).

### 4. Minecraft Plugin (Spigot API)
- Account linking enforced (cannot join without linking, shows unique daily code on join attempt).
- Join restrictions (verified character required for server access).
- Roleplay mechanics sync (job status, health status, bank balance, season data, current age).
- Hardcore mode (death by player triggers ban until new character created, toggleable feature).
- Nation and group mechanics (resource management mostly player-driven, special world designs).
- Manual bans/restrictions by moderators (report-based or moderator observation).
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
- Website: Python (Flask)
- Discord Bot: Python (Pycord)
- Minecraft Plugin: Java (Spigot API)
- API: Python (Flask RESTful)
- Database: To be decided (SQL/NoSQL)