<template>
    <div id="app">
        <div class="container my-5">
            <div class="welcome">
                <h1 class="display-2">
                    TODAY IS
                </h1>
                <p>
                    and what happened today?
                </p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                today: {}
            }
        },
        created() {
            this.getToday();
        },
        methods: {
            getToday() {
                fetch('http://history.muffinlabs.com/date', {
                    method: 'GET',
                    headers: {
                        'Access-Control-Allow-Origin': '*'
                    }

                }).then(res => res.json()).then(data => this.today = data);
            }
        }
    }
</script>

<style lang="scss">
</style>
