<template>
    <div class="scoreboard-container">
        <div class="header">
            <h1 class="napoli-title">Napwnli Scoreboard</h1>
            <div class="action-buttons" v-if="admin">
                <button class="napoli-btn" @click="$emit('createTeam')">Create Team</button>
                <button class="napoli-btn" @click="$emit('createTask')">Create Task</button>
            </div>
        </div>
        <score-table
            v-if="teams !== null"
            class="table"
            head-row-title="#"
            :team-clickable="true"
            :tasks="tasks"
            :teams="teams"
            :admin="admin"
            @openTeam="openTeam"
        />
    </div>
</template>

<script>
import ScoreTable from '@/components/Lib/ScoreTable.vue';

export default {
    components: {
        ScoreTable,
    },

    computed: {
        teams() {
            return this.$store.state.teams || []; // Default a un array vuoto
        },
        tasks() {
            return this.$store.state.tasks || [];
        },
    },

    props: {
        admin: {
            type: Boolean,
            default: false,
        },
    },

    methods: {
        openTeam(id) {
            this.$router.push({ name: 'team', params: { id } }).catch(() => {});
        },
    },
};
</script>

<style lang="scss" scoped></style>

