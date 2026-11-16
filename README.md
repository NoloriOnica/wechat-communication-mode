# WeChat Communication Mode – MVP prototype

React (Vite) build of the 18 screens on the Figma page **Communication Mode (Our Design)**
(file `THVq7yIquxIQK1pYE91GE6`, node `2009:1688`). Mobile-first: on phones each screen fills the display;
on desktop it is shown in a device frame next to the scenario steps.

```
npm install
npm run dev      # http://localhost:5173
npm run build    # static site in dist/ (relative paths, can be hosted anywhere)
```

## How it works

- **Start screen**: 7 scenarios (cases) plus a gallery of all 18 screens.
- **Player**: tap the real buttons on the phone to follow the prototype links, or use Back / Next,
  the step list, or the ← → keys. Tapping a non-interactive area flashes the tappable areas (like Figma);
  "Show tappable areas" keeps them visible.
- URLs: `#/play/<scenario>/<screen>` deep-links into a step; `#/frame/<id>` renders one bare 393×852 frame.

## Where things are

| Path | What |
| --- | --- |
| `src/components/ui.jsx` | Design tokens and shared Figma components (Button Large, Nav Bar, Banner, Guide Card…) |
| `src/components/iconPaths.js` | The 24 icons, exported from the Figma icon components |
| `src/screens/bases.jsx` | Home, Contact page and Chat base layouts |
| `src/screens/screens.jsx` | Screens 01–18, their prototype links and Figma captions |
| `src/scenarios.js` | Scenario definitions (steps and hints) shown on the start screen |
