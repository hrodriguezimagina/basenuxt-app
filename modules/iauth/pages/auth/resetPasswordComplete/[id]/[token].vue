<script setup lang="ts">
import { reactive, ref } from 'vue'
import PasswordValidator from '@/utils/validators/passwordValidator'

definePageMeta({
  //layout: 'dark-bg',
})
const refReset: any = ref(null)
const isPwd = ref(true)
const store = useAuthStore()
const route = useRoute()

const auth = reactive<{
  password: string
  confirmPassword: string
}>({
  password: '',
  confirmPassword: '',
})

const loading = computed(() => store.loading)
const isEmpty = computed(
  () => auth.password == '' || auth.confirmPassword == '',
)
const isDiferent = computed(() => auth.password != auth.confirmPassword)

async function reset() {
  try {
    const validateReset = await refReset.value.validate()
    if (!validateReset) return

    if (route?.params) {
      const formData = {
        password: auth.password,
        passwordConfirmation: auth.confirmPassword,
        userId: route.params.id,
        token: route.params.token,
      }
      store.changedPasswordRequest(formData)
    }
  } catch (error) {
    console.log(error)
  }
}
</script>

<template>
  <div
    class="sign-bg tw-min-h-screen tw-text-gray-900 tw-flex tw-justify-center"
  >
    <div
      class="tw-max-w-screen-xl tw-m-0 sm:tw-m-10 sm:tw-rounded-lg tw-flex tw-justify-center tw-flex-1"
    >
      <div class="tw-p-6 sm:tw-p-12">
        <div class="tw-mt-12 tw-flex tw-flex-col tw-items-center">
          <div class="tw-mb-8">
            <img src="@/assets/svg/logo-green-text.svg" alt="" />
          </div>
          <h1
            class="tw-text-[35px] xl:tw-text-[50px] tw-font-extralight tw-text-white tw-mb-4"
          >
            {{ $t('iauth.reset.title') }}
          </h1>
          <div class="tw-w-full tw-flex-1">
            <div class="">
              <q-form @submit.prevent.stop="reset" ref="refReset">
                <q-input
                  filled                 
                  class="tw-mb-2"
                  v-model="auth.password"
                  :label="$t('iauth.login.inputs.password')"
                  lazy-rules
                  :rules="PasswordValidator.rules"
                  :type="isPwd ? 'password' : 'text'"
                >
                  <template v-slot:prepend>
                    
                  </template>
                  <template v-slot:append>
                    <q-icon
                      :name="isPwd ? 'visibility_off' : 'visibility'"
                      class="cursor-pointer"
                      @click="isPwd = !isPwd"
                    />
                  </template>
                </q-input>
                <q-input
                  filled
                  dark
                  class="tw-mb-2"
                  v-model="auth.confirmPassword"
                  :label="$t('iauth.reset.confirmPassword')"
                  lazy-rules
                  :rules="[
                    ...PasswordValidator.rules,
                    (val) => val == auth.password || 'Password does not match',
                  ]"
                  :type="isPwd ? 'password' : 'text'"
                >
                  <template v-slot:prepend>
                    
                  </template>
                  <template v-slot:append>
                    <q-icon
                      :name="isPwd ? 'visibility_off' : 'visibility'"
                      class="cursor-pointer"
                      @click="isPwd = !isPwd"
                    />
                  </template>
                </q-input>
                <transition name="hero">
                  <q-btn
                    :disabled="isEmpty || isDiferent"
                    type="submit"
                    class="hero tw-mt-5 tw-tracking-wide tw-font-semibold tw-bg-indigo-500 tw-text-gray-100 tw-w-full tw-py-4 tw-rounded-lg tw-hover:bg-indigo-700 tw-transition-all tw-duration-300 tw-ease-in-out tw-flex tw-items-center tw-justify-center"
                  >
                    <span class="tw-ml-3">
                      {{ $t('iauth.reset.submitBtn') }}
                    </span>
                  </q-btn>
                </transition>
              </q-form>
              <p
                class="tw-mt-8 tw-text-sm tw-font-extralight tw-text-white tw-text-center"
              >
                {{ $t('iauth.register.existAccount.content') }}
                <NuxtLink to="/auth/login" class="tw-text-primary tw-ml-1">
                  {{ $t('iauth.register.existAccount.link') }}
                </NuxtLink>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.hero {
  view-transition-name: article-thumb;
}
</style>
