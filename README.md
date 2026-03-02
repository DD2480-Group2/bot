# Report for assignment 4

## P+

We are aiming for P+ for this assignment. We have done P+ requirement numbers 1, 3, 4 and 6.

### Point 1
We have written an overview of the purpose of the system and architecture of the system.

### Point 3
All our tests are in a table showing which requirement ID it tests and why it is testing that requirement. 

### Point 4
Our patch is clean, it does not comment out obsolete code, produce extraneous output that is not required or add unnecessary whitespace changes.

### Point 6
We argue critically about the benefits, drawbacks, and limitations of our work carried out, in
the context of current software engineering practice (SEMAT kernel). We cover alphas other than Team and Way of Working such as Software System, Stakeholder and Opportunity. 

## Project

Name: Discord Bot 

URL: https://github.com/python-discord/bot

A Discord bot makes interacting in a channel easier. It is primarily used for the “Python Discord”-chanel, and comes with features that allows easier interactions for developers and programmers. 

## Onboarding experience

We chose to work on a new project for this assignment, since the project used in Assignment #3 did not have any Issues on Github. After checking the recommended project sheet, we initially selected JabRef, a cross-platform citation and reference management tool. However, when running the tests on a freshly cloned version of the repo, some tests failed. Since this created uncertainty about the project setup and test reliability, we decided to switch to another project.

We then selected Discord Bot as our project. We previously worked primarily with Maven-based Java projects in this course, we saw this as an opportunity to expose ourselves to a different framework. While the setup took a little longer than with a functional Maven would have, we still feel like it was worth expanding what tools we have worked with.

The onboarding process was slightly time-consuming, compared to our previous projects. Apart from cloning and building the project, we needed to set up a Discord server, create and configure a bot, and prepare the required environment to reproduce the selected issue. Although the setup involved a lot of steps, the documentation was clear and helpful. At no point did we get completely stuck, but the process required more manual oversight compared to the Maven-based project we had previously worked with. Some steps were a little outdated with new layout or logic on the site, but we were all able to set up the environment. 

## Effort spent

| Element / Member | Ahmed Baccar | Adam Egnell | Teoman Köylüoglu | Kevin Löv | Samuel Peetre |
|---------------------------------------|-------------:|------------:|-----------------:|----------:|---------------|
| plenary discussions/meetings            |    1   |     1    |   1     |  1    |    1   |
| discussions within parts of the group |     3  |     3    |     3  |  3      |    3     | 
| reading documentation                 |      4       |    4    |   2   |   4    |   2     |
| configuration and setup               |   2      |  1      |      3      |   1.5    |    2      |
| analyzing code/output                 |    3     |  2      |      2       |  3    |   2      |
| writing documentation                 |     3    |   7     |      3       |  8     |    4    |
| writing code                                 |    4     |   4     |      5       |  5   |   5     |
| running code                               |   1      |     1   |      1     |  1    |   1   |

