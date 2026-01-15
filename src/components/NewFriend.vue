<template>
    <!-- @submit.prevent listens for submit event and prevents default page reload -->
    <form @submit.prevent="submitData">
        <div>
            <label>Name</label>
            <!-- v-model creates two-way data binding between input and data property -->
            <input type="text" v-model="enteredName"/>
        </div>
        <div>
            <label>Phone</label>
            <input type="tel" v-model="enteredPhone"/>
        </div>
        <div>
            <label>Email</label>
            <input type="email" v-model="enteredEmail"/>
        </div>
        <div>
            <button>Add Contact</button>
        </div>
    </form>
</template>

<script>
export default {
    emits: ['add-contact'],
    data() {
        // Reactive state for form input fields
        return {
            enteredName: '',
            enteredPhone: '',
            enteredEmail: ''
        };
    },
    methods: {
        submitData() {
            this.$emit(
                'add-contact',
                this.enteredName,
                this.enteredPhone,
                this.enteredEmail
            );
        },
    },
};
</script>