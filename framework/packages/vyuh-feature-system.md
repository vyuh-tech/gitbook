---
description: >-
  The quintessential system feature that provides the building blocks for
  CMS-driven UI
---

# Vyuh Feature System

This is a system level feature that is needed for all Vyuh Apps. It includes the fundamental blocks for rendering Content Items and Routes, handling Actions, branching based on Conditions and switching between different Layouts for content items.

The following sections go into details of the various elements exposed in this feature.

## Content Items

Content Items, also known as _Structured Content Blocks,_ represent the visual and information blocks used in an Application. These blocks can be configured on the CMS and have a visual equivalent rendered with Flutter. The System feature includes core elements such as:

* **Card**
* **Group**
* **API Content**&#x20;
* **Conditional**
* **Portable Text**
* **Divider**
* **Accordion**
* **Empty**
* **Unknown**

## Routes

Routes represent individual pages or screens of content. When creating a user journey, you would create several routes and link them together. Routes are a special kind of Content Item. The system feature includes two kinds of routes:&#x20;

* **Page**
* **Dialog**

## Conditional Routes

A special type of Route that allows branching between two or more routes based on a condition. Conditions are configured on the CMS and evaluated at runtime on the App by the framework.&#x20;

A simple example of a Conditional Route is one where you go to the _Login Route_ if the user is _not authenticated_ and straight to the _Home route_ when _already logged in_.

## Layouts

Layouts represent different ways of rendering a Content Item. Every content item can have one or more layouts along with a default layout. Following are the various layouts supported by the Content Items mentioned earlier.

* **Card**
  * Default
  * Button
  * List Item
* **Group**
  * Default
  * Carousel
  * Grid
  * Vertical List
* **Portable Text**
  * Default
* **Accordion**
  * Default
* **Divider**
  * Default
* **Empty**
  * Default
* **Unknown**
  * Default
* **API Content**
  * Default
* **Route**
  * Default
  * Tabs
  * Single Item
* **Conditional Route**
  * Default

## Actions

Actions allows you to invoke specific operations based on certain user or system events. These could range from showing a dialog to making API calls. The following actions are built into the system feature:

* **Navigate to a Url or a Route**
* **Navigate Back**
* **Show Alert**
* **Delay**
* **Open/Close Drawer**
* **Open/Close End-Drawer**
* **Open a route in a Dialog**
* **Open Url in a platform-specific App**
* **Refresh a Route**
* **Restart the Vyuh Application**
* **Show/Hide a SnackBar**
* **Toggle Light/Dark Themes**

## Conditions

A Condition is evaluated at runtime and results in one of many values. The conditions in the system feature are:

* **Simple Boolean**
* **Current Platform**
* **Current Theme**
* **Feature Flag**
* **Current Screen Size**
* **User Authenticated**
