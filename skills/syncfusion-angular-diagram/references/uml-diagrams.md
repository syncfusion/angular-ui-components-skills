# UML Diagrams

## Table of Contents
- [Overview](#overview)
- [UML Setup](#uml-setup)
- [Class Diagrams](#class-diagrams)
- [Classifiers](#classifiers)
- [UML Relationships](#uml-relationships)
- [Sequence Diagrams](#sequence-diagrams)

## Overview

**UML (Unified Modeling Language)** provides standardized notation for system design and documentation.

Syncfusion supports:
- **Class Diagrams** - Show classes, attributes, methods, and relationships
- **Sequence Diagrams** - Show message interactions over time

---

## UML Setup

### Enable UML Module

```typescript
import { Inject, UmlDiagrams } from '@syncfusion/ej2-angular-diagrams';

@Component({
  viewProviders: [Inject(UmlDiagrams)]  // Enable UML shapes
})
export class UmlComponent {}
```

---

## Class Diagrams

Class diagrams show the structure and relationships of classes in a system.

### Basic Class Node

```typescript
{
  id: 'class1',
  width: 150,
  height: 150,
  offsetX: 200,
  offsetY: 150,
  shape: {
    type: 'UmlClassifier', // Correct type for UML class
    classifier: 'Class',
    attributes: [
      {
        name: 'id',
        type: 'int',
        isSeparator: false
      },
      {
        name: 'name',
        type: 'string',
        isSeparator: false
      }
    ],
    methods: [
      {
        name: 'getName',
        parameters: [], // Correct property name is 'parameters'
        type: 'string'
      },
      {
        name: 'setName',
        parameters: [{ name: 'name', type: 'string' }],
        type: 'void'
      }
    ]
  }
}
```

### Class Node Structure

```typescript
shape: {
  type: 'UmlClassifier',
  classifier: 'Class',
  attributes: [       // Class properties
    {
      name: 'id',
      type: 'int',
      visibility: 'Private'  // Private, Public, Protected, Package
    }
  ],
  methods: [          // Class operations/methods
    {
      name: 'getName',
      parameters: [],  // Method parameters
      type: 'string',  // Return type
      visibility: 'Public'
    }
  ]
}
```

---

## Classifiers

### Class Classifier

Standard class shape:

```typescript
classifier: 'Class'
```

### Interface Classifier

Shows interface contract:

```typescript
classifier: 'Interface'
```

### Enumeration Classifier

Shows enum values:

```typescript
classifier: 'Enumeration',
enumMembers: [
  { name: 'RED' },
  { name: 'GREEN' },
  { name: 'BLUE' }
]
```

### Package Classifier

Groups related classes:

```typescript
classifier: 'Package'
```

### Component Classifier

Represents a reusable component:

```typescript
classifier: 'Component'
```

### Visibility Modifiers

```typescript
visibility: 'Public'       // + (accessible everywhere)
visibility: 'Private'      // - (accessible within class)
visibility: 'Protected'    // # (accessible in subclasses)
visibility: 'Package'      // ~ (accessible in same package)
```

### Abstract Classes

```typescript
isAbstract: true  // Class name appears in italics
```

### Static Members

```typescript
attributes: [
  {
    name: 'count',
    isStatic: true       // Underlined in diagram
  }
]
```

---

## UML Relationships

Relationships show how classes are connected:

### Association

Generic relationship between classes:

```typescript
{
  id: 'association',
  sourceID: 'class1',
  targetID: 'class2',
  relationship: 'Association',
  sourceMultiplicity: '1',    // Cardinality at source
  targetMultiplicity: '*'     // Cardinality at target (0..*, 1..*, 1..1, etc.)
}
```

### Generalization (Inheritance)

Subclass inherits from superclass:

```typescript
{
  id: 'generalization',
  sourceID: 'subclass',
  targetID: 'superclass',
  relationship: 'Generalization'
  // Shown as unfilled triangle arrow
}
```

### Realization (Interface Implementation)

Class implements interface:

```typescript
{
  id: 'realization',
  sourceID: 'class',
  targetID: 'interface',
  relationship: 'Realization'
  // Shown as dashed line with unfilled triangle
}
```

### Aggregation

"Has-a" relationship (whole-part, but part can exist independently):

```typescript
{
  id: 'aggregation',
  sourceID: 'wholeClass',
  targetID: 'partClass',
  relationship: 'Aggregation'
  // Shown as hollow diamond at whole end
}
```

### Composition

Strong "has-a" relationship (part cannot exist without whole):

```typescript
{
  id: 'composition',
  sourceID: 'ownerClass',
  targetID: 'ownedClass',
  relationship: 'Composition'
  // Shown as filled diamond at owner end
}
```

### Dependency

One class depends on another:

```typescript
{
  id: 'dependency',
  sourceID: 'class1',
  targetID: 'class2',
  relationship: 'Dependency'
  // Shown as dashed line with arrow
}
```

---

## Sequence Diagrams

Sequence diagrams show message interactions between actors and objects over time.

### Sequence Diagram Structure

```typescript
// Actor node
{
  id: 'actor1',
  width: 50,
  height: 50,
  offsetX: 100,
  offsetY: 60,
  shape: { type: 'UmlClassifier', classifier: 'Actor' }
},
// Object node
{
  id: 'object1',
  width: 100,
  height: 30,
  offsetX: 300,
  offsetY: 60,
  shape: { type: 'UmlClassifier', classifier: 'Object' }
},
// Lifeline node
{
  id: 'lifeline1',
  width: 10,
  height: 200,
  offsetX: 300,
  offsetY: 160,
  shape: { type: 'UmlClassifier', classifier: 'Lifeline' },
  // Optionally, you can link to the object node via annotations or custom logic
}
```

### Shapes

```typescript
shape: { type: 'UmlClassifier', classifier: 'Actor' }      // Stick figure
shape: { type: 'UmlClassifier', classifier: 'Object' }     // Rectangle with name
shape: { type: 'UmlClassifier', classifier: 'Lifeline' }   // Vertical dashed line
```

### Messages

```typescript
{
  id: 'message1',
  sourceID: 'lifeline1',
  targetID: 'lifeline2',
  type: 'Orthogonal',
  shape: {
    type: 'UmlMessage',
    messageType: 'Synchronous'  // Synchronous, Asynchronous, Return
  },
  annotations: [{
    content: 'methodCall()'
  }]
}
```

### Message Types

```typescript
messageType: 'Synchronous'   // Solid arrow
messageType: 'Asynchronous'  // Open arrow
messageType: 'Return'        // Dashed arrow
// 'Self' is not a direct messageType, use sourceID and targetID as same for self-message
```

### Interaction Frames

Group messages in constructs:

```typescript
{
  id: 'interactionFrame',
  shape: {
    type: 'UmlSequence',
    shape: 'InteractionUse',
    interactionType: 'Loop'  // Loop, Opt, Break, Par, Neg, Seq, Strict, Neg, Assert, Ignore, Consider, Neg
  }
}
```

---

**→ Next: Organize diagrams with [automatic layouts](layouts.md)**
