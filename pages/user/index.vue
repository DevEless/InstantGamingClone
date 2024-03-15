<template>
    <div>
        <p v-if="$fetchState.pending">Récupération des jeux</p>
        <p v-else-if="$fetchState.error">Une erreur est survenue :(</p>
        <div v-else>
            <h1>Nuxt</h1>
            <ul v-for="(game, index) of displayedGames" :key="index">

                <img :src="game.thumbnail" alt="">
                <li>{{ game.title }}</li>
            </ul>
            <button @click="fetchPreviousPage" :disabled="currentPage <= 1">Précédent</button>
            <button @click="fetchNextPage" :disabled="currentPage >= totalPages">Suivant</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            games: [],
            pageSize: 20,
            currentPage: 1
        }
    },
    computed: {
        displayedGames() {

            //debut et fin
            const startIndex = (this.currentPage - 1) * this.pageSize;
            const endIndex = startIndex + this.pageSize;
            return this.games.slice(startIndex, endIndex);
        },
        totalPages() {
            return Math.ceil(this.games.length / this.pageSize);
        }
    },
    async fetch() {
        await this.fetchGames();
    },
    methods: {
        async fetchGames() {
            this.games = await fetch('https://www.freetogame.com/api/games').then(res => res.json());
        },
        fetchNextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
        },
        fetchPreviousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
        }
    }
}
</script>
