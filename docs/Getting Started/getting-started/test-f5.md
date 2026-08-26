---
title: test f5
deprecated: false
hidden: false
metadata:
  robots: index
---

# Custom Fields (Beta)

Custom fields let you add your own typed attributes to candidate profiles, on top
of the built-in ones. You define each field once for your organization, choosing
its type and (optionally) a fixed set of allowed values.

## Availability

This feature is releasing in phases.

- **Available now:** define and manage your custom fields. Create, list, update,
  and delete field definitions through the endpoints in this section.
- **Coming later:** candidate values for these fields get populated, and you can
  filter and search candidates by them.

Provisioning your fields now means your data model is ready for when populating
and search become available. Until then, a field you create is a definition
only. It does not yet populate candidate values or affect search results.

## How custom fields work

A custom field moves through three stages. Only the first is available today.

1. **Define the field (available now).** You create a field definition: its
   `type`, a `fieldKey` you use to refer to it, a display `label`, and, for a
   dropdown, its `allowedValues`. This is the schema for the attribute. It does
   not yet hold any candidate data.

2. **Values get populated (coming later).** Once populating is enabled, each
   candidate profile can carry a value for the field. A `TEXT` field holds one
   value per candidate, a `TEXT_ARRAY` field holds several, and so on. The
   definition you created now is what those values attach to, which is why
   defining your fields early is useful: your data model is already in place.

3. **Filter and search (coming later).** Once search is enabled, you can query
   candidates by their custom field values, alongside the existing search
   criteria. For example, restrict results to candidates whose `clearanceLevel`
   is `Enhanced`, or whose `yearsExperience` is at least 5. The `type` you chose
   at definition time determines how a field can be queried: fixed-option and
   text fields by match, numbers and dates by range.

### A worked example

Suppose you recruit for roles that require a security clearance and you want to
track it per candidate.

- **Today**, you define the field once:

  ```json
  POST /v1/custom-fields
  {
    "fieldKey": "clearanceLevel",
    "label": "Clearance Level",
    "type": "TEXT",
    "hasFixedOptions": true,
    "allowedValues": ["None", "Basic", "Enhanced"]
  }
  ```

  You might also define a free-input `NUMBER` field `yearsExperience`, and a
  `TEXT_ARRAY` field `certifications`.

- **Later**, once populating is available, each candidate carries their own
  `clearanceLevel`, `yearsExperience` and `certifications` values.

- **Later still**, once search is available, you filter your candidate search to,
  say, `clearanceLevel = Enhanced` and `yearsExperience >= 5`, and rank or shortlist
  from there.

Because the field definition is stable, the fields you provision now are the same
ones those values and searches will use. There is nothing to migrate when the
later stages arrive.

## Field types

Each field has a fixed `type`, chosen at creation and immutable afterwards:

- `TEXT` — a single free-text value.
- `TEXT_ARRAY` — multiple text values.
- `NUMBER` — a numeric value.
- `DATE` — a date.
- `BOOLEAN` — true or false.

Set `hasFixedOptions` to `true` to constrain a field to a fixed list of
`allowedValues` (a dropdown); leave it `false` for free input. The two must stay
consistent: a fixed-options field needs a non-empty `allowedValues`, and a
free-input field must not carry any.

## Capacity and deletion

There is a maximum number of fields per `type`. Deleting a field is
**permanent** and does **not** free that capacity: the field is retired, not
reused. Plan your fields before creating them, and treat deletes as final. A
create that exceeds the limit for a type returns `400`.

## Scoping

Every request is scoped to the organization resolved from your API key. You
cannot read or modify another organization's fields, and any `organizationId` in
a request body is ignored.