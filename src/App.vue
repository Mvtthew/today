<template>
    <div id="app">
        <div class="container my-5">
            <div class="welcome">
                <h1 class="display-2">
                    today is <span>{{today.date}}</span>
                </h1>
                <p>
                    Do you know what happened today?
                </p>
            </div>
            <div class="row mt-5">
                <div class="col-lg-8">
                    <div class="card">
                        <div class="card-header">
                            <h1 class="display-4">
                                Event
                            </h1>
                        </div>
                        <div class="card-body">

                        </div>
                    </div>

                </div>
                <div class="col-lg-4">
                    <div class="card">
                        <div class="card-header">
                            <p class="h3 font-weight-light">Born today</p>
                            <hr>
                            <img :src="birth.image" class="img-thumbnail img-fluid mb-2">

                            <p class="h4 font-weight-light mb-1">
                                {{ birth.name }} <span class="small"> ({{ birth.died ? birth.diedYear - birth.bornYear : today.year - birth.bornYear}}) </span>
                            </p>
                            <p class="small m-0">
                                b. {{ birth.bornYear }}
                            </p>
                            <p class="small m-0" v-if="birth.died">
                                d. {{ birth.diedYear }}
                            </p>
                        </div>
                        <div class="card-body">
                            <p class="h5 font-weight-light m-0">
                                {{ birth.desc }}
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'today',
        data() {
            return {
                today: {
                    year: '',
                    text: '',
                    url: ''
                },
                event: {
                    text: '',
                    year: ''
                },
                birth: {
                    text: '',
                    name: '',
                    desc: '',
                    bornYear: '',
                    died: false,
                    diedYear: '',
                    year: '',
                    image: ''
                }
            }
        },
        created() {
            this.getToday();
        },
        methods: {
            getToday() {
                fetch('https://cors-anywhere.herokuapp.com/history.muffinlabs.com/date')
                    .then(res => res.json())
                    .then(data => {
                        // get image

                        // get and save data
                        this.today = data;
                        const now = new Date();
                        this.today.year = now.getFullYear();

                        // randomize event
                        this.event = this.today.data.Events[parseInt(Math.random() * this.today.data.Events.length)];

                        // randomize birth
                        this.birth = this.today.data.Births[parseInt(Math.random() * this.today.data.Births.length)];
                        // parse data
                        this.birth.name = this.birth.text.substring(0, this.birth.text.indexOf(','));
                        if (this.birth.name == '') {
                            this.getToday();
                        } else {
                            this.birth.bornYear = this.birth.year;

                            this.getBirthImage();

                            // check if died
                            if (this.birth.text.indexOf('(d.') > 0) {
                                // died
                                this.birth.desc = this.birth.text.substring(this.birth.text.indexOf(',') + 2, this.birth.text.indexOf('(d.'));
                                this.birth.died = true;
                                this.birth.diedYear = this.birth.text.substring(this.birth.text.indexOf('(d.') + 4, this.birth.text.length - 1);
                            } else {
                                // NOT died
                                this.birth.desc = this.birth.text.substring(this.birth.text.indexOf(',') + 2);
                                this.birth.died = false;
                                this.birth.diedYear = '';
                            }
                        }
                    });
            },
            getBirthImage() {
                fetch('https://cors-anywhere.herokuapp.com/api.qwant.com/api/search/images?q=' + this.birth.name + '&t=images&count=1&safesearch=1&locale=en_us&uiv=4')
                    .then(res => res.json())
                    .then(data => this.$set(this.birth, 'image', data.data.result.items[0].media));
            }
        }
    }
</script>

<style lang="scss">
</style>
