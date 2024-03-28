<template>
    <div class="body">
        <div v-if="loading">Chargement...</div>
        <div class="game-details-container" v-else-if="game">
            <div class="game-image-wrapper">

                <h1 class="game-title">{{ game.title }}</h1>
                <img :src="game.thumbnail" :alt="'Thumbnail de ' + game.title" />

                <button class="add-to-favorites-btn" @click="addToFavorites(game)">Ajouter aux favoris</button>

                <p>{{ game.description }}</p>
            </div>
        </div>
        <div v-else>
            <p>Impossible de charger les détails du jeu </p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            loading: true,
            game: null,
        };
    },
    async created() {
        await this.fetchGameDetails();
    },
    methods: {
        async fetchGameDetails() {
            this.loading = true;
            try {
                const gameId = this.$route.params.id; // L'ID du jeu dans l'URL
                const response = await fetch('https://mainapppro.netlify.app/public/jeu.json');
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                const games = await response.json();
                this.game = games.find(game => game.id.toString() === gameId);
                if (!this.game) {
                    throw new Error(`Game with ID ${gameId} not found.`);
                }
            } catch (error) {
                console.error('Error fetching game details:', error);
                this.game = null;
            } finally {
                this.loading = false;
            }
        },
        addToFavorites(game) {
            const isLoggedIn = localStorage.getItem('isLoggedIn'); // Vérifie si l'utilisateur est connecté
            if (!isLoggedIn) {
                alert('Veuillez vous connecter pour ajouter des jeux aux favoris.');
                this.$router.push('/login'); // Redirige vers la page de connexion
                return;
            }

            let favorites = JSON.parse(localStorage.getItem('favorites')) || [];
            if (!favorites.some(favoriteGame => favoriteGame.id === game.id)) {
                favorites.push(game);
                localStorage.setItem('favorites', JSON.stringify(favorites));
                alert('Ajouté aux favoris !');
            } else {
                alert('Ce jeu est déjà dans vos favoris.');
            }
        }

    },
};
</script>

<style scoped>
body {
    width: 100%;
    margin: 0px;
    color: #fefefe !important;
    font-family: 'Barlow', sans-serif;
    background-color: #272727;
    display: flex;
}

.game-details-container {
    display: flex;
}

.game-image-wrapper {
    flex: 1;
    display: flex;
    justify-content: center;
align-items: center;
flex-direction: column;
}

.game-cover {
    width: 100%;
    border-radius: 8px;
}

.game-info-wrapper {
    flex: 2;

}

.game-title {
    font-size: 2em;
    margin-bottom: 20px;
}

.game-price {
    font-size: 1.5em;
    margin-bottom: 20px;
}

.add-to-cart-btn {
    background-color: orange;
    border: none;

    font-size: 1em;
    cursor: pointer;
    border-radius: 4px;
}

.add-to-cart-btn:hover {
    background-color: #e69500;
}
h1{
    color: #fefefe;
}
button{
    width: 20%;
    margin-top: 4%;
}
</style>