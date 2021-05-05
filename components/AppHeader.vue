<template>
  <SfHeader
    data-cy="app-header"
    @click:cart="toggleCartSidebar"
    @click:wishlist="toggleWishlistSidebar"
    @click:account="handleAccountClick"
    @enter:search="changeSearchTerm"
    @change:search="(p) => (term = p)"
    :searchValue="term"
    :cartItemsQty="cartTotalItems"
    :accountIcon="accountIcon"
    class="sf-header--has-mobile-search"
  >
    <!-- TODO: add mobile view buttons after SFUI team PR -->
    <template #logo>
      <nuxt-link
        data-cy="app-header-url_logo"
        :to="localePath('/')"
        class="sf-header__logo"
      >
        <SfImage
          src="/icons/logo.png"
          alt="Flavorman"
          class="sf-header__logo-image"
        />
      </nuxt-link>
    </template>
    <template #navigation>
      <SfHeaderNavigationItem
        class="nav-item"
        data-cy="app-header-url_women"
        label="FLAVOR & CONCENTRATE"
        :link="localePath('/c/women')"
      />
      <SfHeaderNavigationItem
        class="nav-item"
        data-cy="app-header-url_men"
        label="COLOR & CLOUD"
        :link="localePath('/c/men')"
      />
      <SfHeaderNavigationItem
        class="nav-item"
        data-cy="app-header-url_men"
        label="HERBAL & DRY BLEND"
        :link="localePath('/c/men')"
      />
    </template>
    <template #aside>
      <LocaleSelector class="smartphone-only" />
    </template>
  </SfHeader>
</template>

<script>
import { SfHeader, SfImage } from "@storefront-ui/vue";
import { useUiState } from "~/composables";
import {
  useCart,
  useWishlist,
  useUser,
  cartGetters,
} from "@vue-storefront/commercetools";
import { computed, ref } from "@vue/composition-api";
import { onSSR } from "@vue-storefront/core";
import { useUiHelpers } from "~/composables";
import LocaleSelector from "./LocaleSelector";

export default {
  components: {
    SfHeader,
    SfImage,
    LocaleSelector,
  },
  setup(props, { root }) {
    const {
      toggleCartSidebar,
      toggleWishlistSidebar,
      toggleLoginModal,
    } = useUiState();
    const { changeSearchTerm, getFacetsFromURL } = useUiHelpers();
    const { isAuthenticated, load: loadUser } = useUser();
    const { cart, load: loadCart } = useCart();
    const { load: loadWishlist } = useWishlist();
    const term = ref(getFacetsFromURL().term);

    const cartTotalItems = computed(() => {
      const count = cartGetters.getTotalItems(cart.value);
      return count ? count.toString() : null;
    });

    const accountIcon = computed(() =>
      isAuthenticated.value ? "profile_fill" : "profile"
    );

    // TODO: https://github.com/DivanteLtd/vue-storefront/issues/4927
    const handleAccountClick = async () => {
      if (isAuthenticated.value) {
        return root.$router.push("/my-account");
      }

      toggleLoginModal();
    };

    onSSR(async () => {
      await loadUser();
      await loadCart();
      await loadWishlist();
    });

    return {
      accountIcon,
      cartTotalItems,
      handleAccountClick,
      toggleCartSidebar,
      toggleWishlistSidebar,
      changeSearchTerm,
      term,
    };
  },
};
</script>

<style lang="scss" scoped>
.sf-header {
  --header-padding: var(--spacer-sm);
  @include for-desktop {
    --header-padding: 0;
  }
  &__logo-image {
    width: 200px;
    height: auto;
  }
}

.nav-item {
  --header-navigation-item-margin: 0 var(--spacer-base);
}
</style>
