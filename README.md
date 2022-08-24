# expo-community-flipper

Flipper Support for Expo Apps

# Usage (Quick Guide)

**1.** Install the module along with [react-native-flipper](https://www.npmjs.com/package/react-native-flipper): `yarn add expo-community-flipper react-native-flipper`

**2.** Add `expo-community-flipper` configuration to the `plugins` section of your `app.json`, as per the examples below. You have the option to specify the version of Flipper and the pods that you want to use. In React Native, the Android SDK is derived from the main Flipper configuration.

If you don't specify anything, `expo-community-flipper` will default to the version of Flipper bundled with the version of React Native you're currently using.

Not sure what Flipper version you need? [Check the Official Flipper Podfile](https://github.com/facebook/flipper/blob/main/react-native/ReactNativeFlipperExample/ios/Podfile) if you are specifying all of Flipper's pods, or the latest version of [react-native-flipper](https://www.npmjs.com/package/react-native-flipper) if you are just specifying a flipper version.

# Configuration

## Disabling Flipper in CI (>= 45.1.0)

To disable Flipper in CI builds, set the env value of `FLIPPER_DISABLE` to `1`. We specifically do not use the `CI` env value, as EAS identifies itself as a CI environment which would remove the ability to generate a development client containing flipper. Instead, the ENV value should be provided for your production builds.

## Flipper (React Native Default Version)

```json
{
  "expo": {
    "..."
    "plugins": [
      "expo-community-flipper"
    ]
  }
}
```

## Flipper with a specific version

```json
{
  "expo": {
    "..."
    "plugins": [
      ["expo-community-flipper", "0.123.0"]
    ]
  }
}
```

## Flipper with all Pod dependencies included

_note: Android uses the same version as specified in the primary `Flipper` pod and does not require additional configuration_

```json
{
  "expo": {
    "..."
    "plugins": [
      ["expo-community-flipper", {
        "Flipper": "0.123.0",
        "ios": {
          "Flipper-Folly": "2.6.10",
          "Flipper-RSocket": "1.4.3",
          "Flipper-DoubleConversion": "3.1.7",
          "Flipper-Glog": "0.3.9",
          "Flipper-PeerTalk": "0.0.4"
        }
      }]
    ]
  }
}
```

# Windows Users + Hermes

> As of right now, using Windows with the Hermes engine requires you to run your app inside of a WSL environment. [The tracked issue is here](https://github.com/jakobo/expo-community-flipper/issues/4) and if you have a `Podfile`, please let me know. It's likely an upstream issue, but we need a Podfile to confirm.

# Verified Versions

The following Flipper versions were verified against EAS. If you have another working combination, open a ticket or PR. Thank you!

| Expo SDK Version | Flipper                      |
| :--------------- | :--------------------------- |
| 46               | 0.161.0, builtin (rn 0.69.4)
| 45               | 0.123.0, builtin (rn 0.68.1) |
| 44               | 0.123.0                      |
| 43               | 0.123.0                      |

(note, we follow [expo's policy for the deprecation of old SDKs](https://docs.expo.dev/workflow/upgrading-expo-sdk-walkthrough/))

# Testing

An `/example` directory is built with `expo init example` for each major SDK release with a default `eas.json` file. The plugin is directly linked using expo's filepath support for config plugins. You can run `expo prebuild` in the directory to verify the plugin is modifying build files appropriately.

**example eas.json**

```json
{
  "cli": {
    "version": ">= 0.35.0"
  },
  "build": {
    "example": {
      "releaseChannel": "default",
      "channel": "default"
    }
  }
}
```

**example app.json**

```json
{
  "expo": {
    "...": "... standard app.json entries ...",
    "plugins": [
      [
        "./../build/withFlipper",
        {
          "Flipper": "0.123.0",
          "ios": {
            "Flipper-Folly": "2.6.10",
            "Flipper-RSocket": "1.4.3",
            "Flipper-DoubleConversion": "3.1.7",
            "Flipper-Glog": "0.3.9",
            "Flipper-PeerTalk": "0.0.4"
          }
        }
      ]
    ]
  }
}
```

# References

- This code is based on the [Flipper Getting Started Guide](https://fbflipper.com/docs/getting-started/react-native/)
- [Expo Config Plugins](https://docs.expo.dev/guides/config-plugins/)
