# Push200

Get a phone app from its GitHub code into App Store Connect and Google Play, without writing code or owning a Mac. This plugin gives Claude the Push200 connector and a skill that walks you from "I have an app" to "it's in the stores".

[Push200](https://push200.com) builds apps made with Expo, Capacitor, React Native and Flutter. React Native (without Expo) and Flutter support are new.

## What's inside

- **The Push200 connector** (`https://push200.com/api/mcp`): tools Claude can call.
- **The `release-app-with-push200` skill**: how to use those tools well, in plain words.

## Use it

Without signing in, ask things like:

- "How does Push200 work, and what do I need before I start?"
- "Can Push200 build my Flutter app?"
- "What does Push200 cost?"

After you connect your Push200 account, ask things like:

- "What do my apps still need before I can build?"
- "Build Corner Bakery for iPhone."
- "How did my last build go? Why did it fail?"
- "Cancel the build that's running."

The first time Claude needs your account it shows a Connect button. You sign in on Push200's own page and approve there. You never type a password into the conversation. You can disconnect at any time on your Push200 Account page, under Connected AI assistants.

Starting a build is real: it uses your own GitHub build minutes, counts against your plan's builds for the day, and sends your app to the stores. Claude asks before it starts or cancels one.

## What it can and can't do

It can list your apps and what each still needs, list your builds and explain how each went, start a build, and cancel a running build. The same rules and plan limits as the Push200 website apply.

It can't add or change apps, change settings or billing, or see, accept or handle store keys. That setup is done on the Push200 website. Push200 itself holds no store keys: they stay in your own GitHub.

## Data

The plugin stores nothing itself and runs no code on your computer. It contains only this text, the skill and the connector's address.

When Claude calls a tool, the request goes to `push200.com` and nowhere else. The public tools send nothing about you. The account tools send your connector token and the app or build you asked about, and return your apps' names, IDs, repository names, setup state and build results. They never return store keys, passwords or payment details.

Privacy policy: https://push200.com/privacy · Terms: https://push200.com/terms · Support: contact@push200.com
