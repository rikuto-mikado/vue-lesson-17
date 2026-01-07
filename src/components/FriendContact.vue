<template>
  <li>
    <!-- Use local friendIsFavourite (props cannot be mutated directly) -->
    <!-- Ternary operator: if friendIsFavourite is true, display '(Favourite)', otherwise display empty string -->
    <h2>{{ name }} {{ friendIsFavourite ? '(Favourite)' : ''}}</h2>
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
    data() {
        return {
            detailsVisible: false,
            // Important: Props cannot be mutated directly, so copy to local data
            friendIsFavourite: this.isFavourite
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

