<template>
  <div>
    <div class="header-avon">
      <div class="topbar">
        <nav>
          <div class="mobile-menu">
            <div
              @click="() => toggleMenu()"
              v-bind:class="{ closeMenu: menuOpen }"
            >
              <span></span>
              <span></span>
              <span></span>
              <m>MENU</m>
            </div>
            <sidemenu
              class="smartphone-only"
              v-bind:style="{ left: menuLeftPosition }"
            >
              <div class="top_navigation">
                <a
                  v-for="category in categoryList"
                  :href="'/c/' + category.slug"
                  :key="category.id"
                  >{{ category.name
                  }}<SfIcon
                    icon="chevron_right"
                    class="chevron_right"
                    size="xxs"
                    color="grey"
                    viewBox="0 0 24 24"
                    :coverage="1"
                /></a>
              </div>
              <div class="category_navigation">
                <a
                  :href="'/c/' + category.slug"
                  v-for="category in subCategories"
                  :key="category.id"
                  >{{ category.name
                  }}<SfIcon
                    class="chevron_right"
                    icon="chevron_right"
                    size="xxs"
                    color="grey"
                    viewBox="0 0 24 24"
                    :coverage="1"
                /></a>
              </div>
            </sidemenu>
          </div>
        </nav>
        <div class="logo">
          <nuxt-link
            data-cy="app-header-url_logo"
            :to="localePath('/')"
            class="sf-header__logo"
          >
            <SfImage
              src="/homepage/logo.png"
              alt="Vue Storefront Next"
              class="sf-header__logo-image"
            />
          </nuxt-link>
        </div>
        <div class="account-block">
          <div class="account">
            <svg viewBox="0 0 23.8 29.5" id="Svg_avatar-user">
              <path
                d="m 0,29.48381 c 0,0 0.39974,-11.36425 4.04061,-14.39467 4.90355,4.60886 9.16066,4.7208 14.83661,-10e-6 3.65566,3.10787 4.86136,14.39468 4.86136,14.39468 z"
                fill="currentColor"
              ></path>
              <ellipse
                cx="11.553617"
                cy="7.8760314"
                rx="7.9549513"
                ry="7.8760333"
                fill="currentColor"
              ></ellipse>
            </svg>
          </div>
          <div class="cart-bag">
            <svg viewBox="0 0 21 21" id="Svg_bag">
              <path
                fill="currentColor"
                d="m 1.573013,0.95981479 17.995268,0 0,2.45390021 1.289871,0.7865065 -0.03146,14.4087985 c -0.214975,0.747788 -0.472995,1.289367 -1.761774,1.541553 l -16.9885398,0 C 1.0964915,20.037816 0.37578471,19.618757 0.31460259,18.420259 l 0,-14.282958 1.22695011,-0.6292052 z"
              ></path>
              <circle
                r="1.2898706"
                cy="7.3147874"
                cx="6.04037"
                style="
                  fill: #ffffff;
                  stroke-width: 1px;
                  stroke-linecap: butt;
                  stroke-linejoin: miter;
                  stroke-opacity: 1;
                "
              ></circle>
              <circle
                style="
                  fill: #ffffff;
                  fill-opacity: 1;
                  fill-rule: evenodd;
                  stroke: none;
                  stroke-width: 1px;
                  stroke-linecap: butt;
                  stroke-linejoin: miter;
                  stroke-opacity: 1;
                "
                cx="14.975083"
                cy="7.3147874"
                r="1.2898706"
              ></circle>
              <path
                d="m 6.6381147,8.4473566 c -0.029178,3.6032694 2.146418,4.6204984 4.0326393,4.6375324 1.86741,0.01686 3.659503,-1.470167 3.769505,-4.700453 l 1.25841,-0.125841 c -0.09959,4.565509 -2.467711,6.106906 -5.128022,6.10329 C 6.8231278,14.320164 5.3929038,11.946652 5.4111646,8.2271347 Z"
                style="
                  fill: #ffffff;
                  fill-opacity: 1;
                  fill-rule: evenodd;
                  stroke: none;
                  stroke-width: 1px;
                  stroke-linecap: butt;
                  stroke-linejoin: miter;
                  stroke-opacity: 1;
                "
              ></path>
              <path
                d="m 2.9258041,2.1867649 15.2267659,0 -1.22695,1.2584104 0.471903,0.5977449 -13.7481329,0 0.5033641,-0.5977449 z"
                style="
                  fill: #ffffff;
                  fill-rule: evenodd;
                  stroke: none;
                  stroke-width: 1px;
                  stroke-linecap: butt;
                  stroke-linejoin: miter;
                  stroke-opacity: 1;
                  fill-opacity: 1;
                "
              ></path>
            </svg>
            <span class="cart-count">3</span>
          </div>
        </div>
      </div>
      <div class="seperator-avon"></div>
      <SfSearchBar
        ref="searchBarRef"
        :placeholder="$t('Search for items')"
        aria-label="Search"
        class="sf-header__search"
        :value="term"
        @input="handleSearch"
        @keydown.enter="handleSearch($event)"
        @focus="isSearchOpen = true"
        @keydown.esc="closeSearch"
        v-click-outside="closeSearch"
      >
        <template #icon>
          <SfButton
            v-if="!!term"
            class="sf-search-bar__button sf-button--pure"
            @click="closeOrFocusSearchBar"
          >
            <span class="sf-search-bar_ _icon">
              <SfIcon color="var(--c-text)" size="18px" icon="cross" />
            </span>
          </SfButton>
          <SfButton
            v-else
            class="sf-search-bar__button sf-button--pure"
            @click="
              isSearchOpen ? (isSearchOpen = false) : (isSearchOpen = true)
            "
          >
            <span class="sf-search-bar__icon">
              <SfIcon color="var(--c-text)" size="20px" icon="search" />
            </span>
          </SfButton>
        </template>
      </SfSearchBar>
    </div>
  </div>
