<template>
    <section class="menu">

        <div v-tooltip="'Home'"
             :class="{'item': 1, active: activeTab === 'home'}"
             @click="changeTab('home')">
            <i class="fas fa-fw fa-home"></i>
        </div>

        <div v-tooltip="'View marked folder and files'"
             :class="{'item': 1, active: activeTab === 'marked'}"
             @click="changeTab('marked')">
            <i class="fas fa-fw fa-bookmark"></i>
            <intro-box id="0"
                       header="Marked Folders and files"
                       text="Mark your important files, folder or just use it as a quick way to access them."/>
        </div>

        <div v-tooltip="'Removed files / folders'"
             :class="{'item': 1, active: activeTab === 'bin'}"
             @click="changeTab('bin')">
            <i class="fas fa-fw fa-trash-alt"></i>
        </div>

        <div class="eat-space"></div>

        <div v-tooltip="'Refresh'"
             class="item bottom"
             @click="refresh">
            <i class="fas fa-fw fa-sync-alt"></i>
        </div>

        <div v-tooltip="'User settings'"
             :class="{'item bottom': 1, active: activeTab === 'settings'}"
             @click="changeTab('settings')">
            <i class="fas fa-fw fa-cog"></i>
        </div>

        <div v-tooltip="'Logout'"
             class="item bottom"
             @click="$store.dispatch('auth/logout')">
            <i class="fas fa-fw fa-sign-out-alt"></i>
        </div>

    </section>
</template>

<script>

    // Components
    import IntroBox from '../../ui/specific/IntroBox';

    // Vue stuff
    import {mapState} from 'vuex';

    export default {

        components: {IntroBox},

        data() {
            return {};
        },

        computed: {
            ...mapState(['activeTab'])
        },

        methods: {

            changeTab(newTab) {

                // Clear selection
                this.$store.commit('selection/clear');

                // Show new tab
                this.$store.commit('setActiveTab', newTab);
            },

            refresh() {

                // Update nodes
                this.$store.dispatch('nodes/update', {keepLocation: true});

                // Go to home
                this.changeTab('home');
            }
        }
    };

</script>

<style lang="scss" scoped>

    .menu {
        background: white;
        box-shadow: 0 0 3px 0 rgba($palette-deep-blue, 0.05);
        border-right: 1px solid rgba($palette-deep-blue, 0.05);
        @include flex(column, center);
        padding: 1em 0.75em;
        height: 100%;
        color: $palette-decent-blue;
    }

    .item {
        @include flex(column, center, center);
        @include size(2.5em);
        position: relative;
        margin-bottom: 1em;
        cursor: pointer;
        transition: all 0.3s;

        i {
            transition: all 0.3s;
            font-size: 1em;
        }

        &.active i {
            color: $palette-theme-primary;
        }

        &:not(.active):hover i {
            color: $palette-deep-blue;
        }

        &.bottom {
            margin: 1em 0 0;
        }
    }

    .eat-space {
        flex-grow: 1;
    }

</style>
