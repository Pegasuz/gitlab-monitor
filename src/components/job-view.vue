<template>
  <a class="job-view" target="_blank" rel="noopener noreferrer" :href="project.web_url + '/-/jobs/' + job.id">
    <div :class="['job-circle', job.status, {square: !showJobNames}]">
      <transition name="fade" mode="out-in">
        <div v-if="showJobNames" :key="job.status">
          {{ job.name }}
        </div>
        <svg v-else :key="statusIconName">
          <use
            v-bind="{
            'href': require('../assets/icons.svg') + '#' + statusIconName,
            'xlink:href': require('../assets/icons.svg') + '#' + statusIconName
          }">
          </use>
        </svg>
      </transition>
    </div>
    <div class="pipe"></div>
  </a>
</template>

<script>
  import {getQueryParameter} from '../util';

  export default {
    name: 'job-view',
    props: ['job', 'project'],
    computed: {
      statusIconName() {
        switch (this.$props.job.status) {
          case 'canceled':
            return 'status_canceled_borderless';
          case 'failed':
            return 'status_failed_borderless';
          case 'pending':
            return 'status_pending_borderless';
          case 'running':
            return 'status_running_borderless';
          case 'skipped':
            return 'status_skipped_borderless';
          case 'success':
            return 'status_success_borderless';
          default:
            return 'status_not_found_borderless';
        }
      },
      showJobNames() {
        return !!getQueryParameter('showJobNames')
      }
    }
  };
</script>

<style lang="scss" scoped>
  .job-view {
    display: inline-flex;
    align-items: center;

    &:last-child {
      .pipe {
        display: none;
      }
    }

    .job-circle {
      width: auto;
      display: inline-block;
      height: 24px;
      border: 2px solid rgba(255, 255, 255, 0.8);
      border-radius: 24px;
      line-height: 24px;
      padding: 0 6px;
      font-size: 12px;
      transition: background-color 200ms;

      &.square {
        width: 24px;
        padding: 0;
      }

      &.success {
        background-color: #2E7D32;
      }

      &.running {
        background-color: #1565C0;
      }

      &.pending {
        background-color: #EF6C00;
      }

      &.failed {
        background-color: #C62828;
      }

      &.canceled {
        background-color: #010101;
      }

      &.skipped {
        background-color: #4b4b4b;
      }

      svg {
        width: 100%;
        height: 100%;
        fill: rgba(255, 255, 255, 0.8);
      }
    }

    .pipe {
      height: 2px;
      background-color: rgba(255, 255, 255, 0.8);
      width: 6px;
    }
  }
</style>
