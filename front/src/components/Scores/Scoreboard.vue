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

export default {
    components: {
        ScoreTable,
    },

    computed: mapState(['teams', 'tasks']),

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

/* Aggiunta di stili per .table e .row */
.table {
    display: flex;
    flex-flow: column nowrap;
    margin: 1rem 0;
    border: 1px solid #c6cad1;

    & > :first-child > :not(:last-child) {
        font-weight: bold;
        padding-top: 0.6em;
        padding-bottom: 0.6em;
    }

    & > :not(:first-child) > * {
        min-height: 6em;
    }

    & > :last-child > :last-child > * {
        border-bottom: 1px solid #c6cad1;
    }
}

.row {
    display: flex;
    flex-flow: row nowrap;
    text-align: center;

    border-top: 1px solid #c6cad1;
    border-left: 1px solid #c6cad1;
    border-right: 1px solid #c6cad1;

    &.highlighted > * {
        padding-top: 3px;
        padding-bottom: 3px;
    }

    &.highlighted > :first-child {
        padding-left: 3px;
    }

    &.highlighted > :last-child {
        padding-right: 3px;
    }
}
</style>
