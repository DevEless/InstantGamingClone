<template>
    <div class="content">
        <div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
            <ol class="carousel-indicators">
                <li data-target="#carouselExampleIndicators" data-slide-to="0" class="active"></li>
                <li data-target="#carouselExampleIndicators" data-slide-to="1"></li>
                <li data-target="#carouselExampleIndicators" data-slide-to="2"></li>
                <li data-target="#carouselExampleIndicators" data-slide-to="3"></li>
                <li data-target="#carouselExampleIndicators" data-slide-to="4"></li>
            </ol>
            <div class="carousel-inner">

                <div class="carousel-item" v-for="(game, index) in gamesToShow" :key="game.id" :class="{ active: index === 0 }">
                    <img class="d-block w-100" :src="game.thumbnail" :alt="`Slide ${index + 1}`">
                </div>
            </div>
            <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="sr-only">Previous</span>
            </a>
            <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="sr-only">Next</span>
            </a>
        </div>

        <h1 class="trend">Précommandes</h1>

        <div class="search-container">
            <input class="input" type="text" v-model="searchQuery" placeholder="Rechercher un jeu...">
        </div>
        <div class="games-container">

            <div class="game-item" v-for="game in paginatedFilteredGames" :key="game.id" @click="redirectToGamePage(game.id)">
                <img class="game-thumbnail" :src="game.thumbnail" alt="Thumbnail" />
                <p class="game-title">{{ game.title }}</p>
            </div>
        </div>
        <div class="pagination">
            <button class="button" @click="previousPage" :disabled="currentPage === 1">Précédent</button>
            <button class="button" @click="nextPage" :disabled="currentPage >= totalPages">Suivant</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            games: [],
            currentPage: 1,
            pageSize: 21,
            searchQuery: '', 
        };
    },
    computed: {
        totalPages() {
            // Calcul basé sur les jeux filtrés pour la pagination
            return Math.ceil(this.filteredGames.length / this.pageSize);
        },
        filteredGames() {

            return this.searchQuery ?
                this.games.filter(game => game.title.toLowerCase().includes(this.searchQuery.toLowerCase())).sort((a, b) => a.title.localeCompare(b.title)) :
                this.games;
        },
        paginatedFilteredGames() {

            const start = (this.currentPage - 1) * this.pageSize;
            const end = start + this.pageSize;
            return this.filteredGames.slice(start, end);
        },
        gamesToShow() {

            return this.games.slice(0, 5);
        },
    },
    async created() {
        await this.fetchGames();
    },
    methods: {
        async fetchGames() {
            try {
                const response = await fetch(`https://mainapppro.netlify.app/public/jeu.json`);
                const data = await response.json();
                this.games = data;
            } catch (error) {
                console.error('Erreur lors de la récupération des jeux:', error);
            }
        },
        redirectToGamePage(gameId) {
            this.$router.push(`/game/${gameId}`);
        },
        previousPage() {
            if (this.currentPage > 1) this.currentPage--;
        },
        nextPage() {
            if (this.currentPage < this.totalPages) this.currentPage++;
        },
    },
};
</script>


<style>
.input{
    width: 25% !important;
    margin-top: 2%;
    margin-bottom:2%;
}
.trend{
    margin-top: 5%;
}
.carousel {
    display: flex;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
}

.carousel-item {
    scroll-snap-align: start;
    flex: none;
    width: 100%;

}

body {
    background-color: #272727;
}

.content {
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    color: #fefefe;
    font-family: 'Barlow', sans-serif;
    background-color: #272727;
}

.games-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}

.game-item {
    cursor: pointer;
    margin-bottom: 20px;
}

.game-thumbnail {
    border-radius: 10px;
    max-width: 326px;
    height: auto;
    transition: transform .3s ease;
}

.game-thumbnail:hover {
    transform: scale(1.05);
}

.game-title {
    font-size: 15px;
    text-align: center;
    margin-top: 10px;
}

.button-container {
    display: flex;
    justify-content: center;
}

.button {
    background-image: linear-gradient(-180deg, #FF7E31, #E62C03);
    border-radius: 6px;
    box-shadow: rgba(0, 0, 0, 0.1) 0 2px 4px;
    color: #FFFFFF;
    cursor: pointer;
    font-size: 16px;
    font-family: Inter, -apple-system, system-ui, Roboto, "Helvetica Neue", Arial, sans-serif;
    padding: 10px 20px;
    outline: none;
    border: none;
    transition: box-shadow .2s;
    margin: 20px 0;
    width: 25%;
}

.button:hover {
    box-shadow: rgba(253, 76, 0, 0.5) 0 3px 8px;
}

.pagination {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}
img{
    width: 320px;
}
</style>
