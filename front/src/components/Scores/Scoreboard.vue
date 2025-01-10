<template>
    <div class="scoreboard-container">
        <div class="header">
            <h1 class="napoli-title">Napnwli Scoreboard</h1>
            <div class="action-buttons" v-if="admin">
                <button class="napoli-btn" @click="$emit('createTeam')">Create team</button>
                <button class="napoli-btn" @click="$emit('createTask')">Create task</button>
            </div>
        </div>
        <score-table
            v-if="teams !== null"
            head-row-title="#"
            :team-clickable="true"
            :tasks="tasks"
            :teams="teams"
            @openTeam="openTeam"
        />
    </div>
</template>

<script>
import ScoreTable from '@/components/Lib/ScoreTable.vue';
import { mapState } from 'vuex';

export default {
    components: {
        ScoreTable,
    },

    computed: mapState(['teams', 'tasks']),

    methods: {
        openTeam: function (id) {
            this.$router.push({ name: 'team', params: { id } }).catch(() => {});
        },
    },
};
</script>

<style lang="scss" scoped>
$napoli-blue: #13214F;
$napoli-light-blue: #00A1E0;

.scoreboard-container {
    margin: 1rem;
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    background: $napoli-blue;
    border-radius: 8px 8px 0 0;
    margin-bottom: -1rem;
}

.napoli-title {
    color: white;
    font-size: 1.5rem;
    margin: 0;
}

.action-buttons {
    display: flex;
    gap: 1rem;
}

.napoli-btn {
    background: $napoli-light-blue;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
    transition: background 0.2s;

    &:hover {
        background: lighten($napoli-light-blue, 10%);
    }
}

</style>
