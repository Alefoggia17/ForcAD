<template>
    <div class="table">
        <div class="row">
            <div class="number">{{ headRowTitle }}</div>
            <div class="team">team</div>
            <div class="score">score</div>
            <div class="service-name">
                <div
                    v-for="{ name, id } in tasks"
                    :key="name"
                    :style="taskStyle"
                    class="service-cell"
                    @click="$emit('openTask', id)"
                >
                    {{ name }}
                    <button
                        v-if="admin"
                        class="edit"
                        @click.stop="$emit('openTaskAdmin', id)"
                    >
                        <i class="fas fa-edit" />
                    </button>
                </div>
            </div>
        </div>
        <transition-group name="teams-list">
            <div
                v-for="(
                    {
                        name,
                        score: totalScore,
                        tasks: teamTasks,
                        ip,
                        id,
                        highlighted,
                    },
                    index
                ) in teams"
                :key="name"
                class="row"
                :class="[highlighted ? 'highlighted' : '', `rank-${index + 1}`]"
            >
                <div class="team-group" :class="highlighted ? '' : 'pd-3'">
                    <div class="number">
                        {{ index + 1 }}
                    </div>
                    <div
                        class="team team-row"
                        :style="teamStyle"
                        @click="teamClickable && $emit('openTeam', id)"
                    >
                        <div class="team-name">
                            {{ name }}
                        </div>
                        <div class="ip">
                            {{ ip }}
                        </div>
                        <button
                            v-if="admin"
                            class="edit"
                            @click.stop="$emit('openTeamAdmin', id)"
                        >
                            <i class="fas fa-edit" />
                        </button>
                    </div>
                    <div class="score">
                        {{ totalScore.toFixed(2) }}
                    </div>
                </div>
                <div class="service">
                    <div
                        v-for="{
                            id: teamTaskID,
                            teamId,
                            taskId,
                            sla,
                            score,
                            stolen,
                            lost,
                            message,
                            status,
                        } in teamTasks"
                        :key="teamTaskID"
                        class="service-cell"
                        :class="status ? 'status-up' : 'status-down'"
                    >
                        <button
                            v-if="admin"
                            class="tt-edit"
                            @click="$emit('openTeamTaskHistory', teamId, taskId)"
                        >
                            <i class="fas fa-edit" />
                        </button>
                        <button class="info">
                            <i class="fas fa-info-circle" />
                            <span class="tooltip">{{ message }}</span>
                        </button>
                        <div class="sla">
                            <strong>SLA</strong>: {{ sla.toFixed(2) }}%
                        </div>
                        <div class="fp">
                            <strong>FP</strong>: {{ score.toFixed(2) }}
                        </div>
                        <div class="flags">
                            <i class="fas fa-flag" />
                            +{{ stolen }}/-{{ lost }}
                        </div>
                    </div>
                </div>
            </div>
        </transition-group>
    </div>
</template>

<script>
export default {
    props: {
        headRowTitle: {
            type: String,
            default: '#',
        },
        tasks: {
            type: Array,
            required: true,
        },
        teams: {
            type: Array,
            required: true,
        },
        teamClickable: Boolean,
        taskClickable: Boolean,
        admin: Boolean,
    },

    computed: {
        teamStyle() {
            return this.teamClickable ? { cursor: 'pointer' } : {};
        },
        taskStyle() {
            return this.taskClickable ? { cursor: 'pointer' } : {};
        },
    },
};
</script>

<style lang="scss" scoped>
$napoli-blue: #13214F;
$napoli-light-blue: #00A1E0;
$napoli-gold: #FFD700;
$napoli-silver: #C0C0C0;
$napoli-bronze: #CD7F32;

.table {
    width: 100%;
    background: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin: 1rem;

    .row:first-child {
        background: $napoli-blue;
        color: white;
        font-weight: bold;
        min-height: 4em;

        .number, .team, .score, .service-name {
            padding: 12px;
        }
    }
}

.row {
    display: flex;
    flex-flow: row nowrap;
    min-height: 3em;
    border-bottom: 1px solid #eee;
}

.rank-1 {
    background-color: rgba($napoli-gold, 0.2);
}

.rank-2 {
    background-color: rgba($napoli-silver, 0.2);
}

.rank-3 {
    background-color: rgba($napoli-bronze, 0.2);
}

.pd-3 {
    margin-left: 2px;
}

.team-group {
    flex: 7 1 20%;
    display: flex;
    flex-flow: row nowrap;
}

.teams-list-move {
    transition: transform 1s;
}

.number {
    flex: 1 1 0;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    padding: 0 1rem;
}

.team {
    flex: 4 1 15%;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    position: relative;
    padding: 0.5rem 1rem;
}

.team-name {
    font-weight: bold;
    color: $napoli-blue;
}

.ip {
    font-size: 0.9em;
    color: #666;
}

.score {
    flex: 2 1 5%;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    padding: 0 1rem;
    font-weight: bold;
    color: $napoli-blue;
}

.service {
    flex: 20 2 0;
    display: flex;
    flex-flow: row nowrap;
    border-left: 1px solid #eee;

    & > :not(:last-child) {
        border-right: 1px solid #eee;
    }
}

.service-name {
    flex: 20 2 0;
    display: flex;
    flex-flow: row nowrap;
    text-align: center;
}

.service-cell {
    flex: 1 1 0;
    position: relative;
    display: flex;
    flex-flow: column nowrap;
    justify-content: space-around;
    padding: 1rem;
    min-width: 80px;
}

.sla, .fp, .flags {
    text-align: left;
    margin-left: 0.5em;
}

button {
    &.info, &.edit, &.tt-edit {
        background: $napoli-light-blue;
        color: white;
        padding: 0;
        position: absolute;
        top: 0.5em;
        width: 2.5em;
        height: 2.5em;
        border-radius: 0.3em;
        font-size: 0.7em;
        border: none;
        transition: background 0.2s;

        &:hover {
            background: lighten($napoli-light-blue, 10%);
        }
    }

    &.info {
        right: 0.5em;
    }

    &.edit {
        right: 0.5em;
    }

    &.tt-edit {
        right: 3em;
    }
}

.tooltip {
    font-size: 0.7rem;
    position: absolute;
    width: 20em;
    text-align: center;
    padding: 1em;
    border-radius: 0.5em;
    left: 50%;
    transform: translateX(-50%) translateY(-100%);
    top: -0.5em;
    opacity: 0;
    z-index: -1;
    transition: opacity 0.2s;
    background: $napoli-blue;
    border: 1px solid $napoli-light-blue;
    color: white;
}

.info:hover .tooltip {
    opacity: 1;
    z-index: 1;
}

.status-up {
    background-color: rgba(0, 255, 0, 0.1);
}

.status-down {
    background-color: rgba(255, 0, 0, 0.1);
}

.highlighted {
    background: linear-gradient(
        to right,
        rgba($napoli-light-blue, 0.3) 0%,
        rgba($napoli-blue, 0.3) 50%,
        rgba($napoli-light-blue, 0.3) 100%
    );
}
</style>