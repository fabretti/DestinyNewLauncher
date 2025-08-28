<template>
  <div class="auth-container">
    <div class="auth-left">
      <img src="../assets/img/Logo.svg" alt="logo" class="logo" />
      <div class="auth-tabs">
        <span
          v-for="(tabOption, index) in tabs"
          :key="tabOption.value"
          :class="['auth-tab', tab === tabOption.value ? 'active' : '']"
          :ref="
            (el) => {
              if (el) tabRefs[index] = el as HTMLElement
            }
          "
          @click="changeTab(tabOption.value)"
        >
          {{ tabOption.label }}
        </span>
        <div class="auth-tab-indicator" :style="indicatorStyle"></div>
      </div>
      <div class="auth-form-block" :class="tab === 'login' ? 'auth-form-block-login' : 'auth-form-block-register'">
        <h2 class="auth-title text-body-32">{{ currentTab.title }}</h2>
        <form class="auth-form" @submit.prevent="handleSubmit">
          <AuthInput v-model="form.login" type="text" placeholder="Логин" :error="errors.login" />
          <AuthInput
            v-if="tab === 'register'"
            v-model="form.email"
            type="email"
            placeholder="Email"
            :error="errors.email"
          />
          <AuthInput
            v-model="form.password"
            type="password"
            placeholder="Пароль"
            :show-password="tab === 'login' ? showLoginPassword : showRegisterPassword"
            :error="errors.password"
            @toggle-password="
              tab === 'login'
                ? (showLoginPassword = !showLoginPassword)
                : (showRegisterPassword = !showRegisterPassword)
            "
          />
          <AuthInput
            v-if="tab === 'register'"
            v-model="form.repeatPassword"
            type="password"
            placeholder="Повторите пароль"
            :show-password="showRegisterRepeat"
            :error="errors.repeatPassword"
            @toggle-password="showRegisterRepeat = !showRegisterRepeat"
          />
          <ButtonItem type="submit" class="auth-btn">
            {{ currentTab.buttonText }}
          </ButtonItem>
        </form>
      </div>
    </div>
    <div class="auth-right">
      <div class="auth-right-img">
        <img src="../assets/img/authLoginGuys.png" alt="girl" class="auth-girl" />
        <div class="text text-body-32">Новый сезон</div>
      </div>
      <div class="subtitle text-body-18">
        Новый ивент уже<br />
        доступен
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import AuthInput from '@/components/AuthInput.vue'
import ButtonItem from '@/shared/ButtonItem.vue'
import { ref, computed, watch, nextTick, onMounted } from 'vue'

const tabs = [
  { value: 'login', label: 'Войти', title: 'Войти в аккаунт', buttonText: 'ВОЙТИ' },
  {
    value: 'register',
    label: 'Регистрация',
    title: 'Регистрация',
    buttonText: 'ЗАРЕГИСТРИРОВАТЬСЯ',
  },
] as const

const tab = ref<'login' | 'register'>('login')
const tabRefs = ref<HTMLElement[]>([])
const showLoginPassword = ref(false)
const showRegisterPassword = ref(false)
const showRegisterRepeat = ref(false)
const form = ref({
  login: '',
  password: '',
  email: '',
  repeatPassword: '',
})

const errors = ref({
  login: '',
  password: '',
  email: '',
  repeatPassword: '',
})

watch(tab, () => {
  clearErrors()
  nextTick(() => {
    updateIndicatorPosition()
  })
})

const changeTab = (value: 'login' | 'register') => {
  tab.value = value
  clearErrors()
  clearForm()
}

const updateIndicatorPosition = () => {
  // Принудительно обновляем computed свойство
  nextTick(() => {
    // Дополнительная задержка для корректного расчета позиции
    setTimeout(() => {
      // Обновляем позицию индикатора
    }, 50)
  })
}

onMounted(() => {
  nextTick(() => {
    updateIndicatorPosition()
  })
})

const currentTab = computed(() => tabs.find((t) => t.value === tab.value)!)

const indicatorStyle = computed(() => {
  const activeIndex = tabs.findIndex((t) => t.value === tab.value)
  if (activeIndex === -1 || !tabRefs.value[activeIndex]) {
    return {
      left: '0px',
      width: '0px',
    }
  }

  const activeTab = tabRefs.value[activeIndex]
  const container = activeTab.parentElement
  if (!container) return { left: '0px', width: '0px' }

  const containerRect = container.getBoundingClientRect()
  const tabRect = activeTab.getBoundingClientRect()

  return {
    left: `${tabRect.left - containerRect.left}px`,
    width: `${tabRect.width}px`,
  }
})

