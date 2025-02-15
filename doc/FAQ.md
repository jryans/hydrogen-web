# FAQ

## What browsers are supported?

Internet Explorer 11, Chrome [1], Firefox [1] (not in a private window), Edge [1], Safari [1] and any mobile versions of these. It will probably also work on any derivatives of these.

1: Because of https://github.com/vector-im/hydrogen-web/issues/230, only [more recent versions](https://caniuse.com/mdn-javascript_operators_optional_chaining) are supported.

TorBrowser ships a crippled IndexedDB implementation and will not work. At some point we should support a memory store as a fallback, but that will still give a sub-par experience with end-to-end encryption.

It used work in pre-webkit Edge, to have it work on Windows Phone, but that support has probably bit-rotted as it isn't tested anymore.

## Is there a way to run the app as a desktop app?

You can install Hydrogen as a PWA using Chrome/Chromium on any platform or Edge on Windows. Gnome Web/Ephiphany also allows to "Install site as web application". There is no Electron build of Hydrogen, and there will likely be none in the near future, as Electron complicates the release process considerably. Once Hydrogen is more mature and feature complete, we might reconsider and use [Tauri](https://tauri.studio) if there are compelling use cases not possible with PWAs. For now though, we want to keep development and releasing fast and nimble ;)

## Is feature X supported?

If you can't find an easy way to locate the feature you are looking for, then the anwser is usually "no, not yet" :) But here are some things people have asked about in the past:

### How does newline work? Shift+Enter has no effect.

That's not yet a feature, as hydrogen just uses a single line text box for message input for now.

## How can I verify my session from Element?

You can only verify by comparing keys manually currently. In Element, go to your own profile in the right panel, click on the Hydrogen device and select Manually Verify by Text. The session key displayed should be the same as in the Hydrogen settings. You can't yet mark your Element session as trusted from Hydrogen.

## I want to host my own Hydrogen, how do I do that?

There are no published builds at this point. You need to checkout the version you want to build, or master if you want to run bleeding edge, and run `yarn install` and then `yarn build` in a console (and install nodejs > 14 and yarn if you haven't yet). Now you should find all the files needed to host Hydrogen in the `target/` folder, just copy them all over to your server. As always, don't host your client on the same [origin](https://web.dev/same-origin-policy/#what's-considered-same-origin) as your homeserver.

## I want to embed Hydrogen in my website, how should I do that?

There are no npm modules yet published for Hydrogen. The easiest is probably to setup your website project, do yarn/npm init if you haven't yet, then add the hydrogen repo as a git http dependency, and import the files/classes you want to use from Hydrogen.

For example, for a single room chat, you could create an instance of `Platform`, you create a new `SessionContainer` with it, call `startWithLogin` on it, observe `sessionContainer.loadStatus` to know when initial sync is done, then do `sessionContainer.session.rooms.get('roomid')` and you create a `RoomViewModel` with it and pass that to a `RoomView`. Then you call `document.appendChild(roomView.mount())` and you should see a syncing room.

Feel free to ask for pointers in #hydrogen:matrix.org as the documentation is still lacking considerably. Note that at this early, pre 1.0 stage of the project, there is no promise of API stability yet.

Also, to make end-to-end encryption work, you'll likely need some tweaks to your build system, see [this issue](https://github.com/vector-im/hydrogen-web/issues/467).
