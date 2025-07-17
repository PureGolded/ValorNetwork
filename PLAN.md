# This is the main plan, featuring almost everything without too much technical things, more to inform colaborators or spectators about the plan in detail.
# ValorStone Network - Project Plan

## Project Purpose
ValorStone Network is a role-playing system for Minecraft, integrated with a Discord community and a website. It aims to create a rich, player-driven lore and economy, with strong account verification and moderation to ensure fair and immersive gameplay.

## Key Features
- Central API for all data and account linking
- Website with Minecraft-themed UI (pixelated fonts, book textures, item hover effects)
- Character creation with detailed forms (Name, Age, Family connections, extensive Lore)
- Wikipedia-style lore editing with draft system and version control
- Discord bot for quick info retrieval and moderation (/loresee, /mod commands)
- Minecraft plugin for gameplay enforcement and extensive event logging
- AI and manual moderation for lore and character creation
- Hardcore mode and nation mechanics for immersive roleplay
- Economy system with national banks backed by rare items
- Maintenance mode with database rollback capabilities

## Technologies
- Website & API: Python (Flask, Flask-RESTful)
- Discord Bot: Python (Pycord)
- Minecraft Plugin: Java (Spigot API)
- Database: SQLite3 (initial)

## Account Verification & Linking
- Registration requires a light application process with special tricks to confirm rule reading
- Must acknowledge that roleplay doesn't always end as players want, accept compromises
- Account linking via unique daily code system (same code valid for one day, used on website and Discord)
- Character creation requires moderator review and Discord notification
- ALT detection to prevent banned players from rejoining
- Initial character and lore must be verified before server access

## Moderation & AI
- AI moderation for lore creation/editing (prevents unfair advantages, inappropriate content, immortality/invincibility)
- Manual moderator review for character creation and major changes (hand review, not AI)
- User report mechanism via tickets and evidence for unfair advantages or inappropriate content
- Strikes system (maximum 3 strikes) and punishments linked across all systems
- Common sense rules: no targeted/rude content, no character repetition, no real-life copies
- Moderators have same roleplay rules as players, can enter invisible moderation mode when needed

## Gameplay & Economy
- Hardcore mode (death by player triggers ban until new character created, toggleable for future)
- Nation mechanics (staff-created nations initially, user towns allowed, leader rotation encouraged)
- Economy system with national banks backed by rare items (gold-standard simulation)
- Resource scarcity management through external plugins and world generation limits
- Status sync via API (job status, health status, bank balance, season data, current age progression)
- Cannot join server without verified character and account linking
- Lore categorization system (Nation, Town, Character, Events - wars, reforms, age progression)
- Character stats system (individual stats optional but suggested, generic lore for details)
- Hyperlinking and referencing system for lore interconnections
- Player-driven motivational content with staff balance intervention when needed

## Extensibility & Future Features
- Families prioritized for new player onboarding after server start (family-based joining system)
- Wars, professions, jobs, titles planned for future updates
- System designed to be complete before launch, not modular during operation
- Scalable database and API design, but no scaling expected initially
- Most features should be prepared before usage rather than added during operation
- Discord integration for forum/discussion (not on website)

## Open Questions & Suggestions
- See QUESTIONS.md for ongoing design considerations and staff suggestions

---
Staff and reviewers are encouraged to suggest changes, additions, or improvements to any part of this plan. Please add your feedback to QUESTIONS.md or discuss in the Discord server.