<template>
  <div class="main-container">
    <div class="home">
      <div class="home-left">
        <div class="logo">
          <img src="../assets/img/Logo.svg" alt="logo" class="logo" />
        </div>
        <div class="home-left__func">
          <div class="menu-item options">
            <span class="menu-text">Опции</span>
            <div class="dropdown-arrows">
              <div class="arrow up"></div>
              <div class="arrow down"></div>
            </div>
          </div>
          <div class="menu-item">
            <IconBase name="key" size="26" />
            <span class="menu-text">Использовать код</span>
          </div>
          <div class="menu-item">
            <IconBase name="access" size="26" />
            <span class="menu-text">Ранний доступ</span>
          </div>
        </div>
        <div class="home-left__swiper">
          <div class="video-block">
            <div class="video-container">
              <div class="corner-bottom-left"></div>
              <div class="corner-bottom-right"></div>
              <div class="video-thumbnail">
                <div class="play-button">
                  <div class="play-triangle"></div>
                </div>
              </div>
            </div>
            <div class="video-controls">
              <span class="video-title">Видео</span>
              <div class="progress-indicator">
                <div
                  class="progress-step"
                  :class="{ active: currentStep === 1 }"
                  @click="switchToStep(1)"
                ></div>
                <div
                  class="progress-step"
                  :class="{ active: currentStep === 2 }"
                  @click="switchToStep(2)"
                ></div>
                <div
                  class="progress-step"
                  :class="{ active: currentStep === 3 }"
                  @click="switchToStep(3)"
                ></div>
                <div
                  class="progress-step"
                  :class="{ active: currentStep === 4 }"
                  @click="switchToStep(4)"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="home-right">
        <div class="home-right__season">
          <div class="img">
            <img src="../assets/img/authLoginGuys.png" alt="girl" class="girl" />
            <div class="text text-body-18">Новый сезон</div>
          </div>
          <div class="subtitle text-body-12">Новый ивент уже доступен</div>
        </div>
        <div class="home-right__vip">
          <div class="img">
            <img src="../assets/img/vip.png" alt="vip" class="vip" />
            <div class="text text-body-18">
              VIP на 30<br />
              дней
            </div>
          </div>
          <ButtonItem @click="buyVip">КУПИТЬ</ButtonItem>
        </div>
      </div>
    </div>

    <div class="home-download">
      <FileUploadProgress
        :progress="uploadProgress"
        :isActive="true"
        @pause="handlePause"
        @settings="handleSettings"
      />
      <div class="home-download__btn">ИГРАТЬ</div>
    </div>
  </div>
</template>
<script setup lang="ts">
import IconBase from '../shared/IconBase.vue'
import { ref } from 'vue'
import ButtonItem from '../shared/ButtonItem.vue'
import FileUploadProgress from '../components/FileUploadProgress.vue'

const currentStep = ref(4)
const uploadProgress = ref(10)

const switchToStep = (step: number) => {
  currentStep.value = step
}

const handlePause = () => {
  console.log('Загрузка приостановлена')
}

const handleSettings = () => {
  console.log('Открыть настройки')
}

