<template>
    <div>
        <header
            :style="{
                'background-image': `url(${headerBackgroundImage})`
            }"
        >
            <b-img
                thumbnail
                rounded="circle"
                :src="profileImg"
                class="profile-img mt-4"
                alt="Indiana"
            ></b-img>
        </header>
        <b-container>
            <h1 class="display-1 mt-5 mb-0">
                Indiana <span class="surname">Lown-Collins</span>
            </h1>
            <h2 class="text-muted pb-3 mb-3">
                <em>Award-Winning Theatre Director</em>
            </h2>
        </b-container>
    </div>
</template>

<script>
    import defaultProfilePic from '../assets/profilePic.jpeg'

    export default {
        name: 'greetingHeader',

        props: ['headerBackgroundImage', 'profileImage'],

        computed: {
            profileImg() {
                const userUpload = this.profileImage
                return userUpload
                    ? userUpload.split('/public').join('')
                    : defaultProfilePic
            }
        }
    }
</script>

<style scoped>
    header {
        background: black;
        background-size: cover;
        max-height: 230px;
    }

    .profile-img {
        height: 250px;
        aspect-ratio: 1/1;
        object-fit: cover;
    }

    .surname {
        white-space: nowrap;
    }
</style>
