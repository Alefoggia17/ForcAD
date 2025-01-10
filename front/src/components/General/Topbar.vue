<template>
    <div class="topbar">
        <router-link class="tp" to="/live/"> Gasati con me </router-link>
         <router-link class="tp" to="/pre-match/"> Entra nello spogliatoio prima del match</router-link>
        <div class="progress-bar" :style="{ width: `${roundProgress}%` }" />
        <div class="tp">Round: {{ round }}</div>
    </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
    beforeRouteLeave: function (to, from, next) {
        clearInterval(this.timer);
        next();
    },
    data: function () {
        return {
            timer: null,
        };
    },

    computed: mapState(['round', 'roundProgress']),

    created: async function () {
        await this.$store.dispatch('fetchRoundTime');
        this.timer = setInterval(
            () => this.$store.dispatch('calculateRoundProgress'),
            500
        );
    },
};
</script>

<style lang="scss" scoped>
.tp {
    z-index: 1;
}

.topbar {
    position: relative;
    background: #bbbbbb55;
    padding: 1em;

    display: flex;
    flex-flow: row nowrap;

    justify-content: space-between;
    align-items: center;
}

.progress-bar {
    background-color: #00A1E0;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0.5;
}
</style>
