<template>
    <nav class="navbar">
        <div class="nav-brand">Instantanément dans le Game</div>
        <div class="nav-menu">
            <ul class="nav-links">
                <li><nuxt-link to="/" class="nav-link">Accueil</nuxt-link></li>
                <li><nuxt-link to="/favorites" class="nav-link">Favoris</nuxt-link></li>
                <li><nuxt-link to="/login" class="nav-link">Login</nuxt-link></li>
            </ul>
            <button @click="logout" class="logout-button">Déconnexion</button>
        </div>
    </nav>
</template>

<script>
export default {
    methods: {
        logout() {
            localStorage.removeItem('isLoggedIn');
            this.$router.push('/login');
        }
    }
}
</script>

<style scoped>
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #333;
    color: #fff;
    padding: 10px 20px;
}

.nav-brand {
    font-weight: bold;
    color: #ffa500;

}

.nav-menu {
    display: flex;
    align-items: center;
}

.nav-links {
    list-style: none;
    display: flex;
    align-items: center;
    padding: 0;
    margin: 0;
}

.nav-link {
    color: #fff;
    text-decoration: none;
    padding: 10px 15px;
    transition: color 0.3s;
}

.nav-link:hover {
    color: #ffa500;

}

.logout-button {
    background-color: #ffa500;

    border: none;
    border-radius: 4px;
    padding: 10px 20px;
    margin-left: 15px;

    cursor: pointer;
    color: #fff;
    transition: background-color 0.3s;
}

.logout-button:hover {
    background-color: #cc8400;

}
</style>