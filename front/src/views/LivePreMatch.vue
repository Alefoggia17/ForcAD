<template>
    <div class="screen">
        <button class="ponies-toggle-btn" @click="togglePonies">
            Buona fortuna e fai GOAL!
        </button>
        <iframe
            v-if="showPonies"
            src="https://www.youtube.com/embed/MO4Mmo5oykw?autoplay=1"
            class="pony"
            width="640"
            height="480"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
            title="pony-video"
        ></iframe>
        <live-scoreboard />
    </div>
</template>

<script>
import LiveScoreboard from '@/components/Scores/LiveScoreboard.vue';
import { mapState } from 'vuex';

export default {
    components: {
        LiveScoreboard,
    },

    computed: mapState(['showPonies']),

    methods: {
        togglePonies: function () {
            this.$store.commit('togglePonies');
        },
    },
};
</script>

<style lang="scss" scoped>
.screen {
    width: 100%;
    height: 100%;
    background-color: black;
}

.pony {
    position: fixed;
    overflow: hidden;
    border-style: none;
    margin: 0;
    padding: 0;
    background: transparent;
    width: 100%;
    height: 100%;
    z-index: 2;
}

.ponies-toggle-btn {
    z-index: 3;
    position: absolute;
    right: 1em;
}
</style>
