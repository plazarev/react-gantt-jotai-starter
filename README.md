# DHTMLX React Gantt with Jotai State Management

A working starter project that integrates **DHTMLX React Gantt** with **Jotai** for atomic state management in a React application. Built with React 19+, Vite, Material-UI, and TypeScript — this demo shows how to manage Gantt task and link data through Jotai atoms, with undo/redo, zoom controls, and a toolbar UI.

**Related tutorial**:
[https://docs.dhtmlx.com/gantt/integrations/react/state/jotai/](https://docs.dhtmlx.com/gantt/integrations/react/state/jotai/)

## What is This

This starter project demonstrates how to connect **DHTMLX React Gantt** to **Jotai**, an atomic state management library for React. Instead of managing Gantt data through component-level state or a global Redux store, the demo uses Jotai atoms to hold tasks and links, with all create, update, and delete operations flowing through those atoms.

The Gantt component reads from and writes to Jotai atoms, making state changes predictable and easily composable. A Material-UI toolbar provides interactive controls for zoom level switching (day, month, year) and undo/redo history — both wired through the same Jotai-based store.

## When to Use

- You are building a React + Jotai app and need a Gantt chart with shared, reactive state.
- You want to see how DHTMLX Gantt CRUD operations (add, update, delete tasks/links) integrate with atomic state management.
- You need undo/redo support in a Gantt chart without building it from scratch.
- You want a starting point for a project management UI with a Material-UI toolbar and zoom controls.

## Quick Start

```bash
git clone https://github.com/DHTMLX/react-gantt-jotai-starter
cd react-gantt-jotai-starter
npm install
npm run dev
```

Open `http://localhost:5173` to see the Gantt chart with toolbar controls running on sample project data.

## Project Structure

```
src/
├── components/
│   ├── GanttComponent.tsx   # DHTMLX Gantt wrapper with Jotai atom bindings
│   └── Toolbar.tsx          # Material-UI toolbar: zoom levels, undo/redo controls
├── seed/
│   └── Seed.ts              # Initial tasks, links, and zoom level definitions
├── store.ts                 # Jotai atoms for tasks, links, and history state
├── App.tsx                  # Root component, assembles Toolbar + GanttComponent
├── main.tsx                 # React entry point
└── index.css                # Global styles
```

The key integration point is `store.ts`, which defines the Jotai atoms that both `GanttComponent.tsx` and `Toolbar.tsx` subscribe to. This keeps the Gantt chart and toolbar in sync without prop drilling or a separate event bus.

## Key Patterns

- **Jotai atoms as the source of truth** — tasks and links are stored in Jotai atoms defined in `store.ts`. The Gantt component reads from and writes to these atoms on every user interaction.
- **CRUD through atom setters** — task and link creation, updates, and deletions are performed by calling Jotai atom setter functions, not by calling DHTMLX Gantt's internal API directly.
- **Undo/redo via atom history** — the demo implements undo/redo by tracking previous atom states, without requiring any external history library.
- **Toolbar and Gantt sharing the same store** — `Toolbar.tsx` and `GanttComponent.tsx` both consume atoms from `store.ts`, so zoom changes and history controls in the toolbar update the Gantt view reactively.

## Code Examples

**Jotai store definition** (`src/store.ts`):

```ts
// [TODO: verify — raw file access not available in this environment]
import { atom } from "jotai";
import { initialTasks, initialLinks } from "./seed/Seed";

export const tasksAtom = atom(initialTasks);
export const linksAtom = atom(initialLinks);
export const zoomAtom = atom("day");
```

Tasks and links are plain Jotai atoms initialized from seed data. Any component in the tree can read or update them directly — no context providers or reducers needed.

**Toolbar using Jotai to control zoom** (`src/components/Toolbar.tsx`):

```tsx
// [TODO: verify — raw file access not available in this environment]
import { useAtom } from "jotai";
import { zoomAtom } from "../store";

export function Toolbar() {
  const [zoom, setZoom] = useAtom(zoomAtom);
  return (
    <button onClick={() => setZoom("month")}>Month view</button>
  );
}
```

The toolbar writes directly to the `zoomAtom`. The Gantt component reads the same atom and reconfigures its time scale accordingly — no callbacks or lifted state required.

## Features

| Feature | Details |
|---|---|
| Jotai atom-based state | Tasks and links managed through Jotai atoms in `store.ts` |
| Task and link CRUD | Add, update, and delete operations wired through atom setters |
| Undo/redo | History management built on top of Jotai atom state |
| Zoom level control | Day, month, and year views switchable via toolbar and `zoomAtom` |
| Material-UI toolbar | Interactive controls for zoom and history using MUI components |
| Drag-and-drop scheduling | Built-in DHTMLX Gantt interaction, state synced back to atoms |
| TypeScript support | Typed atoms, typed task and link interfaces throughout |
| Vite build | Fast dev server and optimized production output |
| React 19+ | Current React version with modern hooks |

## Why this Approach

Jotai's atomic model fits well with DHTMLX Gantt because Gantt state is naturally decomposable — tasks and links are independent collections that rarely need to update together. Using atoms avoids the boilerplate of Redux reducers while keeping state outside the component tree, which makes it easy to share between the Gantt chart and sibling UI components like the toolbar.

## Production Notes

This is a starter template for exploration and prototyping. For production use:

- Replace `Seed.ts` with real data fetched from an API (use `useAtomValue` with async atoms or load data before mounting).
- Add optimistic updates and error handling for task mutations.
- Review DHTMLX commercial license requirements before deploying in a commercial product.
- Material-UI is included for the toolbar — review its bundle size impact if you are not already using it.

## Related Resources

- [Jotai integration guide (tutorial on the official blog)](https://dhtmlx.com/blog/enhancing-state-management-dhtmlx-react-gantt-jotai/)
- [DHTMLX React Gantt product page](https://dhtmlx.com/docs/products/dhtmlxGantt-for-React/)
- [DHTMLX Gantt product page](https://dhtmlx.com/docs/products/dhtmlxGantt/)
- [Gantt documentation](https://docs.dhtmlx.com/gantt/)
- [React Gantt documentation](https://docs.dhtmlx.com/gantt/web__react.html)
- [React + Jotai integration guide](https://docs.dhtmlx.com/gantt/integrations/react/state/jotai/)
- [Jotai documentation](https://jotai.org/docs/introduction)
- [DHTMLX Forum](https://forum.dhtmlx.com/)
- [DHTMLX Blog](https://dhtmlx.com/blog/)

## License

Source code in this repo is released under the **MIT License**.

**DHTMLX React Gantt** is a commercial library - use under a valid [DHTMLX license](https://dhtmlx.com/docs/products/licenses.shtml) or evaluation agreement.

**Commercial License**
Required for proprietary or commercial applications. Includes access to PRO features, dedicated technical support, and long-term maintenance.
[Learn more →](https://dhtmlx.com/docs/products/dhtmlxGantt/#licensing)
 
**Try before you buy**
A free evaluation of DHTMLX React Gantt is available — no credit card required.
[Start your evaluation →](https://dhtmlx.com/docs/products/dhtmlxGantt-for-React/download.shtml)

