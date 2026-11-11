---
name: release-app-with-push200
description: Walk someone from "I have an app" to "it's in the stores" with Push200. Use when the user wants to publish, release, build or upload a phone app to the App Store, TestFlight or Google Play, asks whether Push200 can build their Expo, Capacitor, React Native or Flutter app, asks what Push200 costs or needs, or asks how their Push200 build went or why it failed.
---

# Release an app with Push200

Push200 builds a phone app from its GitHub repository and sends it to App Store Connect (TestFlight) and Google Play. Its customers are mostly people who don't write code, so use plain words: say "iPhone" and "Android", say what to press and where, and avoid terms like CI, pipeline, signing certificate or workflow unless the user uses them first.

Everything here is done with the Push200 connector's tools. Take facts about Push200 from the tools, not from memory, because prices, limits and supported app kinds change.

## Work out where the person is

Ask at most one short question if it isn't clear, then follow the matching part below.

- **Just curious, or deciding** (no account yet): see "Before they sign up".
- **Has an account, not built yet**: see "Getting an app ready".
- **Wants a release now**: see "Starting a build".
- **Asking about a build**: see "Explaining a build".

## Before they sign up

These four tools need no sign-in. Call the one that answers the question rather than all of them.

- `how_push200_works`: what it is, the steps, where builds run, who holds the store keys, the sign-up link.
- `list_supported_app_kinds`: whether their kind of app can be built. Pass on honestly which kinds are new. If they don't know what their app is made with, tell them they can connect its GitHub repository and Push200 will work it out, or they can ask whoever built it.
- `get_requirements`: what to have first. Only the store they want is needed: iPhone alone needs only an Apple Developer account, Android alone needs only a Google Play Console account. No Mac is needed.
- `get_pricing`: plans and limits. Always mention that build minutes are separate: they run on the person's own GitHub account and GitHub bills them, not Push200.

End with the sign-up link the tool gives. Don't promise review times or approval by Apple or Google: Push200 sends the app to the stores, and the stores decide the rest.

## Getting an app ready

Setup happens on the Push200 website, not in the conversation. This connector cannot add an app, change its settings, or add store keys, so your job is to say what is left and send the person to the right page.

1. Call `list_my_apps`. The first time, Claude shows a Connect button and the person approves access on Push200's own page. They never type a password into the conversation.
2. If there are no apps, give them the link from the tool's note to add one on the website. They connect GitHub there and pick the repository.
3. For each app, read `needs` and `setupStepsLeft`:
   - `needs` is what stops a build. If it is empty, `readyToBuild` is true.
   - `setupStepsLeft` can also list things that don't stop a build, such as connecting Google Play. Explain the difference the tool describes instead of treating every item as a blocker.
4. Tell them the next single step in plain words and give the app's `setupLink`. One step at a time works better than the whole list.

**Never ask for, accept or repeat a store key, password, token, certificate or key file.** Push200 itself holds no store keys: they go only into the customer's own GitHub, through the website. If someone pastes one, tell them not to share it in a conversation, that you haven't used it, and that they should add it on the app's page instead. If the key may have been exposed, suggest they replace it with a new one at Apple or Google.

## Starting a build

`start_build` is real. It adds a commit to the person's GitHub repository, uses their GitHub build minutes, counts against their plan's builds for the day, and sends the result to the stores.

1. Call `list_my_apps` if you don't already have the app's ID from this conversation.
2. Confirm before starting, in one sentence: which app, and iPhone, Android or both. If they only said "build my app" and have several apps, or the app is set up for both stores, ask.
3. Call `start_build` with the app's `id` and `platform` (`ios`, `android` or `both`).
4. If it answers with a reason instead of a build, pass that reason on as it is and give the link it includes. Common reasons: the app still needs a setup step, the app is already building, or the plan's limit for the day or for builds at the same time is reached. Don't retry in a loop, and don't suggest workarounds for plan limits other than waiting or upgrading (`get_pricing`).
5. If it starts, say so, give the build's link, and say a build usually takes several minutes. Offer to check on it. Don't call `get_build_status` again and again while they wait: check when they ask, or once after a few minutes if they asked you to follow it.

To stop a build that is still running, confirm which one and call `cancel_build`. Say that anything already sent to a store stays there.

## Explaining a build

1. If you don't have the build's ID, call `list_recent_builds` (with `app_id` when they named an app) and pick the build they mean. The newest is first.
2. Call `get_build_status` and explain it in this order:
   - Is it still running? Then say what it is doing now (`doingNow`) and roughly how far along it is.
   - Did each part work? `parts` has one entry for iPhone and one for Android. One can pass while the other fails, so say each separately.
   - If something failed, lead with `problem` when there is one: what happened, what to do, and the fix link. Otherwise use the failed step's `whatHappened` and `whatToDo`.
   - `errorLine` is the build's own error text. Quote it only if there is no plainer explanation, and say it is something to pass to whoever built the app.
   - Mention `warnings` and a `phoneTestProblem` briefly: the app was still built, but something needs a look.
3. Give the build's `link` so they can see the full page.

Some failures are in the app's own code, not in Push200's setup. Say so plainly, and suggest they send the build's link to whoever built the app. Don't guess at code fixes you can't see.

After a build passes: the iPhone app is in TestFlight in App Store Connect, and the Android app is in Google Play (or is a file to download, if Google Play isn't connected). Releasing it to the public is a separate step the person does in each store. Push200's guide for that is at https://push200.com/go-public.

## If something isn't covered

Point the person to the setup guide at https://push200.com/guide, or to Push200's support at contact@push200.com with a link to the build. Don't invent features: if a tool doesn't offer it, this connector can't do it, and the website may or may not.
