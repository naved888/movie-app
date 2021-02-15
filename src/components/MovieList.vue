<template>
    <div class="movies-section" :class="loader ? 'mt-32' : 'mt-48'">
        <div v-if="loader" class="d-flex justify-content-center align-items-center loader">
            <div class="spinner"></div>
        </div>
        <b-alert v-if="error" class="mt-5" show variant="danger">Something went wrong</b-alert>

        <div v-else>
            <img class="add-modal" v-b-modal.add-movie-modal
                src="https://previews.123rf.com/images/martialred/martialred1810/martialred181000015/112752943-add-new-video-or-upload-movie-create-media-flat-vector-icon-for-apps-and-websites.jpg"
                title="Add Movie" />
        </div>
        <div class="w-75 m-auto">
            <div class="movies-list">
                <div class="movie-card-wrapper" v-for="movie in movies" :key="movie.id">
                    <img :src="movie.Poster" alt="" class="img-fluid">
                    <div class="d-flex flex-column justify-content-between align-content-between">
                        <p class="text-sm p-1 wrap-text">{{movie.Title}}</p>
                        <div><button class="btn btn-warning text-md" @click='movieSelected(movie)'>Explore</button></div>
                    </div>
                </div>
            </div>
        </div>
        <b-modal id="movie-info" title="Movie History" title-class="text-md" :hide-footer="true" ok-title="Close">
            <div class="modal-wrapper text-md">
                <img :src="selectedMovie.Poster" alt="">
                <h4 class="text-md">Title: {{selectedMovie.Title}}</h4>
                <p><mark class="bg-color-yellow text-sm">Type: {{selectedMovie.Type}}</mark></p>
                <p><mark class="bg-color-yellow text-sm">Release Year: {{selectedMovie.Year}}</mark></p>
            </div>
        </b-modal>
        <b-modal id="add-movie-modal" dialog-class="new-movie" title-class="text-md" title="Add Movie" hide-footer centered
            @hide="hideModal()">
            <div class="d-flex text-md wrap">
                <div class="my-4 form-group pr-3 mr-3" :class="!isEmpty(inputFields) ? 'w-50 border-right ' : 'w-100'">
                    <form action="" class=" input-form text-md" v-on:submit.prevent='submitForm(inputFields)'>
                        <label class="text-sm" for="">Poster Url:</label>
                        <input type="url" name="" id="url" placeholder="Enter Poster url" class="form-control"
                            v-model.trim="inputFields.Poster" required><br>
                        <label class="text-sm" for="">Title:</label>
                        <input type="text" name="" id="title" placeholder="Enter Title" class="form-control"
                            v-model.trim="inputFields.Title" required><br>
                        <label class="text-sm" for="">Type:</label>
                        <input type="text" name="" id="type" placeholder="Enter Movie Type" class="form-control"
                            v-model.trim="inputFields.Type" required><br>
                        <label class="text-sm" for="">Year:</label>
                        <select id="dob" class="form-control" v-model.trim="inputFields.Year" required>
                            <option v-for="year in years" :value="year" :key="year">{{ year }}</option>
                        </select><br>
                        <div class="modal-footer">
                            <button type="submit" class="btn btn-danger" value="Reset">Submit</button>
                            <button type="reset" value="Reset" class="btn btn-danger" @click="reset()">Reset</button>
                        </div>
                    </form>
                </div>
                <div class="wrapper-preview input-form" v-if="!isEmpty(inputFields)">
                    <img :src="inputFields.Poster" alt="">
                    <h1 class="title wrap-text text-md p-2"> {{inputFields.Title}}</h1>
                    <h1 class="type wrap-text text-md p-2" v-if="inputFields.Type"><mark class="bg-color-yellow">{{inputFields.Type}}</mark></h1>
                    <h1 class="year wrap-text text-sm" v-if="inputFields.Year"><mark class="bg-color-yellow"> {{inputFields.Year}}</mark></h1>
                </div>
            </div>
        </b-modal>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            movies: [],
            loader: true,
            selectedMovie: {},
            inputFields: {},
            newMovies: [],
            error: '',
        }
    },
   async created() {
       await  setTimeout(() => {
            axios.get(`https://www.omdbapi.com/?apikey=e0620bd4&s=harry+potter`)
                .then(response => {
                    // JSON responses are automatically parsed.
                    this.movies = response.data.Search
                    this.loader = false;
                    console.log(this.movies)
                })
                .catch(e => {
                    this.error = e;
                    this.loader = false;
                })
        }, 200)
    },
    methods: {
        movieSelected(movie) {
            this.selectedMovie = movie;
            this.$bvModal.show('movie-info')
        },
        submitForm(movieData) {
            if (movieData.Title && movieData.Poster && movieData.Type && movieData.Year) {
                this.movies.push({
                    Title: movieData.Title,
                    Poster: movieData.Poster,
                    Type: movieData.Type,
                    Year: movieData.Year
                });
                this.$bvModal.hide('add-movie-modal');
                this.inputFields = {}
            }
        },
        getYear() {
            const year = new Date().getFullYear()
            console.log(year)
        },
        isEmpty(inputFields) {
            return Object.keys(inputFields).length === 0;
        },
        reset() {
            this.inputFields = {};
        },
        hideModal() {
            this.inputFields = {};
        }
    },

    computed: {
        years() {
            const year = new Date().getFullYear()
            return Array.from({
                length: year - 1900
            }, (value, index) => 1901 + index)
        },

    },
}
</script>

