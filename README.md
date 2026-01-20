# Community Cordova ProGuard Plugin

A Cordova plugin that activates ProGuard/R8 and minification for Android builds in your Cordova mobile application.

## Support This Plugin

I dedicate a considerable amount of my free time to developing and maintaining many Cordova plugins for the community ([See the list with all my maintained plugins][community_plugins]).

To help ensure this plugin is kept updated, new features are added and bugfixes are implemented quickly, please donate a couple of dollars (or a little more if you can stretch) as this will help me to afford to dedicate time to its maintenance.

Please consider donating if you're using this plugin in an app that makes you money, or if you're asking for new features or priority bug fixes. Thank you!

[![Sponsor Me](https://img.shields.io/static/v1?label=Sponsor%20Me&style=for-the-badge&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/eyalin)

## Credits

This plugin is based on [cordova-plugin-proguard](https://github.com/greybax/cordova-plugin-proguard) by [Aleksandr Filatov](https://alfilatov.com). We are grateful for the original work and contributions.

## Why This Plugin?

The original [cordova-plugin-proguard](https://github.com/greybax/cordova-plugin-proguard) is no longer actively maintained. Many Cordova developers face issues with:

- Outdated ProGuard rules that don't work with modern libraries
- Missing rules for Firebase, AdMob, and other commonly used SDKs
- Compatibility issues with newer versions of Android and Gradle
- Lack of updates to address R8 (the new default code shrinker in Android)

This **community-maintained** fork aims to:

- Provide up-to-date ProGuard/R8 rules for modern Android development
- Include pre-configured rules for popular libraries (Firebase, AdMob, Google Play Services, etc.)
- Ensure compatibility with the latest Android Gradle Plugin versions
- Actively maintain and fix issues reported by the community

## What is ProGuard/R8?

ProGuard is the open source optimizer for Java bytecode. R8 is Google's replacement for ProGuard that is now the default in Android builds.

You can read more about it on:
- [ProGuard official website](https://www.guardsquare.com/en/proguard)
- [Android developer portal](https://developer.android.com/studio/build/shrink-code.html)

## Features

- Automatically enables code shrinking and obfuscation for release builds
- Includes pre-configured ProGuard rules for Cordova applications
- Supports custom ProGuard rules via project-level configuration
- Compatible with Firebase, Google Play Services, AdMob, and other common libraries
- Updated rules for R8 compatibility

## Installation

```bash
cordova plugin add community-cordova-plugin-proguard
```

Or from local path:

```bash
cordova plugin add /path/to/community-cordova-plugin-proguard
```

This will:
1. Configure your `build.gradle` file
2. Copy `proguard-custom.txt` to `${androidPlatformDirectory}/assets/www/proguard-custom.txt`

## Customization

### Adding Custom Rules

If you want to add custom ProGuard rules, create a `proguard-custom.txt` file in your project root folder. When installing the plugin, these rules will be automatically appended to the plugin's default rules.

```bash
# Create your custom rules file
touch proguard-custom.txt
```

Then add your rules:

```proguard
# Example: Keep a specific class
-keep class com.mycompany.myapp.** { *; }

# Example: Suppress warnings for a library
-dontwarn com.somelibrary.**
```

After modifying rules, re-add the platform:

```bash
cordova platform rm android
cordova platform add android
```

### Default Rules

The plugin includes sensible default rules for:

- Cordova core classes
- Google Play Services
- Firebase (Analytics, Crashlytics)
- AdMob
- SQLite plugins
- Ionic WebView
- Facebook SDK
- User Messaging Platform (UMP/Consent)
- And more...

## Build Configuration

The plugin configures your build.gradle with the following settings:

**Debug builds:**
- `minifyEnabled: false` (no obfuscation for easier debugging)

**Release builds:**
- `minifyEnabled: true` (full obfuscation and shrinking)

## Platform Support

- Android

## Troubleshooting

### App crashes after enabling ProGuard

If your app crashes with `ClassNotFoundException` or similar errors, you may need to add keep rules for the affected classes. Check your crash logs and add appropriate `-keep` rules to your custom `proguard-custom.txt`.

### Missing methods at runtime

Some libraries use reflection which ProGuard cannot detect. Add keep rules for these classes:

```proguard
-keep class com.example.MyReflectedClass { *; }
```

### JNI/Native method issues

If you're using plugins with native code (like SQLite), make sure native methods are preserved:

```proguard
-keepclasseswithmembers class * {
    native <methods>;
}
```

## Contributing

- Star this repository
- Open issue for feature requests
- [Sponsor this project](https://github.com/sponsors/eyalin)

## License

This project is [MIT licensed](LICENSE).

[community_plugins]: https://github.com/EYALIN?tab=repositories&q=community&type=&language=&sort=
