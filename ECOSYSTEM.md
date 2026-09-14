# Ecosystem, Integration, and Interfaces

`time-boxer` is a boundary component in a loosely coupled planning ecosystem.

Its role is intentionally narrow: provide the human-facing time interface between a plan and real calendar time.

## Ecosystem Position

```text
plan-editor
    |
    | plan contract
    v
 time-boxer
    |
    | time / calendar contract
    v
weekly-scheduler
    |
    v
 workflow
    |
    v
auto-recap
    |
    +----> plan-editor
```

## Loose Coupling

Components communicate through contracts rather than implementation details.

`time-boxer` does not need to know how `plan-editor` generated a plan. It only needs the plan contract.

Likewise, `weekly-scheduler` should consume time information without depending on the internal UI implementation.

```text
producer -> contract -> consumer
```

not:

```text
producer -> private implementation -> consumer
```

## Integration

Integration means making independently evolving components interoperable.

For `time-boxer`, the key interfaces are:

- plan input;
- 15-minute time-kit representation;
- scheduled time blocks;
- calendar events;
- execution links or references.

The interface should describe **what the next component needs**, not how the current component works.

## Interface

A time-box is the smallest practical boundary between planning and calendar time.

```yaml
kit:
  duration: 15m
  task: "...
  action: "..."
  output: "..."
```

A kit is an interface object, not an implementation object.

It can be rendered in a UI, placed into a schedule, exported to a calendar, or handed to a workflow.

## Responsibility Boundary

`time-boxer` owns:

- time-oriented presentation;
- editing and reviewing time kits;
- human interaction with time allocation;
- calendar interaction/synchronization;
- conversion between calendar representation and ecosystem representation.

`time-boxer` does **not** own:

- deciding what the user should work on;
- discovering project priorities;
- resolving business dependencies;
- executing GitHub changes;
- generating the historical recap.

Those responsibilities belong to other components.

## Calendar Is an Interface

Google Calendar is treated as an external interface, not as the source of planning truth.

The ecosystem representation remains independent of the calendar provider.

```text
ecosystem time-kit
        |
        v
  calendar adapter
        |
        v
Google Calendar
```

This allows the calendar provider to be replaced without changing the planning model.

## Boundary Principle

> **The interface belongs to the ecosystem; the implementation belongs to the component.**

A good interface is small, explicit, versionable, and replaceable.

## Design Principle

> **Thin interfaces, autonomous components, explicit contracts.**

`time-boxer` should therefore remain a thin layer between planning and calendar time rather than becoming another planning brain.
