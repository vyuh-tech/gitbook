---
description: A mindset for building Vyuh Apps assembled with different Content Blocks
---

# Thinking in Blocks

Vyuh is an innovative framework designed to revolutionize the way developers build Flutter applications. Unlike traditional methods, Vyuh focuses on a content-based approach that leverages the power of modularity and dynamic content management systems (CMS). This framework introduces the concept of "blocks" enabling developers to create highly flexible and maintainable applications by decomposing them into distinct features and content blocks.

## Let's first examine the traditional method

**Traditional Flutter Development Approach**

In a traditional Flutter development approach, applications are often built in a monolithic fashion. This means:

1. **Monolithic Structure**: The entire application is typically a single codebase where features and content are tightly coupled.
2. **Hard-Coded Journeys**: User journeys and interfaces are hard-coded, requiring code changes for updates or modifications.
3. **Limited Modularity**: While Flutter encourages some level of component reuse, the overall structure remains monolithic, making it difficult to isolate and update individual features or content.
4. **Static Content**: Content is usually static and embedded directly into the app, leading to frequent updates and redeployments.

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

## How Vyuh Approach Differs

**Vyuh's Content-Based Approach**

Vyuh shifts the paradigm from a monolithic development approach to a more modular, feature-based block mode of operation. Here's how:

1. **Feature-Based Blocks**: Vyuh promotes the decomposition of applications into independent, reusable blocks. Each block represents a specific feature or piece of content.
2. **Dynamic Content Control**: Instead of hard-coding user journeys, Vyuh allows these to be managed dynamically via an external CMS. This enables real-time updates and modifications without requiring app redeployment.
3. **Enhanced Modularity**: By breaking down the app into smaller, manageable blocks, Vyuh enhances modularity. Each block can be developed, tested, and updated independently.
4. **Separation of Concerns**: This approach enforces a clear separation between the app's structure and its content, leading to cleaner, more maintainable codebases.

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>Feature Hierarchy in an Vyuh application</p></figcaption></figure>

## **Comparing the Approaches**

<table><thead><tr><th>Aspect</th><th width="235.66666666666663">Traditional Flutter Development</th><th>Vyuh Framework</th></tr></thead><tbody><tr><td><strong>Structure</strong></td><td>Monolithic</td><td>Modular (Feature-Based Blocks)</td></tr><tr><td><strong>Content Management</strong></td><td>Static, hard-coded within the app</td><td>Dynamic, managed via external CMS</td></tr><tr><td><strong>User Journeys</strong></td><td>Hard-coded</td><td>Dynamically controlled through CMS</td></tr><tr><td><strong>Modularity</strong></td><td>Limited</td><td>Enhanced through reusable blocks</td></tr><tr><td><strong>Flexibility</strong></td><td>Low, requires redeployment for updates</td><td>High, real-time updates through CMS</td></tr></tbody></table>

## **Mindset for Building Vyuh Apps**

Building applications with Vyuh requires a shift in mindset from traditional development practices. Developers need to think in terms of independent content blocks rather than a single, cohesive codebase. Here are key points to adopt this mindset:

* **Modular Design**: Think of each feature or piece of content as a self-contained block. Design these blocks to be as independent as possible, with minimal dependencies on other blocks.
* **Dynamic Content Management**: Embrace the flexibility offered by the CMS. Plan for dynamic content updates and user journey modifications. Design blocks to be adaptable to changes from the CMS without requiring code changes.
* **Separation of Concerns**: Maintain a clear separation between the application’s structure and its content. Treat the CMS as the primary source of truth for content and user journeys.
* **Reusability**: Design blocks with reusability in mind. Aim to create blocks that can be reused across different parts of the application or even in different applications.

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p>Features and Plugins as the two primary building blocks</p></figcaption></figure>

By moving away from monolithic structures and embracing feature-based blocks, developers can build more flexible, maintainable, and scalable applications. The dynamic control over user journeys and content through a CMS further enhances the capabilities of Flutter apps, making Vyuh a powerful framework for modern app development.