</template>

<script>
import {
  SfImage,
  SfIcon,
  SfButton,
  SfBadge,
  SfSearchBar,
  SfOverlay,
} from "@storefront-ui/vue";
import { useUiState } from "~/composables";
import {
  useCart,
  useWishlist,
  useUser,
  cartGetters,
  useFacet,
  useCategory,
} from "@vue-storefront/commercetools";
import { computed, ref, onBeforeUnmount, watch } from "@vue/composition-api";
import { onSSR } from "@vue-storefront/core";
import { useUiHelpers } from "~/composables";
import LocaleSelector from "./LocaleSelector";
import SearchResults from "~/components/SearchResults";
import { clickOutside } from "@storefront-ui/vue/src/utilities/directives/click-outside/click-outside-directive.js";
import {
  mapMobileObserver,
  unMapMobileObserver,
} from "@storefront-ui/vue/src/utilities/mobile-observer.js";
import debounce from "lodash.debounce";

export default {
  components: {
    SfImage,
    LocaleSelector,
    SfIcon,
    SfButton,
    SfBadge,
    SfSearchBar,
    SearchResults,
    SfOverlay,
  },
  directives: { clickOutside },
  setup(props, { root }) {
    const {
      toggleCartSidebar,
      toggleWishlistSidebar,
      toggleLoginModal,
    } = useUiState();
    const {
      setTermForUrl,
      getFacetsFromURL,
      getSearchTermFromUrl,
    } = useUiHelpers();
    let menuLeftPosition = ref("-1000px");
    let menuOpen = ref(false);
    const { result, search } = useFacet();
    const { isAuthenticated, load: loadUser } = useUser();
    const { cart, load: loadCart } = useCart();
    const { load: loadWishlist } = useWishlist();
    const term = ref(getFacetsFromURL().term);
    const isSearchOpen = ref(false);
    const searchBarRef = ref(null);
    const { categories: cat1, search: search1 } = useCategory("categories");

    const subCategories = computed(() => {
      return {
        ...cat1.value.filter((cat) => {
          return cat.parent != null;
        }),
      };
    });

    const categoryList = computed(() => {
      return cat1.value.filter((cat) => {
        return cat.parent === null;
      });
    });

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

    const toggleMenu = () => {
      menuLeftPosition.value = !menuOpen.value ? "0px" : "-1000px";
      menuOpen.value = !menuOpen.value;
      console.log("toggling", menuLeftPosition.value);
    };

    onSSR(async () => {
      await loadUser();
      await loadCart();
      await loadWishlist();
      await search1({});
    });

    const closeSearch = () => {
      if (!isSearchOpen.value) return;

      term.value = "";
      isSearchOpen.value = false;
      setTermForUrl(term.value);
    };

    const handleSearch = debounce(async (paramValue) => {
      if (!paramValue.target) {
        term.value = paramValue;
      } else {
        term.value = paramValue.target.value;
      }
      setTermForUrl(term.value);
      await search(getSearchTermFromUrl(term.value));
    }, 1000);

    const isMobile = computed(() => mapMobileObserver().isMobile.get());

    const closeOrFocusSearchBar = () => {
      if (isMobile.value) {
        return closeSearch();
      } else {
        term.value = "";
        return searchBarRef.value.$el.children[0].focus();
      }
    };

    watch(
      () => term.value,
      (newVal, oldVal) => {
        const shouldSearchBeOpened =
          !isMobile.value &&
          term.value.length > 0 &&
          ((!oldVal && newVal) ||
            (newVal.length !== oldVal.length && isSearchOpen.value === false));
        if (shouldSearchBeOpened) {
          isSearchOpen.value = true;
        }
      }
    );

    onBeforeUnmount(() => {
      unMapMobileObserver();
    });

    return {
      subCategories,
      toggleMenu,
      menuLeftPosition,
      menuOpen,
      accountIcon,
      cartTotalItems,
      handleAccountClick,
      toggleCartSidebar,
      toggleWishlistSidebar,
      setTermForUrl,
      term,
      isSearchOpen,
      closeSearch,
      handleSearch,
      result,
      closeOrFocusSearchBar,
      searchBarRef,
      isMobile,
      categoryList,
    };
  },
};
</script>