const buyVip = () => {
  console.log('buyVip')
}
</script>
<style lang="scss">
.home {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 44px;
  margin: 25px 0 40px;
  .home-left {
    display: flex;
    flex-direction: column;
    .logo {
      display: flex;
    }
    .home-left__func {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-top: 20px;
      .menu-item {
        display: flex;
        align-items: center;
        cursor: pointer;
        &.options {
          width: 100px;
          height: 32px;
          padding: 6px 12px;
          border: 0.5px solid rgba(255, 255, 255, 0.3);
        }

        .menu-text {
          font-size: 12px;
          font-weight: 500;
          white-space: nowrap;
        }

        .dropdown-arrows {
          display: flex;
          flex-direction: column;
          gap: 2px;
          margin-left: auto;

          .arrow {
            width: 0;
            height: 0;
            border-left: 4px solid transparent;
            border-right: 4px solid transparent;

            &.up {
              border-bottom: 4px solid rgba(255, 255, 255, 0.6);
            }

            &.down {
              border-top: 4px solid rgba(255, 255, 255, 0.6);
            }
          }
        }

        .iconBase {
          svg {
            filter: drop-shadow(0 0 8px rgba(59, 130, 246, 0.5));
          }
        }
      }
    }

    .home-left__swiper {
      margin-top: 20px;
      display: flex;
      .video-block {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 16px;

        .video-container {
          position: relative;
          width: 484px;
          border: 0.5px solid rgba(105, 113, 123, 1);
          padding: 10px 16px;

          &::before,
          &::after {
            content: '';
            position: absolute;
            width: 8px;
            height: 8px;
            background: rgba(105, 113, 123, 1);
          }

          &::before {
            top: -4px;
            left: -4px;
          }

          &::after {
            top: -4px;
            right: -4px;
          }

          .corner-bottom-left,
          .corner-bottom-right {
            position: absolute;
            width: 8px;
            height: 8px;
            background: rgba(105, 113, 123, 1);
          }

          .corner-bottom-left {
            bottom: -4px;
            left: -4px;
          }

          .corner-bottom-right {
            bottom: -4px;
            right: -4px;
          }

          .video-thumbnail {
            width: 100%;
            height: 220px;
            background: url('../assets/img/video.png') no-repeat center center;
            position: relative;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;

            &:hover {
              transform: scale(1.02);
              box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
            }

            .play-button {
              width: 60px;
              height: 60px;
              border: 2px solid rgba(255, 255, 255, 0.8);
              border-radius: 50%;
              display: flex;
              align-items: center;
              justify-content: center;
              background: rgba(255, 255, 255, 0.1);
              backdrop-filter: blur(10px);
              cursor: pointer;
              transition: all 0.3s ease;

              &:hover {
                background: rgba(255, 255, 255, 0.2);
                transform: scale(1.1);
              }

              .play-triangle {
                width: 0;
                height: 0;
                border-left: 16px solid rgba(255, 255, 255, 0.9);
                border-top: 12px solid transparent;
                border-bottom: 12px solid transparent;
                margin-left: 4px;
              }
            }
          }
        }

        .video-controls {
          display: flex;
          flex-direction: column;
          align-items: center;
          gap: 16px;

          .video-title {
            font-weight: 400;
            font-size: 25px;
            line-height: 100%;
            letter-spacing: 1px;
          }

          .progress-indicator {
            display: flex;
            gap: 33px;
            align-items: center;

            .progress-step {
              position: relative;
              width: 12px;
              height: 12px;
              border: 2px solid rgba(255, 255, 255, 0.6);
              border-radius: 50%;
              transition: all 0.3s ease;
              cursor: pointer;
              &:not(:last-child) {
                &:before {
                  content: '';
                  position: absolute;
                  top: 50%;
                  width: 34px;
                  left: 10px;
                  right: 6px;
                  height: 2px;
                  background: rgba(255, 255, 255, 0.3);
                  transform: translateY(-50%);
                  z-index: 1;
                }
              }
              &:hover {
                border-color: rgba(255, 255, 255, 0.9);
              }

              &.active {
                background: rgba(114, 201, 243, 1);
                border-color: rgba(114, 201, 243, 1);
                box-shadow: 0 0 10px rgba(0, 191, 255, 0.5);
              }
            }
          }

          .navigation-buttons {
            display: flex;
            gap: 12px;
            align-items: center;

            .nav-btn {
              width: 32px;
              height: 32px;
              border: 1px solid rgba(255, 255, 255, 0.4);
              border-radius: 50%;
              background: rgba(255, 255, 255, 0.1);
              color: white;
              font-size: 18px;
              cursor: pointer;
              transition: all 0.3s ease;
              display: flex;
              align-items: center;
              justify-content: center;

              &:hover {
                background: rgba(255, 255, 255, 0.2);
                border-color: rgba(255, 255, 255, 0.8);
                transform: scale(1.1);
              }

              &.prev {
                font-size: 20px;
                font-weight: bold;
              }

              &.next {
                font-size: 20px;
                font-weight: bold;
              }
            }
          }
        }
      }
    }
  }
  .home-right {
    display: grid;
    align-items: flex-end;
    gap: 44px;
    grid-template-columns: 1fr 1fr;
    .home-right__season,
    .home-right__vip {
      width: max-content;
      display: flex;
      flex-direction: column;
      .img {
        width: 215px;
        display: flex;
        position: relative;
        img {
          width: 100%;
        }
        .text {
          text-align: center;
          position: absolute;
          bottom: 20px;
          left: 50%;
          transform: translateX(-50%);
          white-space: nowrap;
        }
      }
      .subtitle {
        padding-top: 10px;
        font-weight: 500;
        text-align: center;
        border-top: 0.5px solid rgba(255, 255, 255, 0.2);
      }
      .ButtonItem {
        margin-top: 14px;
      }
    }
    .home-right__vip {
      margin-bottom: 100px;
    }
  }
}
.home-download {
  display: grid;
  grid-template-columns: 1fr 290px;
  align-items: flex-start;
  gap: 40px;
  .progress {
    width: 100%;
  }
  .home-download__btn {
    height: 123px;
    font-weight: 400;
    font-size: 25px;
    line-height: 100%;
    letter-spacing: 2px;
    text-align: center;
    vertical-align: middle;
    color: rgba(10, 24, 39, 1);
    cursor: pointer;

    display: flex;
    align-items: center;
    justify-content: center;
    background: url('../assets/img/button.png') no-repeat center center;
  }
}
</style>
