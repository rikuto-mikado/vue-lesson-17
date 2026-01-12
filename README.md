# Vue Lesson 17 - Parent-Child Component Communication

A Vue 3 learning project focused on component communication patterns.

## What I Learned Today

### 1. Props: Parent → Child Communication

Props allow parent components to pass data to child components (read-only).

**Code Location:** `src/App.vue:7-18` and `src/components/FriendContact.vue:26-52`

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

**Key Points:**
- Use `:prop-name` (v-bind) for dynamic values
- kebab-case in template → camelCase in JavaScript
- Props are **read-only** in child components

---

### 2. $emit: Child → Parent Communication

`$emit` allows child components to notify parent components about events.

**Code Location:** `src/components/FriendContact.vue:64-67` and `src/App.vue:17, 46-49`

#### How $emit Works

| Step | Component | Code | Description |
|------|-----------|------|-------------|
| 1 | Child | `this.$emit('toggle-favourte', this.id)` | Emit event with data |
| 2 | Parent | `@toggle-favourite="toggleFavouriteStatus"` | Listen for event |
| 3 | Parent | `toggleFavouriteStatus(friendId) { ... }` | Handle event |

#### Code Example

```vue
<!-- Child Component (FriendContact.vue) -->
<button @click="toggleFavourite">Toggle Favourite</button>

<script>
methods: {
  toggleFavourite() {
    // Emit 'toggle-favourte' event to parent component
    this.$emit('toggle-favourte', this.id);
  }
}
</script>
```

```vue
<!-- Parent Component (App.vue) -->
<friend-contact
  @toggle-favourite="toggleFavouriteStatus"
></friend-contact>

<script>
methods: {
  toggleFavouriteStatus(friendId) {
    const identifiedFriend = this.friends.find(friend => friend.id === friendId);
    identifiedFriend.isFavourite = !identifiedFriend.isFavourite;
  }
}
</script>
```

#### $emit Syntax

```javascript
this.$emit('event-name', payload)
//         └─────────┬────────┘  └─┬─┘
//              Event name      Optional data
```

---

### 3. Data Flow Diagram

```
┌─────────────────────────────────────────────┐
│          App.vue (Parent)                   │
│  ┌─────────────────────────────────┐       │
│  │ friends: [                       │       │
│  │   { id, name, isFavourite }      │       │
│  │ ]                                 │       │
│  └─────────────────────────────────┘       │
│               │                              │
│               │ Props ↓                      │
│               ↓                              │
│  ┌─────────────────────────────────┐       │
│  │   FriendContact.vue (Child)      │       │
│  │   Receives: id, name, isFavourite │       │
│  │   Button click triggers emit      │       │
│  └─────────────────────────────────┘       │
│               │                              │
│               │ $emit ↑                      │
│               ↑                              │
│  ┌─────────────────────────────────┐       │
│  │ toggleFavouriteStatus(friendId)  │       │
│  │ Updates friends array            │       │
│  └─────────────────────────────────┘       │
└─────────────────────────────────────────────┘
```

---

### 4. Array.find() Method

**Code Location:** `src/App.vue:47`

```javascript
const identifiedFriend = this.friends.find(friend => friend.id === friendId);
identifiedFriend.isFavourite = !identifiedFriend.isFavourite;
```

- `find()` returns the first element that matches the condition
- Used to locate a specific friend by ID and toggle their favourite status

---

### 5. v-for with Props

**Code Location:** `src/App.vue:10-18`

```vue
<friend-contact
  v-for="friend in friends"
  :key="friend.id"
  :id="friend.id"
  :name="friend.name"
  :is-favourite="friend.isFavourite"
></friend-contact>
```

- Loop through array and create multiple child components
- Each component receives different prop values
- `:key` is required for Vue to track each component

---

## Challenges

### 1. Method Name Mismatch

**Problem:** Event listener name didn't match the method name.

```vue
<!-- ❌ Wrong -->
@toggle-favourite="toggleFavourite"

methods: {
  toggleFavouriteStatus(friendId) { ... }
}
```

```vue
<!-- ✅ Correct -->
@toggle-favourite="toggleFavouriteStatus"

methods: {
  toggleFavouriteStatus(friendId) { ... }
}
```

**Lesson:** Event listener and method name must match exactly.

---

### 2. Understanding Props vs Local Data

**Challenge:** Props are read-only, so child components cannot directly modify them.

**Solution:**
- Child emits event to parent
- Parent modifies its own data
- Updated data flows back to child via props (reactivity)

```
Child cannot: props.isFavourite = !props.isFavourite ❌
Child must:   this.$emit('toggle', this.id) ✅
Parent then:  friend.isFavourite = !friend.isFavourite ✅
```

---

### 3. Naming Conventions

| Context | Format | Example |
|---------|--------|---------|
| Template attribute | kebab-case | `:is-favourite` |
| JavaScript prop | camelCase | `isFavourite` |
| Event name | kebab-case | `@toggle-favourite` |
| Method name | camelCase | `toggleFavouriteStatus` |

---

## Key Takeaways

1. **Props** = Parent sends data to child (one-way, read-only)
2. **$emit** = Child notifies parent about events (with optional data)
3. **Event names** must match between `$emit()` and `@event-name`
4. **Method names** must match between `@event="methodName"` and `methods: { methodName() {} }`
5. Props use **kebab-case** in templates, **camelCase** in JavaScript

---

## Installation

```bash
npm install
```

## Development

```bash
npm run serve
```xw