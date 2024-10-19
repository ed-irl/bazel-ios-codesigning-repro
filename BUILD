load("@build_bazel_rules_apple//apple:apple.bzl", "local_provisioning_profile")
load("@build_bazel_rules_apple//apple:ios.bzl", "ios_application")
load("@build_bazel_rules_swift//swift:swift.bzl", "swift_library")
load("@rules_xcodeproj//xcodeproj:defs.bzl", "xcode_provisioning_profile", "xcodeproj", "top_level_target")

APP_BUNDLE_ID = "llc.irl.TestBazelApp" # Change this to an app ID you have access to
TEAMID = "ABCXYZ" # Change this to your team ID

xcodeproj(
    name = "xcodeproj",
    top_level_targets = [
        top_level_target(
            ":TestBazelApp",
            target_environments = [
                "device",
                "simulator",
            ],
        ),
    ],
)

config_setting(
    name = "release_build",
    values = {
        "compilation_mode": "opt",
    },
)

config_setting(
    name = "device_build",
    values = {
        "cpu": "ios_arm64",
    },
)

ios_application(
    name = "TestBazelApp",
    bundle_id = APP_BUNDLE_ID,
    bundle_name = "TestBazel",
    entitlements = ":entitlements",
    families = ["iphone"],
    infoplists = [":Info.plist"],
    minimum_os_version = "15.0",
    provisioning_profile = ":xcode_profile",
    visibility = [
        "@rules_xcodeproj//xcodeproj:generated",
    ],
    deps = [":TestBazelApp.library"],
)

genrule(
    name = "entitlements",
    srcs = ["Entitlements.entitlements"],
    outs = ["Entitlements.withbundleid.plist"],
    cmd = """
sed \
  -e 's/APP_BUNDLE_ID/{}/g' \
  -e 's/TEAMID/{}/g' $< > $@
""".format(
        APP_BUNDLE_ID,
        TEAMID,
    ),
)

xcode_provisioning_profile(
    name = "xcode_profile",
    managed_by_xcode = True,
    provisioning_profile = ":xcode_managed_profile",
    tags = ["manual"],
)

local_provisioning_profile(
    name = "xcode_managed_profile",
    profile_name = "iOS Team Provisioning Profile: *",
    # Once some entitlements are added, XCode will generate a different profile with the app bundle ID in the name. Comment
    # out the line above and uncomment the line below to reproduce the failure.
    # profile_name = "iOS Team Provisioning Profile: {}".format(APP_BUNDLE_ID),
    tags = ["manual"],
    team_id = TEAMID,
)

swift_library(
    name = "TestBazelApp.library",
    srcs = glob(["**/*.swift"]),
    module_name = "TestBazelApp",
    tags = ["manual"],
)
