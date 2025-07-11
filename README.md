# KingdomProjects

**KingdomProjects** is a collaborative Minecraft plugin for the [Theatria](https://playtheatria.com) server, built on the Paper API. It allows players to propose, accept, complete, or cancel shared in-game projects with an escrow-based reward system. Players can create project offers, track progress, and handle approval and cancellation flows, with real-time and login-time notifications.

---

## Features

* Create and describe long-form collaborative projects
* Escrow system: safely hold and transfer funds via Vault
* Offer and accept projects between players
* Real-time or deferred notifications for online/offline users
* View active, completed, and canceled projects
* Completion and cancellation approval flows
* Fair, transparent project lifecycle with audit trail

---

## Project Lifecycle

### 1. Create a Project

```bash
/project create <project_name> <long_description>
```

* Initiated by the offering player (User A)
* Creates a draft project with description and owner

### 2. Offer the Project

```bash
/project <project_name> offer <username>
```

* Sends a project proposal to recipient player (User B)
* Funds from User A are placed into escrow
* Notification is delivered immediately or on next login

Example:

```
Zarathale offers you $1,000 for the following project:
"Build a villager trading hall near Smittiville with redstone-powered sorting."

Type /project tradinghall accept or /project tradinghall reject
```

### 3. Respond to Offer

```bash
/project <project_name> accept
/project <project_name> reject
```

* Accepting activates the project and makes it publicly visible
* Rejecting returns funds from escrow to User A
* User A receives a notification when B responds

### 4. View Project Lists

```bash
/projects server
/projects mine
```

* `server` lists all accepted, completed, and canceled projects
* `mine` filters to the current player's involvement

### 5. Cancel a Project (request)

```bash
/project <project_name> cancel
```

* Sends a cancel request to the other player
* They must approve for cancellation to take effect

Notification example:

```
Zarathale has requested to cancel the following project: "tradinghall".
Type /project tradinghall approve or /project tradinghall deny
```

### 6. Mark Project as Complete

```bash
/project <project_name> complete
```

* Signals readiness for final approval and payout
* Recipient player must respond:

```bash
/project <project_name> approve
/project <project_name> deny
```

* Approval → Funds released from escrow to User B
* Denial → Project remains active, with a dispute status tracked

---

## Plugin Structure

```
KingdomProjects/
├── src/main/java/com/theatria/kingdomprojects/
│   ├── KingdomProjects.java
│   ├── config/
│   │   └── PluginConfig.java
│   ├── command/
│   │   ├── ProjectCommand.java
│   │   └── Subcommands/
│   │       ├── CreateSubcommand.java
│   │       ├── OfferSubcommand.java
│   │       ├── AcceptSubcommand.java
│   │       ├── RejectSubcommand.java
│   │       ├── CancelSubcommand.java
│   │       ├── CompleteSubcommand.java
│   │       ├── ApproveSubcommand.java
│   │       ├── DenySubcommand.java
│   │       └── ListSubcommand.java
│   ├── model/
│   │   ├── Project.java
│   │   ├── ProjectStatus.java
│   │   └── ProjectStore.java
│   ├── notification/
│   │   ├── NotificationManager.java
│   │   └── AlertType.java
│   └── util/
│       └── MessageUtils.java
├── resources/
│   ├── config.yml
│   └── lang.yml
└── plugin.yml
```

---

## Economy Integration

* Vault is used to handle all monetary transactions
* On `offer`, funds are withdrawn from User A and held by the plugin
* On `approve`, funds are paid to User B
* On `reject` or `cancel`, funds return to User A
* Configurable currency symbol and minimum project amounts in `config.yml`

---

## Notification System

Notifications are sent via:

* Real-time chat messages for online users
* Queued delivery on next login for offline users

All messages are customizable via `lang.yml`.

---

## Requirements

* PaperMC 1.20+
* Java 17+
* Vault (for economy handling)
* A compatible economy plugin (e.g. EssentialsX)

---

## Roadmap

* [ ] Stub all core commands and registration
* [ ] Implement Vault escrow handling
* [ ] Store and serialize project data
* [ ] Build login queue notification system
* [ ] Add `/projects server` filtering and sorting
* [ ] Optional: GUI menu for project listings
* [ ] Optional: Webhook or audit logging for admin use

---

## About

This plugin is part of the Kingdom\* plugin series for the Theatria server. See also:

* KingdomBank
* KingdomLoan

---

## License

MIT License – open for community expansion and customization within Theatria or similar servers.