<style lang="scss" scoped>
.seperator-avon {
  border-bottom: solid;
  border-image-slice: 1;
  border-bottom-width: 3px;
  border-image-source: linear-gradient(
    to right,
    #7f28c4 -6%,
    #e2197c 62%,
    #e5231b 100%
  );
}

.sf-header {
  --header-padding: var(--spacer-sm);
  @include for-desktop {
    --header-padding: 0;
  }
  &__logo-image {
    height: 100%;
  }
}
.header-on-top {
  z-index: 2;
}
.nav-item {
  --header-navigation-item-margin: 0 var(--spacer-base);
}

.cart-badge {
  position: absolute;
  bottom: 40%;
  left: 40%;
}
.header-avon {
  box-shadow: 0px -9px 20px grey;
  padding-bottom: 6px;
  .topbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 15px 15px 5px 15px;
    .mobile-menu {
      .closeMenu {
        span {
          &:nth-child(1) {
            transform: rotate(45deg);
          }
          &:nth-child(2) {
            display: none;
          }
          &:nth-child(3) {
            transform: rotate(135deg);
            margin-top: -5px;
            margin-bottom: 7px;
          }
        }
      }
      div {
        span {
          transition: 0.1s ease-in;
          width: 33px;
          border-top: 5px solid black;
          display: block;
          margin-top: 5px;
          border-radius: 16px;
          &:first-child {
            margin-top: 0;
          }
        }
        m {
          font-size: 11px;
          color: #000;
          font-family: var(--font-family);
          font-weight: 500;
        }
      }
    }
    .account-block {
      display: flex;
      align-items: center;
      .account {
        margin-right: 15px;
        #Svg_avatar-user {
          width: 25px;
          color: var(--c-primary);
        }
      }
      .cart-bag {
        position: relative;
        .cart-count {
          background: #000;
          color: #fff;
          font-family: "Roboto";
          font-size: 11px;
          border-radius: 10px;
          position: absolute;
          padding: 3px 6px;
          top: -7px;
          left: -8px;
          border: 1px solid #fff;
        }
        #Svg_bag {
          width: 31px;
          color: var(--c-primary);
        }
      }
    }
  }
  .sf-header__search {
    display: block;
    width: 93%;
    margin: 0 auto;
    background: #f3f3f3;
    border-radius: 25px;
    padding: 0px 17px;
    margin-top: 6px;
  }
  sidemenu.smartphone-only {
    display: flex;
    background: #fff;
    justify-content: flex-start;
    flex-direction: column;
    width: 80%;
    position: absolute;
    z-index: 999;
    left: -1000px;
    top: 106px;
    transition: 0.3s ease-in;
    box-shadow: 7px 10px 23px -13px grey;
    .top_navigation,
    .category_navigation {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      margin-bottom: 25px;
      a {
        padding: 15px 20px;
        width: 88%;
        font-family: var(--font-family);
        text-transform: uppercase;
        font-size: 14px;
        font-weight: 500;
        .chevron_right {
          float: right;
        }
      }
    }
  }
}
</style>
