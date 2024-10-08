---
description: Making use of the cross-cutting Plugin API for invoking various capabilities
---

# Using Plugins

Plugins as discussed in the [Features and Plugins](../concepts/features-and-plugins.md) article, is all about cross-cutting functionality that is available to all features. Think of it as the Frontend brain for the User-facing feature. The classic example is the Authentication plugin. It allows you to invoke auth operations like `login`, `logout` without worrying about the UI. The authentication feature provides the UI and invokes the **auth** plugin (via **`vyuh.auth`**) as per the User events.

This separation of Plugins into their own space allows features to focus more on the presentation and less on the plumbing used to achieve the user-facing functionality.

## Setting Up Plugins

The Vyuh framework has built in plugins for most of the common scenarios. This helps you to get started quickly without configuring all of them at the beginning. However, as you start building more complex apps, it would be required to setup custom plugins and integrations that are suited for your app.

This is done as part of the **`runApp()`** call, from the **`vyuh_core`** package. Here is an example of the invocation with the various plugins.

{% code title="lib/main.dart" lineNumbers="true" %}
```dart
void main() async {
  final plugins = await _getPlugins();

  vc.runApp(
    initialLocation: '/demo',
    features: () => [
      system.feature,
      news.feature,
      developer.feature,
      tmdb.feature,
      food.feature,
      onboarding.feature,
      wonderous.feature,
      firebase_auth.feature(),
      misc.feature,
      forms.feature,
    ],
    plugins: plugins,
    ),
  );
}

_getPlugins() async {
  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);

  // Ensure all imperatively navigated URLs are shown in the URL bar
  DefaultNavigationPlugin.enableURLReflectsImperativeAPIs();
  DefaultNavigationPlugin.usePathStrategy();

  return PluginDescriptor(
    content: DefaultContentPlugin(
      provider: SanityContentProvider.withConfig(
        config: SanityConfig(
          projectId: '<project-id>',
          dataset: 'production',
          perspective: Perspective.previewDrafts,
          useCdn: false,
          token: '<token>',
        ),
        cacheDuration: const Duration(seconds: 5),
      ),
    ),
    analytics: vc.AnalyticsPlugin(
      providers: [
        FirebaseAnalyticsProvider(),
        SentryAnalyticsProvider(
          config: SentryProviderConfig(
            sampleRate: 0.2,
            dsn: '<dsn>',
          ),
        ),
      ],
    ),
    auth: FirebaseAuthPlugin(),
    others: [
      vc.ConsoleLoggerPlugin(),
      FirebaseFeatureFlagPlugin(
        settings: RemoteConfigSettings(
          fetchTimeout: const Duration(seconds: 10),
          minimumFetchInterval: const Duration(seconds: 10),
        ),
      ),
    ],
  );
}

```
{% endcode %}

In the above code-block, we are using a variety of custom configured plugins:

* **Content**: configured with the Sanity provider
* **Navigation**: using the GoRouter plugin
* **Analytics**: uses Firebase Analytics, Performance and Crashlytics
* **Feature Flag**: using Firebase Remote Config
* **Logger**: the built-in console logger
* **Authentication**: using Firebase Auth

## Available Plugins

There are many built-in plugins available in the Open Source and many more are being added for the Enterprise plans. These plugins fall into the following categories:

* Content
* Network
* Dependency Injection
* Storage
* Secure Storage
* Authentication
* Analytics
* Feature Flag
* Logger
* Ads
* Notifications

We also plan to introduce new categories in the future that caters to simplifying some aspect of the Application. An example could be _User Feedback_.



