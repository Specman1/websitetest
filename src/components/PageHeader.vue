<template>
    <div>
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <div class="container-fluid">
                <router-link class="navbar-brand" to="/">
                    <img src="@/assets/bklogo.png" alt="BKR Logo" />
                </router-link>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/')}">
                            <router-link class="nav-link px-0" aria-current="page" to="/">Home</router-link>
                        </li>
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/fares')}">
                            <router-link class="nav-link px-0" to="/fares">Timetables &amp; Fares</router-link>
                        </li>
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/plan')}">
                            <router-link class="nav-link px-0" to="/plan">Plan your visit</router-link>
                        </li>
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/events')}">
                            <router-link class="nav-link px-0" to="/events">Events</router-link>
                        </li>
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/special')}">
                            <router-link class="nav-link px-0" to="/special">Something Special</router-link>
                        </li>
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/page/about')}">
                            <router-link class="nav-link px-0" to="/page/about">About us</router-link>
                        </li>
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/news')}">
                            <router-link class="nav-link px-0" to="/news">News</router-link>
                        </li>
                        <li class="nav-item border-bottom border-3 ms-3" :class="{'border-primary': isActive('/page/museum')}">
                            <router-link class="nav-link px-0 text-primary" to="/page/museum">Museum</router-link>
                        </li>
                    </ul>
                    <div class="pe-2 text-center">
                        <a href="https://bkrailway.digitickets.co.uk"><img width="150px" src="@/assets/Click-for-tickets.png" alt="Book your Tickets" /></a>
                    </div>
                </div>
            </div>
        </nav>

        <div v-if="notification" class="alert" :class="'alert-'+notification.Style">
            <h3>{{ notification.Heading }}</h3>
            <p>{{ notification.Content }}</p>
        </div>
    </div>

</template>

<script>
    import axios from 'axios';

    export default {
        name: 'PageHeader',
        props: {
            
        },
        data: function() {
            return {
                notification: undefined,
            }
        },
        methods: {
            isActive: function(path) {
                return (path == this.$route.path);
            }
        },
        mounted: function() {

        },
        watch: {
            $route: function() {
                const fullpath = this.$route.fullPath.trim();
                const url = process.env.VUE_APP_ENDPOINT;
                const v = this;
                axios.get(url + '/Notifications?filter={ "Active": {"_eq": "true"}}')
                .then(response => {
                    const notifications = response.data.data;
                    this.notification = notifications.find(notification => {
                        return fullpath.startsWith(notification.PathMatch);
                    });
                })
                .catch(err => {
                    v.$log.error(err);
                })
            }
        },
    }
</script>