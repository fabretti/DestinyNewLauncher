<template>
  <div class="progress">
    <div class="status-text">Клиент обновляется, пожалуйста подождите</div>

    <div class="progress-wrapper">
      <div class="progress-container">
        <div class="side-square left-square"></div>

        <div class="progress-bar">
          <div class="progress-fill" :style="{ width: `${progress}%` }">
            <span class="progress-text">{{ progress }}%</span>
          </div>
        </div>

        <div class="side-square right-square"></div>
      </div>

      <div class="control-buttons">
        <button class="control-btn" @click="handlePause" :disabled="!isActive">
          <IconBase name="pause" size="34" />
        </button>
        <button class="control-btn" @click="handleSettings">
          <IconBase name="settings" size="34" />
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import IconBase from '../shared/IconBase.vue'

interface Props {
  progress: number
  status?: string
  isActive?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  progress: 0,
  status: 'Клиент обновляется, пожалуйста подождите',
  isActive: true,
})

const emit = defineEmits<{
  pause: []
  settings: []
}>()

const statusText = computed(() => props.status)

const handlePause = () => {
  emit('pause')
}

const handleSettings = () => {
  emit('settings')
}
</script>

<style lang="scss" scoped>
.progress {
  display: flex;
  flex-direction: column;
  gap: 4px;

  .status-text {
    font-size: 14px;
    line-height: 38px;
    font-weight: 300;
  }
  .progress-wrapper {
    display: flex;
    .progress-container {
      position: relative;
      display: flex;
      align-items: center;
      gap: 0;
      width: 690px;

      .side-square {
        position: absolute;
        width: 15px;
        height: 15px;
        background: rgba(92, 147, 230, 1);

        &.left-square {
          transform: rotate(45deg);
          left: -7px;
        }

        &.right-square {
          transform: rotate(45deg);
          right: -5px;
        }
      }

      .progress-bar {
        flex: 1;
        overflow: hidden;
        position: relative;
        padding: 8px 15px;
        border: 1px solid rgba(92, 147, 230, 1);

        .progress-fill {
          height: 36px;
          background:
            linear-gradient(270deg, #6b9ee1 0%, #6592e5 103.01%),
            linear-gradient(270deg, #6b9ee1 0%, #6592e5 103.01%);

          box-shadow:
            0px 4px 44px 0px rgba(103, 201, 247, 0.4),
            3px 3px 30px 0px rgba(0, 102, 255, 0.6) inset;

          transition: width 0.3s ease;
          position: relative;
          display: flex;
          align-items: center;
        }

        .progress-text {
          position: absolute;
          left: 8px;
          font-size: 12px;
          font-weight: 400;
        }

        .control-buttons {
          display: flex;
          gap: 12px;
        }
      }
    }
    .control-buttons {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-left: 16px;
    }
    .control-btn {
      width: 48px;
      height: 48px;
      border: 1px solid rgba(31, 50, 72, 1);
      border-radius: 10px;
      background: transparent;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
      backdrop-filter: blur(10px);

      &:hover {
        box-shadow: 0 0 15px rgba(0, 212, 255, 0.5);
        transform: translateY(-1px);
      }

      &:active {
        transform: translateY(0);
      }

      &:disabled {
        opacity: 0.5;
        cursor: not-allowed;

        &:hover {
          box-shadow: 0 0 10px rgba(0, 212, 255, 0.3);
          transform: none;
        }
      }

      :deep(svg) {
        color: #00d4ff;
      }
    }
  }
}
</style>