<style lang="scss">@import "../assets/styles/app.scss";


.new-movie {
    max-width: 991px !important;
}

.movies-section {
    background: $white;

    .spinner {
        width: 40px;
        height: 40px;
        background-color: #333;
        display: block;
        -webkit-animation: sk-rotateplane 1.2s infinite ease-in-out;
        animation: sk-rotateplane 1.2s infinite ease-in-out;
    }

    .add-modal {
        width: 70px;
        float: right;
        margin-right: 11px;
        position: fixed;
        right: 0;
        top: 98px;
        border: 2px double black;
        -webkit-box-shadow: 2px 4px 13px 0px rgba(0, 0, 0, 0.29);
        -moz-box-shadow: 2px 4px 13px 0px rgba(0, 0, 0, 0.29);
        box-shadow: 2px 4px 13px 0px rgba(0, 0, 0, 0.29);

        @media (max-width: 360px) {
            width: 46px;
        }
    }

    .movies-list {
        display: grid;
        grid-template-columns: repeat(auto-fill, 200px);
        gap: 30px;
        position: relative;
        justify-content: center;
        margin-bottom: 60px;
        z-index: 1;
        top: 51px;

        .movie-card-wrapper {
            text-align: center;
            -webkit-box-shadow: 0px 0px 13px -2px rgba(0, 0, 0, 0.29);
            -moz-box-shadow: 0px 0px 13px -2px rgba(0, 0, 0, 0.29);
            box-shadow: 0px 0px 13px -2px rgba(0, 0, 0, 0.29);
            padding-bottom: 8px;
            display: grid;
            animation-name: power;
            width: 200px;
            animation-duration: 1s;

            img {
                height: 300px;
                width: 100%;
                object-fit: fill;
            }
        }
    }

    .loader {
        height: 100vh;
    }
}
.wrap {
    @media screen and (max-width: 525px) {
        flex-direction: column;
        flex-direction: column-reverse;
    }

    .form-group {
        @media screen and (max-width: 525px) {
            width: 100% !important;
            border: none !important;
        }
    }

    .wrapper-preview {
        text-align: center;
        box-shadow: 4px 5px 12px rgb(107, 103, 103);
        padding-bottom: 8px;
        animation-name: power;
        animation-duration: 1s;
        margin: auto;
        width: 250px;

        img {
            width: 100%;
            object-fit: contain;
        }

        @media screen and (max-width: 525px) {
            width: 200px !important;
        }
    }
}
.modal-wrapper {
    text-align: center;
    h4 {
        padding-top: 5px;
    }
    img {
        width: 200px;
    }
}

@keyframes power {
    0% {
        transform: scale(0.5, 0.5);
    }
    100% {
        transform: scale();
    }
}
@-webkit-keyframes sk-rotateplane {
    0% {
        -webkit-transform: perspective(120px)
    }

    50% {
        -webkit-transform: perspective(120px) rotateY(180deg)
    }

    100% {
        -webkit-transform: perspective(120px) rotateY(180deg) rotateX(180deg)
    }
}

@keyframes sk-rotateplane {
    0% {
        transform: perspective(120px) rotateX(0deg) rotateY(0deg);
        -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg)
    }

    50% {
        transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
        -webkit-transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg)
    }

    100% {
        transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
        -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
    }
}
</style>