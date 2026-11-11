# macOS

The bundle. `build-app.sh` assembles `dist/Slipcase Query.app` from a release
build, substitutes `@VERSION@` and `@BUILD@` into `Info.plist.in` from
`packaging/version.sh`, and signs the bundle with the identity given on the
command line and `Slipcase Query.entitlements` beside it: the App Sandbox and
read-only access to what the person chooses, because the application reads a
folder and writes nothing. With `--universal` it joins the two
per-architecture builds, and with `--store` it builds the signed package the
Mac App Store takes. `build-app.sh --help` says which certificate each one
uses and how to sign the universal binary a release carries.

    cargo build --release
    ./packaging/macos/build-app.sh --sign "Apple Development: <Your Name> (<TEAMID>)"

`screenshot.sh`, `shots.sh` and `take-shots.sh` are the listing's screenshot
drivers, over the folder `packaging/demo-corpus.sh` builds, and
`window-probe.swift` asks the window server whether a window appeared.

A bare executable draws a window on macOS, but it is not a thing the platform
can associate anything with, so even a development build is a bundle.
`lsregister -f "dist/Slipcase Query.app"` registers it and `lsregister -u`
unregisters it; `dist/` is ignored by git and a deleted bundle otherwise
leaves a claim behind. `lsregister` is not on `PATH`, and `build-app.sh`
prints its full path.

The bundle declares no document type. The application opens a folder, which
is not a type a bundle can claim, so there is one icon and nothing to
associate. Signing, the sandbox and the Store build work as they do in
slipcase-desktop, whose `packaging/macos/README.md` is the long form.
