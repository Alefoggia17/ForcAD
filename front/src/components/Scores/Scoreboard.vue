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

