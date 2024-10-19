# Bazel iOS Codesigning Bug Reproduction

This repo is a reproduction of a bug in Bazel where the codesigning step fails for an iOS app. This reproduction
was run using Bazel 7.3.2 and Xcode 16.0 (16A242d).

If `Entitlements.entitlements` contains no entitlements, XCode will generate a code signing profile named
`iOS Team Provisioning Profile: *`. When the xcode project is built using `bazel run //:xcodeproj`, this
provisioning profile will work and be able to sign the app.

When the entitlements file contains an entitlement, XCode will generate a code signing profile named
`iOS Team Provisioning Profile: APP_BUNDLE_ID`.In this case, when the xcode project is generated using
the new provisioning profile will be used to sign the app, however attempting to run the app will generate an 
error:

`Failed to verify code signature of /var/installd/Library/Caches/com.apple.mobile.installd.staging/temp.0SjU0G/extracted/TestBazel.app : 0xe8008015 (A valid provisioning profile for this executable was not found.)`
