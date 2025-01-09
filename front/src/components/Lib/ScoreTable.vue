<template>
    <div class="score-table">
        <div class="table-header">
            <h1 class="napoli-title">Napoli Fan Board</h1>
            <div class="action-buttons">
                <button class="napoli-btn">Create team</button>
                <button class="napoli-btn">Create task</button>
            </div>
        </div>
        <table>
            <thead>
                <tr>
                    <th>{{ headRowTitle }}</th>
                    <th>Team</th>
                    <th>Score</th>
                    <th v-for="task in tasks" :key="task.id">{{ task.name }}</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(team, index) in teams" :key="team.id" :class="getRowClass(index)">
                    <td>{{ index + 1 }}</td>
                    <td 
                        class="team-name"
                        :class="{ 'clickable': teamClickable }"
                        @click="teamClickable && $emit('openTeam', team.id)"
                    >
                        {{ team.name }}
                        <div class="team-ip">{{ team.ip }}</div>
                    </td>
                    <td class="score">{{ team.score.toFixed(2) }}</td>
                    <td v-for="task in tasks" :key="task.id" class="task-cell">
                        <button class="status-btn">⚽</button>
                    </td>
                    <td class="actions">
                        <button class="action-btn">📊</button>
                        <button class="action-btn">⚙️</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    props: {
        headRowTitle: {
            type: String,
            default: '#'
        },
        teams: {
            type: Array,
            required: true
        },
        tasks: {
            type: Array,
            required: true
        },
        teamClickable: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        getRowClass(index) {
            const colors = ['napoli-gold', 'napoli-silver', 'napoli-bronze', 'white-row', 'white-row'];
            return colors[index] || 'white-row';
        }
    }
};
</script>

<style lang="scss" scoped>
// SSC Napoli Colors
$napoli-blue: #13214F;
$napoli-light-blue: #00A1E0;
$napoli-gold: #FFD700;
$napoli-silver: #C0C0C0;
$napoli-bronze: #CD7F32;

.score-table {
    width: 100%;
    background: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

    .table-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1rem;
        background: $napoli-blue;
    }

    .napoli-title {
        color: white;
        font-size: 1.5rem;
        font-weight: bold;
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
        
        &:hover {
            background: lighten($napoli-light-blue, 10%);
        }
    }

    table {
        width: 100%;
        border-collapse: collapse;
    }

    th {
        background: $napoli-blue;
        color: white;
        padding: 12px;
        text-align: left;
    }

    td {
        padding: 12px;
        border-bottom: 1px solid #eee;
    }

    .team-name {
        font-weight: bold;
        display: flex;
        flex-direction: column;
        gap: 0.25rem;

        &.clickable {
            cursor: pointer;
            &:hover {
                color: $napoli-light-blue;
            }
        }
    }

    .team-ip {
        font-size: 0.9em;
        color: #666;
    }

    .score {
        font-weight: bold;
        color: $napoli-blue;
    }

    // Row colors using Napoli theme
    .napoli-gold {
        background-color: rgba($napoli-gold, 0.2);
    }

    .napoli-silver {
        background-color: rgba($napoli-silver, 0.2);
    }

    .napoli-bronze {
        background-color: rgba($napoli-bronze, 0.2);
    }

    .white-row {
        background-color: white;
    }

    .task-cell {
        text-align: center;
        min-width: 80px;
    }

    .status-btn, .action-btn {
        background: none;
        border: none;
        cursor: pointer;
        padding: 4px;
        border-radius: 4px;
        
        &:hover {
            background: rgba($napoli-light-blue, 0.1);
        }
    }

    .actions {
        display: flex;
        gap: 0.5rem;
        justify-content: center;
    }
}
</style>