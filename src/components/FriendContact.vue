<template>
  <li>
    <h2>{{ name }}</h2>
    <button @click="toggleDetails">{{ detailsVisible ? 'Hide' : 'Show' }} Details</button>
    <ul v-if="detailsVisible">
        <li><strong>Phone: </strong>{{ phoneNumber }}</li>
        <li><strong>Email: </strong>{{ emailAdress }}</li>
    </ul>
  </li>
</template>

<script>
export default {
    props: [
        'name', 
        'phoneNumber', 
        'emailAdress'
    ],
    data() {
        return {
            detailsVisible: false,
            friend: {
                id: 'rikuto',
                name: 'Rikuto Mikado',
                phone: '0123-45678-90',
                email: 'rikuto@example.com',
            }
        };
    },
    methods: {
        toggleDetails() {
            this.detailsVisible = !this.detailsVisible;
            
        }
    }
};
</script>

