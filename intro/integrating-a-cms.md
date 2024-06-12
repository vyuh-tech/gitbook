---
description: Simple integration of Vyuh with Sanity.io, the default CMS
---

# Integrating a CMS

Now that we know how to setup a feature in Vyuh, let's see how we can integrate it with a CMS. We will be using [Sanity.io](https://sanity.io) for this example, which is also our default supported CMS.

## Using Sanity

Sanity is a headless CMS that allows you to define your own schemas and content types, entirely in code. This makes it easy to quickly build sophisticated schemas without wrestling with any Drag-n-Drop builders. It also sports a **Studio** which becomes the go-to UI tool for content editors. In this section, we will set up the studio and use the Vyuh Framework to build a simple schema and widget.

### Setup a sanity project

Setup a _free_ Sanity project using the [guides from Sanity.io](https://www.sanity.io/get-started?ref=hero) or use the command line shown below:

```bash
pnpm dlx create-sanity@latest
```

While creating sanity project, select project template as `Clean project with no predefined schema types`

{% hint style="info" %}
If you have subscribed to a [Paid Plan](https://vyuh.tech/pricing), you will have access to the [vyuh-tech/bricks](https://github.com/vyuh-tech/bricks) repo. You can use the Dart scaffolding tool, **Mason**, to quickly create a _Vyuh project_ with Flutter and Sanity.

Refer to the [Mason Setup](mason-setup.md) guide for more details.
{% endhint %}

{% hint style="success" %}
**An Aside on developing with a CMS 🤔**

Vyuh is designed to be CMS-agnostic and allows a different style of building Apps where you give the control of content to the content-editors. This is a powerful way to build apps where the content is the king and the app is just a medium to display it.

Content is not limited to only the text and other visual blocks on a CMS but it can be your entire App Experience. This technique has been a key pillar in building large scale apps where the content and experience can be very dynamic.

In this regard, the CMS becomes your declarative, **no-code tool** where you focus on what content should be shown under what conditions and the **Flutter App** becomes the code that renders it. Every block that is available on the CMS has its **counterpart** in Flutter. It's a different way of building apps and a throwback to Model-Driven Development.

What you get is a dynamic App that can be changed on the fly without an App Store release. It also allows greater exploration of the app experience and can be a great tool for A/B testing.
{% endhint %}

### Add the NPM packages for Vyuh Sanity schemas

```json
"dependencies": {
    /* Other Dependency */
    //Vyuh Dependency
    "@vyuh/sanity-schema-core": "^1.3.13",
    "@vyuh/sanity-schema-system": "^1.3.13"
  },
```

After adding these dependency in `package.json` , run `pnpm i`&#x20;

Once installation is done, go to `index` file and update the schemaTypes as below

```typescript
import {bootstrap} from '@vyuh/sanity-schema-core'
import {system} from '@vyuh/sanity-schema-system'

const schemas = bootstrap([system])
export const schemaTypes = schemas
```

{% hint style="info" %}
_**Bootstrap**_ is a function that takes in two arguments, features and plugins, and returns a sanity.SchemaTypeDefinition\[]
{% endhint %}

Now run the project , using `pnpm dev`

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

You will get route, category, region from the `system` out of the box&#x20;

### Explore the Sanity Studio

Bring up the Sanity Studio and start creating some routes. Let's create a simple route for a blog post, with the path `/blog`. Add a region on the route and call it `body`. Inside this region, add the Portable Text block.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Now let's create some text and format it to look like a real blog post. We can add a title, section-headings, paragraphs, and images to the post.

<figure><img src="../.gitbook/assets/Screenshot 2024-06-06 at 6.43.02 PM.png" alt=""><figcaption></figcaption></figure>

### Load the `/blog` route as the `initialPath` in Vyuh app



**Now to run this route `/blog` we need to make some change in the app.**

First you need to add `vyuh_content_provider_sanity` to the pubspec.yaml file

{% hint style="info" %}
This `vyuh_content_provider_sanity` is a part of `Enterprise plan.`&#x20;

```yaml
vyuh_content_provider_sanity:
    hosted: https://<private-hosted-pub>
    version: ^1.0.0
```
{% endhint %}

Now create a blog feature discriptor ie `feature.dart`



{% hint style="info" %}
In our application, we define each feature using a `FeatureDescriptor`. A feature is treated as a self-contained unit within the app and can include various routes, extensions, and extension builders.

For every feature, we will create a dedicated `feature.dart` file, which will contain the `FeatureDescriptor`.
{% endhint %}

```dart

import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';
import 'package:vyuh_core/vyuh_core.dart';
import 'package:vyuh_feature_system/vyuh_feature_system.dart';

final feature = FeatureDescriptor(
  name: 'blog',
  title: 'Blog Example',
  description: 'A simple blog example',
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

* In this, we are using defaultRoutePageBuilder to build the page for blog

Now we add `SanityContentProvider` into main.dart, it will create a connection between sanity studio and our app.&#x20;

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
            dataset: 'your_data_set',
            projectId: 'your_production_id',
            token:'your_token',
          ),
        ),
      )
    ],
  );
}

```

* The SanityContentProvider is used to provide content from the Sanity CMS.
* The SanityContentProvider takes a SanityClient object as a parameter. The SanityClient object is used to connect to the Sanity CMS.

Now run the app

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you are running on web,then update the cors origin in sanity for the port number which you are running the app\
![](<../.gitbook/assets/image (2).png>)
{% endhint %}

## Summary

This guide gave you a quick way of creating content on Sanity and seeing it live on the Device Simulator. Try exploring other content types that are present in the default setup, such as Cards, Groups, etc. This is just the beginning and there is lot you can do with custom content. We will explore that in other guides.\