For setting up the environment and contributing to “Bot” they have the following [guide](https://www.pythondiscord.com/pages/guides/pydis-guides/contributing/bot/).
The guide will take you through how to set up a Discord server, how to create the bot, how to start docker and how to run tests. The majority of the setup time was spent setting up the bot on docker. 

The testing also proved to be very time consuming, often requiring us to manually test the bots commands on each other in the discord server. The temporary voiceban took extra long since we had to wait for the voiceban duration to pass. 

## Overview of issue(s) and work done.

Title: Create new voice-ban command and change the current voice-ban's name to voice-mute

URL: https://github.com/python-discord/bot/issues/1849

A moderator will be able to write the command “voiceban @[user]” to ban a user from joining voice channels. The voiceban command has an option to specify a duration of time the user will be banned. A separate command “tempvoiceban” is the same but needs a specified duration. The voice ban will also prevent users from becoming voice verified for the remaining duration of the voice ban. Lastly, an “unvoiceban” command will remove any voice ban of a given user.

### Scope
The scope affected multiple files, the most focus being on infractions.py where we added multiple functions to correctly implement voiceban, temporary voiceban and manual removal of voiceban similarly to how all other commands were previously implemented. Since the voiceban should prevent users from being voice verified, we had to implement that in the voice_gate.py file with the code for voice verification. Additionally, adding a new infraction and a new role forced us to add new elements to the files constants.py and infraction/_utils.py for the voiceban infraction to be recognised and be applied by the function. So the issue was non-trivial. 

## Requirements for the new feature or requirements affected by functionality being refactored

| ID | Title | Description |
|--: | --: | --: |
| 1001 | Implement voiceban command | Command should ban users from joining voice chat channels. If duration is specified, it temporarily bans the user from joining voice chat for the given period |
| 1002 | voiceban will prevent users from being voice verified | If a user is voicebanned, they should not be able to get voice verified for as long as the voice ban is active |
| 1003 | Implement unvoiceban command | Command should remove the voiceban infraction of users so that they can join voice channels again |
| 1004 | Command adheres to hierarchical structure | A user should only be able to ban a user that is lower in the hierarchical structure |
| 1005 | test the new commands | test the newly implemented commands (requirements 1001-1004) |

### tests traced to requirements
All our tests are traced to the requirement ID it tests and a short description of why it is traced to that requirement.

| ID | Test | Why|
|--: | --: | --: |
| 1001, 1005 | test_permanent_voice_ban | Verifies that the voiceban() method calls apply_voice_ban() |
| 1003, 1005 | test_voice_unban | Verifies that the unvoiceban() method calls pardon_infraction() |
| 1003, 1005 | test_voice_unban_reasonless | Verifies that the unvoiceban() method calls pardon_infraction(), even when reason is not passed as argument |
| 1001, 1005 | test_voice_ban_user_have_active_infraction | Verifies that apply_voice_ban() returns early when an active voice_ban infraction exists, and does not call post_infraction(). |
| 1001, 1005 | test_voice_ban_infraction_post_failed | Verifies that apply_voice_ban() does not voice ban when infraction fail |
| 1001, 1005 | test_voice_ban_infraction_post_add_kwargs | Verifies that apply_voice_ban() forwards extra keywords when they are passed to the method |
| 1003, 1005 | test_voice_unban_user_not_found | Verifies that pardon_voice_ban() handles when no user is found in the server, with info message |
| 1003, 1005 | test_voice_unban_user_found | Verifies that  pardon_voice_ban() notifies a user of them being pardoned and that a dict is returned when succeeding. |
| 1003, 1005 | test_voice_unban_dm_fail | Verifies that pardon_voice_ban() still returns a log dict even when notification to the user fails. |
| 1004, 1005 | test_voice_ban_blocks_if_user_equal_me | Verifies that apply_voice_ban() does not succeed in applying infraction when target role is same as bot role. |
| 1004, 1005 | test_voice_ban_blocks_if_user_above_me | Verifies that apply_voice_ban() does not succeed in applying infraction when target role is above bot role |
| 1004, 1005 | test_voice_ban_allows_if_user_below_me | Verifies that apply_voice_ban() succeed when applying infraction when target role is lower than bot role |
| 1001, 1005 | test_apply_infraction_when_no_previous_infraction_and_reason_is_given | Verifies that post_infraction() calls apply_voice_ban and apply_infraction() when no active infractions exist |
| 1001, 1005 | test_voice_ban_action_disconnects_and_adds_role | Verifies that when an infraction succeeds to be posted to a user they are disconnected from any voice channel |
| 1001, 1005 | test_voice_ban_action_does_not_disconnect_but_adds_role | Verifies that when an infraction succeeds to be posted to a user, if they are not in a voice channel, they are not disconnected from the server, but still get the infraction. |

## Code changes

### Patch

The patch is accessible in the ```voiceban_with_main.patch``` file in root. 

The patch is clean, it does not comment out obsolete code, produce extraneous output that is not required or add unnecessary whitespace changes. 

## Test results

All our tests pass. The only test that is skipped was also skipped before we implemented our code and tests. 

testlog before: ```testlog_clean_before.txt``` in root
testlog after: ```testlog_after_voiceban.txt``` in root


## UML class diagram and its description
<img width="1198" height="588" alt="image" src="https://github.com/user-attachments/assets/7561e583-b732-4555-b76a-a203c89a7d46" />

### Key changes/classes affected
The key changes that have been made are in the Infractions class, where functionality was expanded by implementing the voiceban function, and all functions that relate to it. This means adding unvoiceban, appply_voice_ban (which follows the architecture of how they implement commands), pardon_voice_ban and _pardon_action (which also follow how the architecture wants the structure of the code to be.













### Purpose of the system
The system in question is the official community bot for the Python Discord community. It is written in Python and built on top of a wrapper on the Discord API. The bot provides a wide variety of features, such as warn, kick, timeout, ban, etc. In other words, the purpose of the system is to automate moderation, provide community utilities and assist the Python Discord community with daily tasks that involve the channel, by providing the users of the Python community discord channel to make use of this bot through an easy interface. To better understand the purpose of the system, it is wise to look at its features. More specifically, the core functions can be reduced to (1) moderation/infractions; (2) community help; (3) content filtering; (4) information (community services); (5) recreation; (6) recruitment (talent pool)
As such, it serves the specific needs of the Python Discord community.

The main problem that this system seeks to address seems to be scaling and consistency. Seeing as the community is quite large, and a lot of users are engaged in the community, it is important to be able to serve all users in a consistent and reliable way. Without automation, enforcing rules, managing timeouts and temporary punishments would be next to impossible. As a result, the purpose becomes two-fold; it seeks to reduce operational complexity and manual labor, by increasing developer productivity through the very programming language the community is composed of. 

### Architecture of the system
The overarching architecture of the system is detailed in the following diagram.

<img width="1418" height="394" alt="image" src="https://github.com/user-attachments/assets/6338a439-5ae0-4c39-9604-0a3f8f036c6a" />

The communication between Discord and the bot is event-driven. When the program starts, its main entry is the main.py. It configures all of the necessary services, i.e., HTTP session, redis, API client etc. The Discord bot registers itself to the server (discord channel) and receives events via the Discord API. As events are dispatched by the Discord library, the implemented event listeners (on a specific command) in bot’s extensions (see diagram) are called and perform their given command logic. Some of these command handlers may in turn dispatch Actions, for example when banning a user, or removing them from the voice channel.

The features of the bot are modularized as Cogs under the extensions of the bot directory. A Cog is a Python class used to modularize code related to commands, listeners and states into one class. As such, a class becomes a subclass of the Cog and implements the decorators such as commands.command(), commands.Cog.listener(), etc. In this way, each class becomes a module that is grouped by feature, and each Cog encapsulates a specific concern, such as infractions, management, filtering, help forum etc. This separation of concerns makes it easier to extend additional functionality, resulting in a clean architecture for plugins.

Although it is difficult to place the architecture within well known architectures such as MVC, MVP, etc, the architecture is however a layered one. Via looser categorization, it is easy to see that there is a bootstrapping layer responsible for wiring the app and starting the app. This is found in main.py in the bot/ directory as noted above where the HTTP session, redis, and the bot etc are constructed. An application layer would similarly lie in the bot.py file of the bot/ directory, where the actual Bot class’ behavior is defined. Moreover, there are utilities, constants etc that are defined in the project, and these can be said to lie in the configuration or utility layer. As for the specific commands and Actions etc, these can be said to be in the Feature layer as they define the core features and interfaces that the bot implements.

## Overall experience

### What we learned
We learned to write code for large projects in python which was new for us on this scale since we programmed in java in the previous assignment. We also learned some specific libraries like discord.py and Cog. for discord.py we learned the basics of programming a discord bot and the built-in functions that exist, and for Cog we had to understand the different functions it uses to make a function run during startup. We previously used Maven with automated JUnit testing, but for this project we had to learn how to use pytests with UV.

### Discord community
Since this repository is for a discord bot on a specific server, we joined the server and notified them that we were interested in doing this issue. The moderators were all friendly and quick to respond by assigning us to the issue. They quickly answered any questions we had regarding implementation. We assume that other open source projects are slower with their communication so we felt lucky to have chosen this with its own discord community. 

### Essence standard
From a SEMAT perspective, working on this issue regressed our progress on the alpha “Way of working” since we changed from Java and its frameworks, where we were in the “in place” state, to python with a lot of new frameworks. While this was a setback in process maturity, we viewed it as a strategic trade-off for professional growth through trying different frameworks. Contributing still let us reach the “in use” state of the alpha where we got firsthand experience of how professional teams maintain python code at scale.

For the “Software System” alpha, contributing to this large, live codebase allowed us to get a firsthand experience of the “Operational” state. We looked through and improved the code of a system that currently serves thousands of users. This gave us practical insights into the complexities of sustaining a live production environment.

We progressed a lot in the “Stakeholder” alpha, all the way to the “In agreement” state. The discord bot is for the python-discord server where all the members are python programmers and many contribute to the same project. This resulted in us using their own communication platform leading to seamless communication with capable python programmers as well as moderators of the open source project. 

In regards to the “Opportunity” alpha, our work was limited. As an outside contributor, we only do issues that others deem important and we rarely identify improvement opportunities ourselves. Due to the nature of the assignment and us not being familiar with the source code, we are limited to tactical fixes rather than architectural shifts. 

