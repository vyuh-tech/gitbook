---
description: Scaffold your Vyuh project quickly with Mason bricks
---

# Mason Setup

{% hint style="info" %}
If you are part of an [Enterprise Plan](https://vyuh.tech/pricing), you would have received access to the bricks repository that allows you to quickly setup a Vyuh project. This guide shows you how to setup Mason and scaffold your project.
{% endhint %}

[Mason](https://github.com/felangel/mason) is a tool that uses a template and generates the files needed for your projects. By using simple parameters such as a `name`, `title` and other details of your project, mason can scaffold the initial project structure quickly.&#x20;

This is possible with **bricks**. A _brick_ is a template that can be expanded by Mason to generate the final file or project structure. Let's setup mason and configure it to use the bricks from Vyuh.

## 1. Add Mason

The first step is to activate the [mason\_cli](https://pub.dev/packages/mason\_cli) package globally. This gives you access to the `mason` command that can be used to generate the initial structure. Run this command in your terminal:

```bash
dart pub global activate mason_cli
```

In the folder where you plan to build your _mono-repo_ project for the Vyuh Application, run the following:

```bash
mason init
```

The above command should have created a `mason.yaml` file in that folder.&#x20;

## 2. Add the Vyuh related bricks

Replace the contents of `mason.yaml` with below:

```yaml
# Register bricks which can be consumed via the Mason CLI.
# Run "mason get" to install all registered bricks.
# To learn more, visit https://docs.brickhub.dev.
bricks:
  vyuh_init:
    git:
      url: https://github.com/vyuh-tech/bricks
      path: vyuh_init
  vyuh_feature:
    git:
      url: https://github.com/vyuh-tech/bricks
      path: vyuh_feature
  vyuh_feature_sanity_schema:
    git:
      url: https://github.com/vyuh-tech/bricks
      path: vyuh_feature_sanity_schema

```

Next, in the same folder, run the following command to fetch all the bricks:

```bash
mason get
```

## 3. Create your project with Mason

It is finally time to scaffold our project. We can use the `vyuh_init` brick and create the project:

```bash
mason make vyuh_init
```

This will generate the complete project structure that is essential to build the Vyuh App. For more details about what this project structure looks like, refer to the guide on [Typical Project Structure](../guides/typical-project-structure.md).

## Explore other bricks

In addition to the vyuh\_init brick, there are also bricks for:

* **vyuh\_feature**: scaffolds a single Vyuh feature package in Flutter
* **vyuh\_feature\_sanity\_schema**: scaffolds a single schema package in TypeScript for working with the Sanity CMS.

The process to use them is similar to the one used before. For example, to scaffold a new Feature package, you can run the following command:

```bash
mason make vyuh_feature
```

{% hint style="success" %}
### Custom output folder

By default Mason generates the files in the folder where you run the command. To create it inside a specific folder, you could pass in the **`-o`** flag with the relative directory. This is particularly useful when generating features.&#x20;

For example, to generate the feature package inside the **`./features`** folder of your root project directory, you would run the following:

```bash
mason make vyuh_feature -o features
```
{% endhint %}

## Summary

This guide showed you the steps to setup **mason** on your system and use it to generate various parts of the Vyuh Application. Currently, there is support for creating a new Vyuh App, a Vyuh Feature package for Flutter and a Vyuh schema package for Sanity CMS.&#x20;

There will be more added in the future to accommodate other aspects of developing Vyuh Apps.

