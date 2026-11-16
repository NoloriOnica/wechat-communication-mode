# Architecture and Technology Stack

## 1. Overview

The WeChat Communication Mode prototype is a high-fidelity, interactive implementation of the eighteen screens
designed in Figma on the page *Communication Mode (Our Design)*. Its purpose is to let participants, testers and
assessors experience the proposed design on a real device, following seven task-based scenarios that reflect the
needs of older users, such as making a video call to a family member, sending a voice message, recovering from a
failed message and learning a task through guided help.

The prototype is deliberately limited to the front end. It contains no server, database or real messaging service;
calls, recordings and messages are represented by the screens that a user would see at each step. This keeps the
system simple to run and distribute, while still allowing the interaction design to be evaluated under realistic
conditions.

## 2. Technology Stack

The application is built with **React 19** and **Vite 8**, and is written in plain JavaScript (JSX). These choices
were guided by three requirements: faithfulness to the Figma design, speed of iteration within a short project
timeline, and ease of deployment.

**React** was selected because the design is component-based. The Figma file defines reusable components, such as
large buttons, the navigation bar, status banners and guide cards, and React allows each of these to be implemented
once and reused across all eighteen screens. A change to a shared component is therefore reflected consistently
throughout the prototype, mirroring the way the Figma design system behaves.

**Vite** was selected as the build tool for its fast development server and its ability to produce a static website
with a single command. The build is configured with relative asset paths, so the output can be hosted on any static
web host, served from a subfolder or opened locally without additional configuration.

**Plain JavaScript** was preferred over TypeScript. For a short-lived prototype developed by a small team, the
reduced setup and faster iteration were judged more valuable than the additional type safety TypeScript would
provide.

For styling, components use **inline styles expressed in Figma pixel values**, supported by a single global
stylesheet for the page layout, device frame and animations. This allows dimensions and positions to be copied
directly from the Figma inspector, which reduces the risk of visual discrepancies and avoids the overhead of
adopting a CSS framework.

Several further decisions kept the dependency footprint minimal. Navigation between views is handled by a small
custom hash-based router rather than a routing library, which also removes the need for server-side configuration
when the site is hosted. The twenty-four icons were exported from the Figma icon components as SVG path data, so no
icon library is required and the icons match the design exactly. Typography uses **Inter** and **Noto Sans SC**,
the same typefaces specified in the Figma file, to support both the English and Simplified Chinese labels. As a
result, the application has only two runtime dependencies: `react` and `react-dom`.

## 3. System Architecture

The application consists of three views, selected according to the URL:

1. **Start page**, which introduces the project and lists the seven scenarios together with a gallery of all
   eighteen screens.
2. **Scenario player**, which displays one screen at a time and allows the user to progress through a scenario,
   either by tapping the controls on the simulated phone or by using the step list and navigation buttons.
3. **Frame view**, which renders a single screen at its original size for pixel-level comparison with Figma exports.

The source code is organised into four layers. The **application layer** (`App.jsx`) handles routing and the three
views. The **scenario layer** (`scenarios.js`) defines each scenario as an ordered list of screen identifiers with an
instruction for each step. The **screen layer** (`screens/`) defines the eighteen screens and the base layouts they
share. The **component layer** (`components/`) contains the design tokens, such as colours and typefaces, the shared
interface components and the icons.

### 3.1 Fixed design canvas with uniform scaling

Every screen is laid out on a fixed canvas of 393 × 852 pixels, the frame size used in Figma, using the same
coordinates as the design. The rendering module scales the entire canvas uniformly to fit the available space: on a
mobile phone the screen fills the display, while on a desktop it is shown inside a device frame alongside the
scenario steps. This approach preserves the exact proportions and layout of the design on any screen size.

### 3.2 Composition of screens

Most screens are composed of a shared base layout (the Home page, the Contact page or the Chat page) combined with
an overlay specific to that screen. For example, the voice-recording screen is the Contact page with a dimmed
background and a recording panel placed on top. This composition reduces duplication and ensures that improvements
to a base layout apply to every screen derived from it.

### 3.3 Prototype navigation

Interactive elements declare which screen they lead to, replicating the prototype links defined in Figma. A shared
navigation context supplies these elements with the means to change screen. The same screen can therefore be shown
as a fully interactive screen in the scenario player, or as a static thumbnail in the gallery, simply by changing
whether the context is interactive. In keeping with the behaviour of Figma prototypes, tapping a non-interactive
area briefly highlights the tappable areas, helping participants who are unsure where to tap.

### 3.4 Data-driven scenarios

Scenarios are defined purely as data. Adding or modifying a walkthrough requires only editing the list of steps
and hints, without any change to the interface code. This made it straightforward to align the scenarios with the
user stories and to refine them after feedback.

## 4. Limitations

Because screens are positioned absolutely on a fixed canvas, they scale rather than reflow; this is appropriate for
evaluating a design but would not be suitable for a production application. The prototype holds no state beyond the
current screen, so actions such as sending a message are simulated rather than performed. Finally, the typefaces are
loaded from Google Fonts, so an internet connection is required on first load; all other assets are bundled with the
application.
