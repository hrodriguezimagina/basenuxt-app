<template>
  <div class="tw-flex tw-items-center">
    <div
        class="
        tw-flex tw-flex-col tw-items-start tw-pr-4 lg:tw-border-r
        hover:tw-text-[#DC3545] tw-duration-200 tw-relative tw-order-2 lg:tw-order-1
      "
        :class="{
            'hover:tw-text-secondary lg:tw-border-[#10192333]': !isScrolled,
            'hover:tw-text-[#F9BA48] lg:tw-border-white': isScrolled
      }"
    >      
      <q-btn-dropdown
          v-if="authStore.isLogged()"
          :label="user.fullName"
          flat no-caps padding="0"
          
          dropdown-icon="fa-solid fa-angle-down"
          class="dropdown-sign-in tw-text-[15px] tw-min-w-[140px] tw-w-full tw-overflow-hidden tw-font-[600]"
      >
        <q-list>
          <q-item clickable v-close-popup>
            <q-item-section>
              <q-item-label>
                <NuxtLink
                    :to="{ path: getPath('iauth.logout'), query: { redirectTo: 'icommerce.products' } }"
                    class="dropdown-sign-in tw-text-[15px] "
                >
                  Cerrar sesión
                </NuxtLink>
              </q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-btn-dropdown>

      <q-btn
        v-if="!authStore.isLogged()"
        unelevated
        color="primary"
        no-caps          
        :to="{ path: getPath('iauth.login'), query: { redirectTo: 'icommerce.products' } }"
        class="dropdown-sign-in tw-text-[15px] tw-duration-200 tw-rounded-full xl:tw-px-8"
      >
        <span class="tw-block lg:tw-hidden xl:tw-block">{{ $t('iauth.login.title') }}</span>
        <i class="tw-hidden lg:tw-block xl:tw-hidden fa fa-user" />
    </q-btn>
    </div>

    <div class="tw-pr-4 lg:tw-pl-4 tw-relative tw-order-1 lg:tw-order-2">
      <NuxtLink
          :to="getPath('icommerce.products')"
          class="tw-text-[15px] tw-cursor-pointer tw-border-r tw-border-[#10192333] tw-pr-3 tw-duration-300"
          :class="{
            'hover:tw-text-secondary tw-border-[#10192333]': !isScrolled,
            'hover:tw-text-[#F9BA48] tw-border-white': isScrolled
          }"
      >      
          {{ $t('icommerce.goToStore') }}
      </NuxtLink>
    </div>
  </div>
</template>
<script setup>

const props = defineProps({
  scrollPosition: Object,
  isScrolled :Boolean
});

const authStore = useAuthStore();
const user = computed(() => authStore.user);

</script>
