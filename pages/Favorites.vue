<template>
    <div class="favorites-container">
        <h1>Vos Favoris</h1>
        <div v-if="favorites.length">
            <div class="game-item" v-for="game in favorites" :key="game.id">
                <img :src="game.thumbnail" alt="Thumbnail" class="game-thumbnail" />
                <h2>{{ game.title }}</h2>
                <p>{{ game.description }}</p>
                <button @click="removeFromFavorites(game.id)">Retirer des favoris</button>
            </div>
        </div>
        <div v-else>
            <p>Vous n'avez pas encore ajouté de jeux à vos favoris.</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            favorites: []
        }
    },
    mounted() {
        this.loadFavorites();
    },
    methods: {
        loadFavorites() {
            this.favorites = JSON.parse(localStorage.getItem('favorites')) || [];
        },
        removeFromFavorites(id) {
            this.favorites = this.favorites.filter(game => game.id !== id);
            localStorage.setItem('favorites', JSON.stringify(this.favorites));
        }
    }
}
</script>

<style>
.favorites-container{
    color: #fff;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
}
</style>