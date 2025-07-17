# In this file, AI can put out questions that shall be answered to further enhance the system.
# ValorStone Network - Open Questions & Design Considerations

## General
- What database system should be used (SQL vs NoSQL)?
    - SQL (SQLite3)
- What are the minimum requirements for account verification?
    - Initial Character and its lore should be verified.
    - Must go through a light application process on registering on the website - that includes some special tricks to confirm reading rules etc.
    - Is not an ALT user of banned player, or any player.
    - Must acknowledge that roleplay does not always end how the player wants. Must accept compomises and the average nature of roleplaying.
- How will AI moderation be implemented and what are its limits?
    - AI moderation will be used for lore, that is allowed to be created or contributed by players. Such as some events or their characters. To avoid breaking rules of lore creation.
- What data should be public vs private?
    - Most of the lore data should be public, definitely historical ones. Maybe some economy stuff and other "personal" things for a character should be allowed by verification or registration.
- How will scalability be handled as the network grows?
    - Idealy there will be no scaling, the ideal thing is to prepare everything before its usage. If necessary, it would be handled by expanding the API and database, and implmenting it.

## Website
- What features should be available to unregistered users?
    - Search in lore, browsing lore and other public information.
- How should the Minecraft-themed UI look and feel?
    - Use of the minecrafts pixalated fonts and maybe even buttons and other graphics.
- What is the process for character creation and verification?
    - After registration and linking accounts, user will gain (and be notified on discord about) access to create a character. He will create it by giving it some information, age, features etc. creating some lore for him. Just the normal roleplay stuff. After that moderators will be notified about new character and review it my hand (AI is not for this), then they will either accept and verify it, or contact the user about some issues he should change.
- How will nation creation and management work?
    - Nations will be created by staff before the server starts, users may be able to create towns using different plugin and system, which they can later also include in lore etc.
    - There will be picked or discussed initial leaders of nations, however players will be pushed into rotating them.
- What permissions do moderators/admins have?
    - In roleplaying time, the same as players. With command they will have access to they can enter into moderation mode, where they will be invisible and will be allowed to take necessary action. For punishments etc.
- Should there be a forum or discussion board?
    - Yes, however that will be all on the discord server, so it does not interfere with this system.

## Discord Bot
- What commands should be available to users?
    - To see most information about their own character.
    - To see some public information about other characters or nations etc.
    - The link command for initial linking.
    - Maybe more in future, discord bot will be easy to scale in features.
- What info can be retrieved via Discord?
    - Most of the same that on website, just in lower scale as the website suits for those purposes.
- How will account linking work securely?
    - API would generate a code, that will be sent when trying to join the server. Each player will have always the same code for one day. They will have to input it on the website, as well as using the discord bot command. After this, all accounts should be linked using the one unique code.
- What notifications should be sent to Discord?
    - Mostly about the lore verification, however some in game connections could be made as well. Such as nation has entered war or some other nation announcements (or town).
- What moderation tools are needed?
    - Lore verification, punishments that would be linked throughout the system (ban, tempban, strikes (3 maximum)).

## Minecraft Plugin
- How will account linking be enforced in-game?
    - By "cannot join" message, that would also show a code for linking.
- What events should trigger bans or restrictions?
    - They will be manually added by moderators, if someone reports or they themselves notice player breaking rules.
- How will hardcore mode be implemented (death by player vs any death)?
    - This is to be discussed by staff team, however it will be easily toggable in the plugins code later. For now it is death by player only.
- How will resource management and nation mechanics work?
    - Player will manage it mostly themselves, including economy. However there will be special designs in the server's world. But that wont be included in this plugin.
- What gameplay features should be synced with the API?
    - Job status, Health status, Other statuses maybe, bank balance, the season (serene seasons plugin) or at least the current age. (There will be ages of progression). and some other, this has to be worked out.

## Economy & Lore
- How will the economy be balanced and managed?
    - Mainly by players, but by initial preparation design, or sometimes a little lore from the server.
- What rules govern lore creation and editing?
    - Those will be made at last. So for now some common sensical - like not giving yourself too much advantage, no immortality or invincibility, no rude or targeted things.
    - Plus no repetition of other or past characters, as well as no copies of real life.
- How will unfair advantages or inappropriate content be detected?
    - By user report mechanism, using tickets and evidence, or moderator supervision. Some external plugins may help too.
- What incentives exist for players to join nations, families, or groups?
    - Motivational or promotional content will be also created by players, however server or staff might interfere to balance out the players.

## Extensibility
- What future features (jobs, professions, wars, families) should be prioritized?
    - Families - they will be used to join in new players after the server's start.
    - Wars, professions, jobs, titles, as much as possible.
- How can the system be made modular for easy updates?
    - Hard question, it should not be modular, it should be all done before usage.

---
Add more questions as new ideas or requirements arise.

## Additional Implementation Questions

### Website UI/UX
- What specific Minecraft UI elements should be recreated (inventory slots, buttons, borders)?
- How should the character creation form be structured and what fields are required?
- What should the lore creation/editing interface look like?
- How should the economy dashboard display transactions and balances?
- What should the moderator panel interface include?

### Database Design
- How should the linking code system be implemented in the database?
- What indexes are needed for performance optimization?
- How should character history and lore versions be tracked?
- What backup and recovery procedures should be in place?

### API Design
- What rate limiting should be applied to different endpoints?
- How should API authentication tokens be managed?
- What error handling and logging should be implemented?
- How should data validation be structured for different content types?

### Discord Bot Commands
- What specific commands should be available (/character, /lore, /link, etc.)?
- How should command permissions be structured?
- What should the bot's response format look like?
- How should the bot handle errors and user feedback?

### Minecraft Plugin Integration
- How should the plugin integrate with existing town/nation plugins?
- What events should trigger API calls and data sync?
- How should the plugin handle API failures or network issues?
- What configuration options should be available to server admins?

### Security & Performance
- How should sensitive data be encrypted in the database?
- What should the API rate limiting strategy be?
- How should user sessions be managed across platforms?
- What monitoring and alerting should be in place?

### Testing & Deployment
- What testing strategy should be used for each component?
- How should the development/staging/production environments be structured?
- What CI/CD pipeline should be implemented?
- How should database migrations be handled?

### Content Moderation Details
- What specific AI prompts should be used for lore moderation?
- How should the manual review workflow be structured?
- What appeals process should exist for rejected content?
- How should repeat offenders be handled?

### Economy System
- How should initial economy seeding work?
- What transaction types should be supported?
- How should resource scarcity be managed?
- What economic reporting should be available to staff?

### Character & Lore Management
- How detailed should character creation be (stats, background, relationships)?
- What lore categories and tags should exist?
- How should lore interconnections be tracked?
- What search and filtering options should be available?