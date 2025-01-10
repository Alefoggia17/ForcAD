<template>
    <div class="table">
        <div class="row">
            <div class="number">
                {{ headRowTitle }}
            </div>
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
                        @click="$emit('openTaskAdmin', id)"
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
                :class="[highlighted ? 'highlighted' : '']"
                :style="{
                    backgroundColor: getTeamRowBackground(index),
                }"
            >
                <div class="team-group" :class="highlighted ? '' : 'pd-3'">
                    <div
                        class="number"
                        :style="{
                            backgroundColor: getTeamRowBackground(index),
                        }"
                    >
                        {{ index + 1 }}
                    </div>
                    <div
                        class="team team-row"
                        :style="[
                            teamStyle,
                            { backgroundColor: getTeamRowBackground(index) },
                        ]"
                        @click="$emit('openTeam', id)"
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
                            @click="$emit('openTeamAdmin', id)"
                            @click.stop
                        >
                            <i class="fas fa-edit" />
                        </button>
                    </div>
                    <div
                        class="score"
                        :style="{
                            backgroundColor: getTeamRowBackground(index),
                        }"
                    >
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
                        :style="{
                            fontSize: `${1 - teamTasks.length / 20}em`,
                            backgroundColor: getTeamTaskBackground(status),
                        }"
                    >
                        <button
                            v-if="admin"
                            class="tt-edit"
                            @click="
                                $emit('openTeamTaskHistory', teamId, taskId)
                            "
                        >
                            <i class="fas fa-edit" />
                        </button>
                        <button class="info">
                            <i class="fas fa-info-circle" />
                            <span class="tooltip">{{ message }}</span>
                        </button>
                        <div class="sla">
                            <strong>SLA</strong>
                            : {{ sla.toFixed(2) }}%
                        </div>
                        <div class="fp">
                            <strong>FP</strong>
                            : {{ score.toFixed(2) }}
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
import { getTeamRowBackground, getTeamTaskBackground } from '@/utils/colors';
export default {
    props: {
        headRowTitle: {
            type: String,
            default: '#'
        },
        tasks: {
            type: Array,
            required: true
        },
        teams: {
            type: Array,
            required: true
        },
        teamClickable: Boolean,
        taskClickable: Boolean,
        admin: Boolean
    },

    computed: {
        teamStyle() {
            return this.teamClickable ? { cursor: 'pointer' } : {};
        },
        taskStyle() {
            return this.taskClickable ? { cursor: 'pointer' } : {};
        }
    }
};
</script>

<style lang="scss" scoped>
// SSC Napoli Colors
$napoli-blue: #13214F;
$napoli-light-blue: #00A1E0;
$napoli-lighter-blue: #4DB8FF;
$napoli-lightest-blue: #99D6FF;

.table {
    display: flex;
    flex-flow: column nowrap;
    width: 100%;
    background: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin: 1rem;

    & > :first-child {
        background: $napoli-blue;
        color: white;
        
        & > :not(:last-child) {
            font-weight: bold;
            padding-top: 0.6em;
            padding-bottom: 0.6em;
        }
    }

    & > :not(:first-child) > * {
        min-height: 6em;
    }

    & > :last-child > :last-child > * {
        border-bottom: 1px solid #eee;
    }
}

.row {
    display: flex;
    flex-flow: row nowrap;
    text-align: center;
    min-height: 3em;
    border-bottom: 1px solid #eee;

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

.pd-3 {
    margin-left: 2px;
}

.team-group {
    flex: 7 1 20%;
    display: flex;
    flex-flow: row nowrap;

    &:nth-child(1) {
        background-color: rgba($napoli-light-blue, 0.2);
    }

    &:nth-child(2) {
        background-color: rgba($napoli-lighter-blue, 0.15);
    }

    &:nth-child(3) {
        background-color: rgba($napoli-lightest-blue, 0.1);
    }
}

.teams-list-move {
    transition: transform 1s;
}

.number {
    flex: 1 1 0;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
}

.team {
    flex: 4 1 15%;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    position: relative;
}

.team-name {
    font-weight: bold;
    color: $napoli-blue;
    
    &::before {
        content: "⚽";
        margin-right: 8px;
        color: $napoli-light-blue;
    }
}

.score {
    flex: 2 1 5%;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
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
    padding: 8px;
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
        transition: all 0.2s;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);

        &:hover {
            background: lighten($napoli-light-blue, 10%);
            transform: translateY(-1px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
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
    left: 0;
    top: 0;
    transform: translateX(calc(-100%)) translateY(calc(-100% - 0.25em));
    position: absolute;
    width: 20em;
    text-align: center;
    display: block;
    background-color: black;
    color: white;
    border-radius: 0.5em;
    padding: 1em;
    opacity: 0;
    z-index: -1;
}

.info:hover .tooltip {
    opacity: 1;
    z-index: 1;
}

.highlighted {
    transform: translateZ(0);
    animation: rotate 5s infinite linear;
    background: linear-gradient(
        to right,
        rgba($napoli-light-blue, 0.3) 0%,
        rgba($napoli-blue, 0.3) 50%,
        rgba($napoli-light-blue, 0.3) 100%
    );
}

@keyframes rotate {
    from {
        background-position: -3000px;
    }
    to {
        background-position: 0px;
    }
}
</style>