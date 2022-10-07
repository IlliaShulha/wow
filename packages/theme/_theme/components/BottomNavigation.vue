<template>
  <!-- TODO: create logic with isActive prop for BottomNavigationItems -->
  <WowBottomNavigation class="navigation-bottom smartphone-only">
    <BottomNavigationItem
      :class="route.path == '/' ? 'bottom-navigation__item--active' : ''"
      icon="homeHex" iconSize="24px" label="Home"
      @click="handleHomeClick"
    />
    <!--<BottomNavigationItem icon= "more" size="20px" label="Menu" @click="toggleMobileMenu"/>-->
    <BottomNavigationItem icon="empty_cart" iconSize="24px" label="Cart" @click="toggleCartSidebar"/>
    <!--<BottomNavigationItem icon="profile" size="20px" label="Account" @click="handleAccountClick"/>-->
    <!-- TODO: add logic for label - if on Home then Basket, if on PDC then AddToCart etc. -->
    <BottomNavigationItem
      label="Menu"
      icon="threeDotsHor"
      iconSize = "24px"
      @click="toggleMobileMenu"
    >
    <!--
      <template #icon>
        <div class = "more_options_icon">
        <SfCircleIcon class="cart-button" aria-label="MoreOptions">
          <SfIcon
            icon="more"
            color="white"
            size="25px"
            :style="{margin: '0 0 0 -2px'}"
          />
        </SfCircleIcon>
        
      </div>
      </template>
      -->
    </BottomNavigationItem>
  </WowBottomNavigation>
</template>

<script>
import { WowBottomNavigation, SfIcon, SfCircleIcon, SfBadge } from '@storefront-ui/vue';
import { useUiState } from '~/composables';
import { useUser, useCart, cartGetters } from '@vue-storefront/wow';
import { computed, useRoute, useRouter } from '@nuxtjs/composition-api';
import { addBasePath } from '@vue-storefront/core';


export default {
  components: {
    WowBottomNavigation,
    SfIcon,
    SfCircleIcon,
    SfBadge
  },
  setup() {
    const route = useRoute();
    const router = useRouter();
    const { toggleCartSidebar, toggleWishlistSidebar, toggleLoginModal, toggleMobileMenu, isMobileMenuOpen } = useUiState();
    const { isAuthenticated } = useUser();
    const { cart } = useCart();

    const handleAccountClick = async () => {
      if (isAuthenticated.value) {
        return router.push('/my-account');
      }
      toggleLoginModal();
    };

    const handleHomeClick = () => {
      isMobileMenuOpen.value ? toggleMobileMenu() : false;
      router.push('/');
    };

    const cartTotalItems = computed(() => {
      const count = cartGetters.getTotalItems(cart.value);

      return count ? count.toString() : null;
    });
    const menuOptionIcon = addBasePath("/wow-images-png/steampower.png");
    return {
      route,
      isMobileMenuOpen,
      toggleWishlistSidebar,
      toggleCartSidebar,
      toggleMobileMenu,
      cartTotalItems,
      handleAccountClick,
      handleHomeClick,
      menuOptionIcon
    };
  }
};
</script>
<style lang="scss" scoped>
.navigation-bottom {
  --bottom-navigation-z-index: 3;
}
.cart-button {
  position: relative;
}
.cart-badge {
  position: absolute;
  top: 0;
  right: 0;
}
.more_options_icon {
  transform: rotate(90deg);
}
</style>
