<template>
    <div class="table">
        <div class="row">
            <div class="number">{{ headRowTitle }}</div>
            <div class="team">Team</div>
            <div class="score">Score</div>
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
                v-for="({
                    name,
                    score: totalScore,
                    tasks: teamTasks,
                    ip,
                    id,
                    highlighted
                }, index) in teams"
                :key="name"
                class="row"
                :class="[highlighted ? 'highlighted' : '']"
            >
                <div class="team-group" :class="highlighted ? '' : 'pd-3'">
                    <div class="number">{{ index + 1 }}</div>
                    <div
                        class="team team-row"
                        :style="teamStyle"
                        @click="teamClickable && $emit('openTeam', id)"
                    >
                        <div class="team-name">
                            {{ name }}
                        </div>
                        <div class="ip">{{ ip }}</div>
                        <button
                            v-if="admin"
                            class="edit"
                            @click.stop="$emit('openTeamAdmin', id)"
                        >
                            <i class="fas fa-edit" />
                        </button>
                    </div>
                    <div class="score">{{ totalScore.toFixed(2) }}</div>
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
                            status
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
import { getTeamRowBackground, getTeamTaskBackground } from '@/utils/colors';
import '@/assets/table.scss';

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

    data: function () {
        return {
            getTeamRowBackground,
            getTeamTaskBackground,
        };
    },

    computed: {
        teamStyle: function () {
            return this.teamClickable
                ? {
                      cursor: 'pointer',
                  }
                : {};
        },

        taskStyle: function () {
            return this.taskClickable
                ? {
                      cursor: 'pointer',
                  }
                : {};
        },
    },
};
</script>

<style lang="scss" scoped>