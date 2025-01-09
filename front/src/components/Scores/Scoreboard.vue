<template>
    <score-table
        v-if="teams !== null"
        head-row-title="#"
        :team-clickable="true"
        :tasks="tasks"
        :teams="teams"
        @openTeam="openTeam"
    />
</template>

<script>
import ScoreTable from '@/components/Lib/ScoreTable.vue';
import { mapState } from 'vuex';

export default {
    components: {
        ScoreTable,
    },

    data() {
        return {
            teams: [
                { id: 1, name: 'Napoli Azzurri', score: 2500, ip: '192.168.1.1', tasks: [], highlighted: false },
                { id: 2, name: 'Partenopei Elite', score: 2245, ip: '192.168.1.2', tasks: [], highlighted: false },
                { id: 3, name: 'Maradona Legacy', score: 1987, ip: '192.168.1.3', tasks: [], highlighted: false },
                { id: 4, name: 'San Paolo Stars', score: 1856, ip: '192.168.1.4', tasks: [], highlighted: false },
                { id: 5, name: 'Vesuvio Warriors', score: 1750, ip: '192.168.1.5', tasks: [], highlighted: false },
            ],
            tasks: [
                { name: 'Service 1', id: 1 },
                { name: 'Service 2', id: 2 },
                { name: 'Service 3', id: 3 },
            ],
        };
    },

    methods: {
        openTeam(id) {
            this.$router.push({ name: 'team', params: { id } }).catch(() => {});
        },
    },
};
</script>

<style lang="scss" scoped></style>