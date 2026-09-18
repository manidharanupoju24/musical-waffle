# musical-waffle

My personal reading lists — curated, opinionated, and organized by depth rather than by date.

Each list covers one area I'm trying to actually understand, not just skim. Entries
favor primary sources: spec docs, design proposals, package comments, real source
code. Tutorials are mostly excluded — they teach you to use a thing, not how it works.

## Lists

| List | Covers |
|---|---|
| [operator-reading-list.md](./operator-reading-list.md) | Kubernetes operators — apiserver and object model, client-go plumbing, controller-runtime, CRD design, scale and failure modes, the Go runtime underneath, and real operators worth reading |

## How the lists are organized

- **Layers, not chapters.** Layer 1 is the foundation the next layer sits on. Read in
  order the first time; after that they're reference.
- **Every entry has a "why".** If I can't say what a link gives me that the layer below
  didn't, it doesn't belong on the list.
- **`[yours]`** marks links I came in with, as opposed to ones added while building
  out the list.

## Conventions

- One file per area, named `<area>-reading-list.md`.
- Tables: resource on the left, why it's here on the right. No status columns — this
  is a map, not a tracker.
- Lists get revised, not appended to. If something turns out to be weak, it comes out.
- Notes on individual items, when they get long enough to deserve their own file, go
  under `notes/<area>/`.

## Adding a list

1. Copy the layer structure from an existing list.
2. Start from the thing you're actually a client of and work outward.
3. Write the "why" column first — it's the filter.
