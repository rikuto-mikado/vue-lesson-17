<template>
  <li>
    <!-- Display name with favourite status from props (isFavourite) -->
    <!-- Ternary operator: if isFavourite is true, display '(Favourite)', otherwise display empty string -->
    <h2>{{ name }} {{ isFavourite ? '(Favourite)' : ''}}</h2>
    <button @click="toggleFavourite">Toggle Favourite</button>
    <button @click="toggleDetails">{{ detailsVisible ? 'Hide' : 'Show' }} Details</button>
    <ul v-if="detailsVisible">
        <li><strong>Phone: </strong>{{ phoneNumber }}</li>
        <li><strong>Email: </strong>{{ emailAdress }}</li>
    </ul>
  </li>
</template>

<script>
export default {
    // Data received from parent component (read-only)
    // props: [
    //     'name',
    //     'phoneNumber',
    //     'emailAdress',
    //     'isFavourite'
    // ],

    // Switched to object syntax for explicit type validation and better prop definition
    props: {
        id: {
            type: String,
            required: true
        },
        name: {
            type: String,
            required: true,
        },
        phoneNumber: {
            type: String,
            required: true,
        },
        emailAdress: {
            type: String,
            required: true,
        },
        isFavourite: {
            type: Boolean,
            required: false,
            default: false,
            // Boolean type automatically validates true/false values from parent component
            // When passed from App.vue, string values are auto-converted:
            // - is-favourite="true" or :is-favourite="true" → true
            // - is-favourite="false" or :is-favourite="false" → false
            // - is-favourite (no value) → true
        }
    },
    // Declare custom events that this component can emit to parent
    // Helps with documentation and IDE autocomplete
    // Best practice: explicitly list all events this component emits
    // emits: [
    //     'toggle-favourte'
    // ],
    emits: {
        'toggle-favourite': function(id) {
            if (id) {
                return true;
            } else {
                // Display warning in browser console (for development/debugging)
                console.warn('Id is missing!!')
                return false;
            }
        }
    },
    data() {
        return {
            detailsVisible: false
        };
    },
    methods: {
        toggleDetails() {
            this.detailsVisible = !this.detailsVisible;
        },
        // Modify local friendIsFavourite (not a prop, so mutation is allowed)
        toggleFavourite() {
            // Emit 'toggle-favourte' event to parent component
            this.$emit('toggle-favourte', this.id);
        },
    },
};
</script>

