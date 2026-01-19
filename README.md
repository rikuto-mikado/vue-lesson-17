# Vue Lesson 17 - Component Communication & CRUD Operations

A Vue 3 learning project focused on component communication patterns and basic CRUD operations.

## Table of Contents

- [What I Learned](#what-i-learned)
  - [Props: Parent → Child](#1-props-parent--child-communication)
  - [$emit: Child → Parent](#2-emit-child--parent-communication)
  - [Inline $emit](#3-inline-emit-in-template)
  - [Delete with filter()](#4-deleting-items-with-arrayfilter)
  - [Form Handling](#5-form-handling-with-v-model)
  - [Data Flow Diagram](#6-data-flow-diagram)
- [Q&A: Common Questions](#qa-common-questions)
- [Memo: Tips & Techniques](#memo-tips--techniques)
- [Quick Reference](#quick-reference)

---

## What I Learned

### 1. Props: Parent → Child Communication

Props allow parent components to pass data to child components (read-only).

**Files:** `src/App.vue` → `src/components/FriendContact.vue`

```vue
<!-- Parent (App.vue) -->
<friend-contact
  :id="friend.id"
  :name="friend.name"
  :is-favourite="friend.isFavourite"
></friend-contact>
```

```javascript
// Child (FriendContact.vue)
props: {
  id: {
    type: String,
    required: true
  },
  isFavourite: {
    type: Boolean,
    required: false,
    default: false
  }
}
```

| Feature | Description |
|---------|-------------|
| `:prop-name` | Dynamic binding (v-bind shorthand) |
| `type` | Validates data type (String, Boolean, Number, etc.) |
| `required` | Makes prop mandatory |
| `default` | Fallback value if prop not provided |

---

### 2. $emit: Child → Parent Communication

`$emit` allows child components to send events (with data) to parent components.

**Flow:**

```
Child Component                    Parent Component
─────────────────                  ─────────────────
Button clicked
    ↓
this.$emit('event-name', data)  →  @event-name="handler"
                                       ↓
                                   handler(data) { ... }
```

**Code Example:**

```javascript
// Child: Emit event with payload
this.$emit('toggle-favourte', this.id);

// Parent: Listen and handle
@toggle-favourite="toggleFavouriteStatus"

methods: {
  toggleFavouriteStatus(friendId) {
    const friend = this.friends.find(f => f.id === friendId);
    friend.isFavourite = !friend.isFavourite;
  }
}
```

---

### 3. Inline $emit in Template

For simple events, you can use `$emit` directly in the template without a method.

```vue
<!-- Method approach (more code) -->
<button @click="deleteFriend">Delete</button>

methods: {
  deleteFriend() {
    this.$emit('delete', this.id);
  }
}
```

```vue
<!-- Inline approach (cleaner for simple cases) -->
<button @click="$emit('delete', id)">Delete</button>
```

| Approach | When to Use |
|----------|-------------|
| **Inline** | Simple emit with 1-2 arguments, no extra logic |
| **Method** | Complex logic, validation, or multiple actions needed |

---

### 4. Deleting Items with Array.filter()

Use `filter()` to remove items from an array immutably.

**File:** `src/App.vue:64-66`

```javascript
// Remove a friend from the array by filtering out the one with matching id
// Uses filter() to create a new array excluding the deleted friend
deleteContact(friendId) {
  this.friends = this.friends.filter(friend => friend.id !== friendId);
}
```

**How filter() works:**

```javascript
// Original array
[{id: 'a'}, {id: 'b'}, {id: 'c'}]

// filter(friend => friend.id !== 'b')
// Keep items where id is NOT 'b'

// Result: new array without 'b'
[{id: 'a'}, {id: 'c'}]
```

---

### 5. Form Handling with v-model

**File:** `src/components/NewFriend.vue`

```vue
<form @submit.prevent="submitData">
  <input type="text" v-model="enteredName"/>
  <input type="tel" v-model="enteredPhone"/>
  <input type="email" v-model="enteredEmail"/>
  <button>Add Contact</button>
</form>
```

| Feature | Description |
|---------|-------------|
| `v-model` | Two-way data binding between input and data property |
| `@submit.prevent` | Listens for submit + prevents page reload |

**Emitting multiple values:**

```javascript
this.$emit(
  'add-contact',
  this.enteredName,
  this.enteredPhone,
  this.enteredEmail
);
```

---

### 6. Data Flow Diagram

```
                        ┌─────────────────────┐
                        │   App.vue (Parent)  │
                        │                     │
                        │   friends: [...]    │
                        └─────────────────────┘
                           │             ▲
              Props ↓      │             │      ↑ $emit
         ┌─────────────────┘             └─────────────────┐
         │                                                 │
         ▼                                                 │
┌─────────────────────┐                     ┌─────────────────────┐
│  FriendContact.vue  │                     │    NewFriend.vue    │
├─────────────────────┤                     ├─────────────────────┤
│  Receives:          │                     │  Form inputs:       │
│  - id               │                     │  - name             │
│  - name             │                     │  - phone            │
│  - isFavourite      │                     │  - email            │
├─────────────────────┤                     ├─────────────────────┤
│  Emits:             │                     │  Emits:             │
│  - 'delete'         │ ──────────────────► │  - 'add-contact'    │
│  - 'toggle-favourte'│      to Parent      │                     │
└─────────────────────┘                     └─────────────────────┘
```

**Summary:**

| Direction | Method | Example |
|-----------|--------|---------|
| Parent → Child | Props | `:name="friend.name"` |
| Child → Parent | $emit | `$emit('delete', id)` |

---

## Q&A: Common Questions

### Q1: When should I use inline `$emit` vs a method?

**A:** Use inline when:
- The emit is simple (just event name + 1-2 arguments)
- No additional logic is needed

Use a method when:
- You need validation before emitting
- Multiple actions happen on click
- The logic is complex or reusable

```vue
<!-- Inline: Simple delete -->
<button @click="$emit('delete', id)">Delete</button>

<!-- Method: Needs confirmation -->
<button @click="confirmDelete">Delete</button>

methods: {
  confirmDelete() {
    if (confirm('Are you sure?')) {
      this.$emit('delete', this.id);
    }
  }
}
```

---

### Q2: What's the difference between `filter()` and `splice()` for deleting?

**A:**

| Method | Mutates Original? | Returns |
|--------|-------------------|---------|
| `filter()` | No (creates new array) | New filtered array |
| `splice()` | Yes (modifies in place) | Removed elements |

```javascript
// filter() - Recommended for Vue reactivity
this.friends = this.friends.filter(f => f.id !== friendId);

// splice() - Also works but mutates original
const index = this.friends.findIndex(f => f.id === friendId);
this.friends.splice(index, 1);
```

**Prefer `filter()`** - it's more predictable and works well with Vue's reactivity.

---

### Q3: Why do I need the `emits` option?

**A:** The `emits` option declares which events a component can emit.

```javascript
emits: ['toggle-favourte', 'delete']
```

**Benefits:**
- Documentation for other developers
- IDE autocomplete support
- Vue warns if you emit undeclared events
- Can add validation (object syntax)

---

### Q4: Why use `:key` in v-for?

**A:** Vue uses `key` to track each item's identity for efficient DOM updates.

```vue
<friend-contact
  v-for="friend in friends"
  :key="friend.id"  <!-- Must be unique! -->
></friend-contact>
```

Without `:key`, Vue may reuse DOM elements incorrectly when items are added/removed.

---

## Memo: Tips & Techniques

### Naming Conventions

| Context | Format | Example |
|---------|--------|---------|
| Template attribute | kebab-case | `:is-favourite` |
| JavaScript prop | camelCase | `isFavourite` |
| Event name | kebab-case | `@toggle-favourite` |
| Method name | camelCase | `toggleFavouriteStatus` |

### $emit Syntax

```javascript
this.$emit('event-name')           // No payload
this.$emit('event-name', data)     // Single value
this.$emit('event-name', a, b, c)  // Multiple values
```

### Array Methods Cheat Sheet

| Method | Purpose | Example |
|--------|---------|---------|
| `find()` | Get first matching item | `arr.find(x => x.id === id)` |
| `filter()` | Get all matching items / remove | `arr.filter(x => x.id !== id)` |
| `findIndex()` | Get index of first match | `arr.findIndex(x => x.id === id)` |
| `push()` | Add to end | `arr.push(newItem)` |

### Form Event Modifiers

| Modifier | Description |
|----------|-------------|
| `.prevent` | Calls `event.preventDefault()` |
| `.stop` | Calls `event.stopPropagation()` |
| `.once` | Only triggers once |

### Boolean Props Shorthand

```vue
<!-- These are equivalent -->
<friend-contact :is-favourite="true"></friend-contact>
<friend-contact is-favourite></friend-contact>
```

---

## Quick Reference

```
┌─────────────────────────────────────────────────────────┐
│                  COMPONENT COMMUNICATION                │
├─────────────────────────────────────────────────────────┤
│  Parent → Child:  Props (:prop-name="value")            │
│  Child → Parent:  $emit('event-name', payload)          │
├─────────────────────────────────────────────────────────┤
│                    CRUD OPERATIONS                      │
├─────────────────────────────────────────────────────────┤
│  Create:  this.items.push(newItem)                      │
│  Read:    this.items.find(x => x.id === id)             │
│  Update:  item.property = newValue                      │
│  Delete:  this.items = this.items.filter(x => x.id!==id)│
└─────────────────────────────────────────────────────────┘
```

---

## Installation

```bash
npm install
```

## Development

```bash
npm run serve
```
