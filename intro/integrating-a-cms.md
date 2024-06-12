---
description: Simple integration of Vyuh with Sanity.io, the default CMS
---

# Integrating the CMS

Now that we know how to setup a feature in Vyuh, let's see how we can integrate it with a CMS. We will be using [Sanity.io](https://sanity.io) for this example, which is also our default supported CMS.

## 1. Setup the Sanity CMS

Sanity is a headless CMS that allows you to define your own schemas and content types, entirely in code. This makes it easy to quickly build sophisticated schemas without wrestling with any Drag-n-Drop builders. It also sports a **Studio** which becomes the go-to UI tool for content editors. In this section, we will set up the studio and use the Vyuh Framework to build a simple blog schema and its corresponding widget.

### Scaffold the project

Setup a _free_ Sanity project using the [guides from Sanity.io](https://www.sanity.io/get-started?ref=hero) or use the command line shown below:

```bash
pnpm dlx create-sanity@latest
```

While creating the sanity project, select the project template as `Clean project with no predefined schema types`

{% hint style="warning" %}
If you have subscribed to a [Paid Plan](https://vyuh.tech/pricing), you will have access to the [vyuh-tech/bricks](https://github.com/vyuh-tech/bricks) repo. You can use the Dart scaffolding tool, **Mason**, to quickly create a _Vyuh project_ with Flutter and Sanity.

Refer to the [Mason Setup](mason-setup.md) guide for more details.
{% endhint %}

### **An Aside on developing with a CMS 🤔**

Vyuh is designed to be CMS-agnostic and allows a different style of building Apps where you give the control of content to the content-editors. This is a powerful way to build apps where the content is the king and the app is just a medium to display it.

Content is not limited to only the text and other visual blocks on a CMS but it can be your entire App Experience. This technique has been a key pillar in building large scale apps where the content and experience can be very dynamic.

In this regard, the CMS becomes your declarative, **no-code tool** where you focus on what content should be shown under what conditions and the **Flutter App** becomes the code that renders it. Every block that is available on the CMS has its **counterpart** in Flutter. It's a different way of building apps and a throwback to Model-Driven Development.

What you get is a dynamic App that can be changed on the fly without an App Store release. It also allows greater exploration of the app experience and can be a great tool for A/B testing.

## 2. Configure the Studio

Add the following dependencies to your `package.json` and run run `pnpm i` . These dependencies will help in the schema development with Sanity and Vyuh.

```json
"dependencies": {
    /* Other Dependencies */
    "@vyuh/sanity-schema-core": "^1.3.13",
    "@vyuh/sanity-schema-system": "^1.3.13"
  },
```

Once installation is done, go to the `index.ts` file and update the `schemaTypes` as below

```typescript
import {bootstrap} from '@vyuh/sanity-schema-core'
import {system} from '@vyuh/sanity-schema-system'

const schemas = bootstrap([system])
export const schemaTypes = schemas
```

{% hint style="info" %}
**`bootstrap()`** is a function that takes in two arguments:

1. an array of `FeatureDescriptor`s that represent the features of your application. Each `FeatureDescriptor` contributes a set of content schemas that are available on the Studio.
2. Plugins for advanced configuration of the bootstrapping process. For now, you can ignore this argument.
{% endhint %}

We will also make some minimal changes to the `sanity.config.ts` file that sits at the root of your project.&#x20;

{% hint style="info" %}
Notice that we have updated the `plugins` property to include some standard configuration that is useful for a Vyuh App.
{% endhint %}

{% code title="sanity.config.ts" %}
```typescript
import {defineConfig} from 'sanity'
import {structureTool} from 'sanity/structure'
import {schemaTypes} from './schemaTypes'
import {defaultDocumentNode, deskStructure} from '@vyuh/sanity-schema-core'
import {visionTool} from '@sanity/vision'

export default defineConfig([
  {
    name: 'default',
    title: 'My Blog',
    basePath: '/',

    projectId: '<your-project-id>',
    dataset: 'production',

    plugins: [structureTool({structure: deskStructure, defaultDocumentNode}), visionTool()],
    schema: {
      types: schemaTypes,
    },
  },
])

```
{% endcode %}

Now run the project , using `pnpm dev`

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Your studio should look something like above with the standard document types of _Route_, _Conditional_ _Route_, _Category_ and _Region_.

## 3. Explore the Sanity Studio

Let's create a simple route for a blog post, with the path `/blog`. Add a region on the route and call it `body`. Inside this region, add the `Portable Text` block.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Now let's create some text and format it to look like a real blog post. We can add a title, section-headings, paragraphs, and images to the post.

<figure><img src="../.gitbook/assets/Screenshot 2024-06-06 at 6.43.02 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

## 4. Create the feature on the Flutter side

Just like we had the `FeatureDescriptor` for setting up the schemas in Sanity, we also have an equivalent `FeatureDescriptor` on the Flutter side that handles the rendering of these schemas. Let's set it up inside the `feature.dart` file.

{% hint style="info" %}
In our application, we define each feature using a `FeatureDescriptor`. A feature is treated as a self-contained, independent unit within the app and can include various routes and content-extensions to handle CMS content. It also has other metadata such as a name, title, icon, etc.

For every feature, we will create a dedicated `feature.dart` file, which will contain the `FeatureDescriptor`.
{% endhint %}

{% code title="feature.dart" %}
```dart

import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';
import 'package:vyuh_core/vyuh_core.dart';
import 'package:vyuh_feature_system/vyuh_feature_system.dart';

final feature = FeatureDescriptor(
  name: 'blog',
  title: 'My diary of travel blogs',
  description: 'Chronicling my journeys across the world in my travel blog',
  icon: Icons.pages,
  routes: () async {
    return [
      GoRoute(
        path: '/blog',
        pageBuilder: defaultRoutePageBuilder,
      ),
    ];
  },
);

```
{% endcode %}

Note that the `defaultRoutePageBuilder` is a standard page-builder that knows how to work with CMS routes. We are also declaring our starting blog entry in the `/blog` route.

## 5. Setup the Vyuh app

We are now in the final leg of this journey, where we connect the content, feature and the app to see all of it in action.

In order to pull content from Sanity, we need to include the **Sanity Content Provider** in our App's `pubspec.yaml`. Here's the entry you need to make under `dependencies`.

```yaml
dependencies:
    # ... other dependencies ...
    vyuh_content_provider_sanity:
        hosted: https://<private-hosted-pub>
        version: ^1.0.0
```

{% hint style="success" %}
**`vyuh_content_provider_sanity`** is a privately hosted package and part of the [`Enterprise plan`](https://vyuh.tech/pricing)
{% endhint %}

Now we add `SanityContentProvider` within the `DefaultContentPlugin`. This creates the connection to the Sanity Studio we setup earlier and can fetch content on demand.

```dart
import 'package:flutter/material.dart';
import 'package:sanity_client/client.dart';
import 'package:vyuh_content_provider_sanity/vyuh_content_provider_sanity.dart';
import 'package:vyuh_core/vyuh_core.dart' as vc;
import 'package:vyuh_extension_content/vyuh_extension_content.dart';
import 'feature.dart' as blog;

void main() async {
  WidgetsFlutterBinding.ensureInitialized();

  vc.runApp(
    initialLocation: '/blog',
    features: () => [
      blog.feature,
    ],
    plugins: [
      DefaultContentPlugin(
        provider: SanityContentProvider(
           SanityConfig(
            dataset: 'production',
            projectId: '<your-project-id>',
            token:'<your-token>',
          ),
        ),
      )
    ],
  );
}

```

* We use the /blog as our `initialRoute` to load it up as the starting page of our App.
* The `project-id` should be the same as the one used earlier for the Studio setup
* To get the **token**, visit the `Manage` console of the Sanity Project and create a Read Token. The token is a security measure and ensures only validated requests are allowed by Sanity.

<figure><img src="../.gitbook/assets/create token.png" alt=""><figcaption></figcaption></figure>

Now run the app and be ready to be greeted by our lovely blog. Play around in the Studio to add more text, images, and moving blocks around. If you hit the _**refresh icon**_ at the bottom-left of the screen, you will see the content updates live on your App.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
### Running on the Web?

If you are running on the Web, make sure to update the **CORS Origins** in Sanity to include the port number you are using. In the previous screenshot of the console, you will notice that we have used **`https://localhost:8080`**.\

{% endhint %}

## Summary

This guide gave you a quick way of creating content on Sanity and seeing it live on the Device Simulator. With the power of the Vyuh Framework, you can make changes on Sanity and have it rendered in real-time.&#x20;

Try exploring other Content types such as Cards, Groups, etc. This is just the beginning and there is lot you can do with custom content and the Vyuh Framework. We will explore more in other guides.\