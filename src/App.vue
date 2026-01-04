<template>
  <section>
    <header>
      <h1>My Friends</h1>
    </header>
    <ul>
      <!-- Pass props to child component (FriendContact.vue) -->
      <!-- is-favourite must be "1" (favourite) or "0" (not favourite) to pass validation -->
      <friend-contact
        name="Rikuto Mikado"
        phone-number="0123-45678-90"
        email-adress="rikuto@example.com"
        is-favourite="1"
      ></friend-contact>
      <!-- When is-favourite is omitted, defaults to "0" in child component -->
      <friend-contact
        name="Jun Sato"
        phone-number="0987-65432-10"
        email-adress="jun@example.com"
      ></friend-contact>
    </ul>
  </section>
</template>

<script>
export default {
    data() {
        return {
            friends: [
                {
                    id: 'rikuto',
                    name: 'Rikuto Mikado',
                    phone: '0123-45678-90',
                    email: 'rikuto@example.com',
                },
                {
                    id: 'jun',
                    name: 'Jun Sato',
                    phone: '0987-65432-10',
                    email: 'jun@example.com',
                }
            ]
        }
    },
    methods: {

    },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Jost&display=swap');

* {
  box-sizing: border-box;
}

html {
  font-family: 'Jost', sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: #58004d;
  color: white;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}

#app ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

#app li {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 1rem auto;
  border-radius: 10px;
  padding: 1rem;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}

#app h2 {
  font-size: 2rem;
  border-bottom: 4px solid #ccc;
  color: #58004d;
  margin: 0 0 1rem 0;
}

#app button {
  font: inherit;
  cursor: pointer;
  border: 1px solid #ff0077;
  background-color: #ff0077;
  color: white;
  padding: 0.05rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}

#app button:hover,
#app button:active {
  background-color: #ec3169;
  border-color: #ec3169;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}
</style>