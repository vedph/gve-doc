---
title: Customization
layout: default
parent: Usage
nav_order: 2
---

# Customization

Note that the Cadmus editor used here is tailored to the entities used by VEdition. You are free to completely redefine your entities by adding and removing parts from items facets.

Also, the editor relies on many taxonomies ([thesauri](https://vedph.github.io/cadmus-doc/models/thesauri.html)) defined for its parts. Often, you will want to customize them to fit your own project.

The easiest way to customize both facets and thesauri is loading the VEdition editor profile, customize it, and then upload all what you changed using the editor's UI. The editor anyway provides also admin pages for changing its profile, but this is usually quicker:

1. download the [VEdition profile](https://github.com/vedph/gve-core/blob/master/CadmusGveApi/wwwroot/seed-profile.json). This is a JSON document.
2. use a code editor (e.g. VSCode) to edit facets, flags, thesauri, settings, and optionally also the seeding behavior if you are going to use mock data.
3. upload the modified resources back into the editor via its admin pages:

- facets: copy the `facets` property, paste it into a new JSON file, remove the initial "`facets":` property name making it a root object, and save it. Then from the editor goto Admin/Import facets, select the file, and import it.
- thesauri: copy the `thesauri` property, paste it into a new JSON file, remove the initial "`thesauri":` property name making it a root object, and save it. Then from the editor goto Admin/Thesauri, expand Import, select the file, and import it.
- settings: copy the `settings` property, paste it into a new JSON file, remove the initial "`settings":` property name making it a root object, and save it. Then from the editor goto Admin/Import settings, select the file, and import it.

## Snapshot Customization

For snapshots, which are the core of the GVE model, customization can be very deep. Most of its relevant settings are found in two sections of the JSON profile document: thesauri and settings.

### Thesauri

Among thesauri, the most relevant ones define the vocabulary of your operation facets: `snapshot-feat-names@en` and `snapshot-feat-values@en`. The former is a list of all the facets names; the latter is a list of all the values for each named facet, when this must be restricted to a closed set of values.

For instance, the `log` feature is designed to carry a short summary of the operation; so it's just free text. You have the entry for its name in the first thesaurus, but no entries for its values in the second thesaurus:

```json
{
  "id": "snapshot-feat-names@en",
  "entries": [
    {
      "id": "log",
      "value": "log"
    }
    // ... etc.
  ]
}
```

Instead, ink or pencil colors in VEdition are conventionally defined in a closed list. So you have this entry in names:

```json
{
  "id": "snapshot-feat-names@en",
  "entries": [
    {
      "id": "r_fore-color",
      "value": "foreground color"
    }
    // ... etc.
  ]
}
```

Also, you have these values for that entry in the values thesaurus:

```json
[
  {
    "id": "r_fore-color:black",
    "value": "black"
  },
  {
    "id": "r_fore-color:blue",
    "value": "blue"
  },
  {
    "id": "r_fore-color:brown",
    "value": "brown"
  },
  {
    "id": "r_fore-color:darkgreen",
    "value": "darkgreen"
  },
  {
    "id": "r_fore-color:lightblue",
    "value": "lightblue"
  },
  {
    "id": "r_fore-color:orange",
    "value": "orange"
  },
  {
    "id": "r_fore-color:pink",
    "value": "pink"
  },
  {
    "id": "r_fore-color:red",
    "value": "red"
  }
]
```

Note the conventional syntax used for entry identifiers here: the ID of the feature name entry (`r_fore-color`) plus colon is a constant prefix. This allows the UI to link those entries as values for the foreground color.

### Settings

The settings section can be used by any parts in the Cadmus editor. Each part's settings is keyed under its part type ID, which for the snapshot part is `it.vedph.gve.snapshot`.

This setting key is a complex JSON object providing some of the most used settings for the model and its UI:

- `fontSize`: the default text font size.
- `fontFamily`: the default text font family.
- `foreColor`: the default text foreground color.
- `debug`: `true` to emit debug information in the browser console. Used for diagnostic purposes.
- `hints`: the definitions of all the hints used in the snapshot rendition.
- `animations`: the definitions of all the animations used in the snapshot rendition.
- `gveHintDesignerVariables`: an object whose `variables` property is a dictionary of preset variables used in the hints designer UI.
