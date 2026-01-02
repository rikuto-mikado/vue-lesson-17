<template>
  <li>
    <!-- Use local friendIsFavourite (props cannot be mutated directly) -->
    <h2>{{ name }} {{ friendIsFavourite === '1' ? '(Favourite)' : ''}}</h2>
    <button @click="toggleDetails">Toggle Favourite</button>
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
    props: [
        'name',
        'phoneNumber',
        'emailAdress',
        'isFavourite'
    ],
    data() {
        return {
            detailsVisible: false,
            friend: {
                id: 'rikuto',
                name: 'Rikuto Mikado',
                phone: '0123-45678-90',
                email: 'rikuto@example.com',
            },
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
            if (this.friendIsFavourite === '1') {
                this.friendIsFavourite = '0';
            } else {
                this.friendIsFavourite = '1';
            }
        }
    }
};
</script>

