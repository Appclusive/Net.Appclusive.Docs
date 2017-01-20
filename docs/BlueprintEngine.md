This page contains information about the *BlueprintEngine* and how blueprints are defined and can be combined.

# Definitions

## Behaviour

A `Behaviour` is the most generic component in the system and defines a set of `Actions`, `States` and `Attributes`. A `Blueprint` can choose to *implement* any `Behaviour` it whishes (which is done via a `Model`), but then also must *implement* all the `Actions`, `States` and `Attributes` of that `Behaviour`.

The actual implementation of a `Behaviour` is done via a `BehaviourDefinition` which is a special form of a `Model`.

### BaseBehaviour

`Behaviours` form a hierarchy tree, however unlike a real tree where one `Behaviour` can *derive* from multiple other `Behaviours` (similar to multiple inheritance in programming languages like C++). This chain starts at a common single *root* that is called `BaseBehaviour`. 

In other words, the `BaseBehaviour` defines the most common `Actions` and `States` for all other `Behaviours` (the `BaseBehaviour` does not defines any `Attributes`).

## State

* A `State` describe an instantiated `Model` while at rest. An instance in any given `State` does not change until an `Action` is invoked on that instance.

* Each `State` has a *name* which must be unique throughout the whole state machine.

* Each `State` must be part of a `Transistion`, which means it must have at least an `Action` leading to it or an `Action` leading from it.

## Action

* An `Action` is a means (and the only means) for a `Model` to transit from an arbitrary source `State` to arbitrary destination `State` (where the *destination* `State` can also be the *source* `State`, effectively creating a loop). 

* An `Action` can define zero or more `Attributes` in order to be able to be invoked. Of these `Attributes` zero or more `Attributes` may have additional *constraints* (such as being `Required`).

* Each `Action` in a `Model` may only appear *once* in any `Transition`, which means that the *name* of the `Action` must be unique throughout the whole state machine.

In other words. Given an arbitrary `Action` (name) we also know the associated *source* and *destination* of it (which forms a `Transition`).

## Attribute

* An `Attribute` basically is a Name/Value pair with additional constraints in form of annotations. The name of any `Attribute` is unique throughout the whole system and always consists of a dot-notation starting with reversed top level domain name (such as `com.example.Appclusive.Blueprint.Name`).

* Any `Model` can override or redefine any `Attributes` (or its constraints) that it *derives* from its ancestor `Model` chain.

* Any `Attribute` constraints directly derived from its ancestor `Models` are also present on the current `Model`. Any `Attributes` that a `Model` *implements* via one of its `Behaviours` must be re-defined on the `Model`.

## Model

* `Models` - in contrast to `Behaviours` - provide actual implementations that can be used alone to provide functionality of any kind. A `blueprint` must always reference at least one `Model` explicitly (whereas never a `Behaviour` directly) and a `Model` can be instantiated whereas a `Behaviour` cannot.

* A `Model` must always *derive* from another single *parent* `Model` and multiple `Models` can *derive* from the parent `Model` (making them *siblings*). This effectively forms a single-parent hierarchy tree of `Models` eventually starting with `BaseModel`.

* A `Model` can choose not be the parent for any other `Models` which is then considered to be `sealed`.

### ModelDefinition

Every time a `Behaviour` is defined, its actual implementation is done via a `BehaviourDefinition` (which is a special form of a `Model`). This means that this `BehaviourDefinition` contains all the `Actions`, `States` and `Attributes` the `Behaviour` whishes to expose.

### BaseModel

The `BaseModel` is a special `ModelDefinition` (and thus also a `Model`) as it serves as the *root* `Model` for all other `BehaviourDefinitions` and thus all other `Models`.

#### BaseModelStates

The following lists the minimum set of `States` that every `Model` and `Behaviour` in the system must expose:

1. InitialState

  This is the *first* or starting point (which is in fact a `State`) of every `Model` to be instantiated. From there an instance of a `Model` will traverse from `State` to `State` via its exposed `Actions`.

  From this `State` an `Action` called `Initialise` will be executed to allow the `Model` to povision its instance.

1. DecommissionedState

  Before a instance of a `Model` is disposed it must transit through a `State` that is called the `DecommissionedState`. An instance of a `Model` in this `State` must prepare itself to get disposed, but also may offer an option to get re-activated (via one of its exposed `Action`).

  Instances in this `State` are periodically called by the system via the `Finalise` `Action` to check if they can be safely disposed.

1. FinalState

  With the `InitialState` being the *first* `State` of every instantiated `Model`, the `FinalState` is the *last* `State` of every instantiated `Model`. In fact this is the `State` where an instance of a `Model` is disposed and removed from the [[Inventory]].

1. ErrorState

  The `ErrorState` is reserved for situations where an instantiated model experienced an unforseen error condition and could not continue execution otherwise (such as a corrupt state). The `Blueprint` designer can use this state for remediation `Actions` (such as the `Remedy` `Action`).


#### BaseModelActions

The following lists the minimum set of `Actions` that every `Model` and `Behaviour` in the system must expose:

1. Initialise

  This `Action` is automatically invoked by the system when an instance of any `Model` is created.

1. Finalise

  This `Action` is automatically called by the system when an instance of a `Model` is in the `DecommissionedStatE` and going to be disposed. The `Model` may choose to deny the request for disposal effectively staying in the `DecommissionedState`.

1. Remedy

  An instance of a `Model` in the `ErrorState` may choose to perform a reconciliation task with the aim to place that instance in a defined and stable `State`. This default `Action` is called `Remedy`. A `Blueprint` desginer may choose to add additional remediation `Actions` where required.

#### BaseModel StateMachine and Transitions

The following illustrates the transitions of the state machine of the `BaseModel`:

```
InitialState        -- > Initialise -- > DecommissionedState
DecommissionedState -- > Finalise   -- > FinalState
ErrorState`         -- > Remedy     -- > ErrorState
```

## Item

An instantiated (or instance of a) `Model` becomes an `Item` in the [[Inventory]].