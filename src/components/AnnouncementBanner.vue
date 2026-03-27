<template>
    <b-jumbotron
        header="Indiana will be directing Love's Labour's Lost at Shakespeare's Globe"
        lead="Showing 17 July to 13 September 2026"
        fluid
        class="announcement-jumbotron text-white py-5"
    >
        <p class="mt-5">
            
        </p>
        <b-button
            size="lg"
            variant="custom-primary"
            @click="handleClick('shows')"
        >
            Find Out More
        </b-button>
    </b-jumbotron>
</template>

<script>
    export default {
        name: 'AnnouncementBanner',

        methods: {
            handleClick(refName) {
                this.$emit('announcementClicked', refName)
            }
        }
    }
</script>

<style >
    .announcement-jumbotron {
        background: rgba(0, 50, 61, 0.698);
        background-image: url(../assets/globe.jpeg);
        background-size: cover;
        background-position: center;
        background-blend-mode: darken;
    }
</style>
