<template>
    <div class="login-container">
        <h1>Connexion</h1>
        <form @submit.prevent="login">
            <div>
                <label for="username">Nom d'utilisateur:</label>
                <input id="username" v-model="username" type="text" required>
            </div>
            <div>
                <label for="password">Mot de passe:</label>
                <input id="password" v-model="password" type="password" required>
            </div>
            <button type="submit">Se connecter</button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            username: '',
            password: ''
        }
    },
    methods: {
        login() {
            if (this.username === 'Quentin' && this.password === 'molengeek') {
                localStorage.setItem('isLoggedIn', true);
                this.$router.push('/'); // Rediriger l'utilisateur vers la page d'accueil après une connexion réussie.
            } else {
                alert('Nom d’utilisateur ou mot de passe incorrect.');
            }
        }
    }
}
</script>

<style>

.login-container {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
}

.login-container div {
    margin-bottom: 10px;
}

label {
    display: block;
}

input[type="text"],
input[type="password"] {
    width: 100%;
    padding: 8px;
}

button {
    width: 100%;
    padding: 10px;
    background-color: #007bff;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}
.login-container{
    color: #fff;
}
</style>