const validateLogin = (value: string): string => {
  if (!value.trim()) return 'Логин обязателен'
  if (value.length < 3) return 'Логин должен содержать минимум 3 символа'
  return ''
}

const validatePassword = (value: string): string => {
  if (!value) return 'Пароль обязателен'
  if (value.length < 6) return 'Пароль должен содержать минимум 6 символов'
  return ''
}

const validateEmail = (value: string): string => {
  if (!value) return 'Email обязателен'
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(value)) return 'Введите корректный email'
  return ''
}

const validateRepeatPassword = (value: string): string => {
  if (!value) return 'Повторите пароль'
  if (value !== form.value.password) return 'Пароли не совпадают'
  return ''
}

const clearErrors = () => {
  errors.value = {
    login: '',
    password: '',
    email: '',
    repeatPassword: '',
  }
}

const clearForm = () => {
  form.value = {
    login: '',
    password: '',
    email: '',
    repeatPassword: '',
  }
}

const validateForm = (): boolean => {
  clearErrors()
  let isValid = true

  // Валидация логина
  errors.value.login = validateLogin(form.value.login)
  if (errors.value.login) isValid = false

  // Валидация пароля
  errors.value.password = validatePassword(form.value.password)
  if (errors.value.password) isValid = false

  // Дополнительная валидация для регистрации
  if (tab.value === 'register') {
    errors.value.email = validateEmail(form.value.email)
    if (errors.value.email) isValid = false

    errors.value.repeatPassword = validateRepeatPassword(form.value.repeatPassword)
    if (errors.value.repeatPassword) isValid = false
  }

  return isValid
}

const handleSubmit = () => {
  if (!validateForm()) {
    console.log('Ошибки валидации:', errors.value)
    return
  }

  if (tab.value === 'login') {
    console.log('Login:', { login: form.value.login, password: form.value.password })
    // Здесь будет логика входа
  } else {
    console.log('Register:', form.value)
    // Здесь будет логика регистрации
  }
}
</script>

<style scoped lang="scss">
.auth-container {
  position: relative;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 400px 1fr;
  padding: 60px 30px 0 30px;
  gap: 200px;
  z-index: 2;

  .auth-left {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;

    .auth-tabs {
      width: 100%;
      display: flex;
      justify-content: center;
      gap: 24px;
      margin-top: 22px;
      font-size: 22px;
      font-weight: 400;
      letter-spacing: 0.1em;
      border-bottom: 1px solid rgba(255, 255, 255, 0.15);
      position: relative;
    }
    .auth-tab {
      color: var(--color-white);
      cursor: pointer;
      padding-bottom: 12px;
      border-bottom: 2px solid transparent;
      transition:
        color 0.2s,
        border-color 0.2s;

      &.active {
        color: var(--color-white);
        border-bottom: 2px solid transparent;
      }
    }

    .auth-tab-indicator {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 0;
      height: 2px;
      background-color: var(--color-white);
      transition: all 0.3s ease-in-out;
      border-radius: 1px;
    }

    .auth-form-block {
      width: 100%;
      background: none;
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 80px;
      &.auth-form-block-register {
        margin-top: 30px;
        .auth-title {
          margin-bottom: 30px;
        }
      }
      .auth-title {
        color: var(--color-white);
        margin-bottom: 60px;
        text-align: center;
      }
      .auth-form {
        width: 100%;
        display: flex;
        flex-direction: column;
        gap: 30px;

        .auth-btn {
          margin-top: 18px;
        }
      }
      .auth-forgot {
        display: block;
        margin: 26px auto 0 auto;
        color: var(--color-white);
        font-size: 16px;
        text-align: center;
        text-decoration: underline;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        cursor: pointer;
      }
    }
    @media (width < 900px) {
      .auth-container {
        flex-direction: column;
      }
      .auth-left,
      .auth-right {
        flex: none;
        width: 100%;
        min-height: 320px;
      }
      .auth-form-block {
        width: 98vw;
        max-width: 98vw;
      }
    }
  }
  .auth-right {
    width: max-content;
    display: flex;
    flex-direction: column;
    margin-top: 170px;
    .auth-right-img {
      display: flex;
      position: relative;
      .text {
        position: absolute;
        bottom: 30px;
        left: 50%;
        transform: translateX(-50%);
      }
    }
    .subtitle {
      margin-top: 10px;
      font-weight: 500;
      text-align: center;
    }
  }
  @media (width < 900px) {
    display: flex;
    flex-direction: column;
    .auth-left {
      height: 160px;
      .auth-guys {
        display: none;
      }
      .logo {
        margin: auto 0;
      }
    }
    .auth-right {
      height: 100%;
    }
  }
}
</style>
