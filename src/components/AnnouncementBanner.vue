<template>
    <b-jumbotron
        header="Just Announced: Indiana will be directing Love Labour's Lost at Shakespeare's Globe"
        lead="Summer 2026"
        fluid
        class="announcement-jumbotron text-white py-5"
    >
        <p class="mt-5">
            
        </p>
        <b-button
            size="lg"
            variant="custom-primary"
            href="https://www.jmktrust.org/2022/07/jmk-award-winner-2022/"
            target="_blank"
        >
            Find Out More
        </b-button>
    </b-jumbotron>
</template>

<script>
    export default {
        name: 'AnnouncementBanner',
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
