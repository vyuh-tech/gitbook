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

### Explore the Sanity Studio

Bring up the Sanity Studio and start creating some routes. Let's create a simple route for a blog post, with the path `/blog`. Add a region on the route and call it `body`. Inside this region, add the Portable Text block.

Now let's create some text and format it to look like a real blog post. We can add a title, section-headings, paragraphs, and images to the post.

### Load the `/blog` route as the `initialPath` in Vyuh

## Summary

This guide gave you a quick way of creating content on Sanity and seeing it live on the Device Simulator. Try exploring other content types that are present in the default setup, such as Cards, Groups, etc. This is just the beginning and there is lot you can do with custom content. We will explore that in other guides.