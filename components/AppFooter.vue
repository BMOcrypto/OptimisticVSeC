<template>
  <SfFooter :column="4" multiple id="footer">
    <SfFooterColumn :title="$t('About us')">
      <SfList>
        <SfListItem v-for="item in aboutUs" :key="item">
          <SfMenuItem
            :data-cy="`app-foter-url_about-us_${item
              .split(' ')
              .join('-')
              .toLowerCase()}`"
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('Categories')">
      <SfList>
        <SfListItem v-for="item in categories" :key="item">
          <SfMenuItem
            :data-cy="`app-foter-url_categories_${item
              .split(' ')
              .join('-')
              .toLowerCase()}`"
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('Help')">
      <SfList>
        <SfListItem v-for="item in help" :key="item">
          <SfMenuItem
            :data-cy="`app-foter-url_help_${item
              .split(' ')
              .join('-')
              .toLowerCase()}`"
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('Payment & Delivery')">
      <SfList>
        <SfListItem v-for="item in paymentsDelivery" :key="item">
          <SfMenuItem
            :data-cy="`app-foter-url_payment_${item
              .split(' ')
              .join('-')
              .toLowerCase()}`"
            :label="$t(item)"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn title="Let's get Social">
      <div class="footer__socials">
        <SfImage
          class="footer__social-image"
          v-for="item in social"
          :key="item"
          :src="'/icons/' + item + '.svg'"
          width="22"
          height="22"
        />
      </div>
    </SfFooterColumn>
  </SfFooter>
</template>

<script>
import { SfFooter, SfList, SfImage, SfMenuItem } from "@storefront-ui/vue";

export default {
  components: {
    SfFooter,
    SfList,
    SfImage,
    SfMenuItem,
  },
  data() {
    return {
      aboutUs: ["The Flavorman Promise", "Quality in the details", "Reviews"],
      categories: [
        "Flavor & Concentrate",
        "Color & Cloud",
        "Herbal & Dry Blend",
        "Home",
      ],
      help: ["Customer service", "Minimum Order guide", "Contact us"],
      paymentsDelivery: ["Purchase terms", "Guarantee"],
      social: ["facebook", "pinterest", "twitter", "youtube"],
      isMobile: false,
      desktopMin: 1024,
    };
  },
};
</script>

<style lang="scss">
.sf-footer {
  &__container {
    --footer-margin: var(--spacer-sm);
    @include for-desktop {
      --footer-margin: var(--spacer-2xl) auto var(--spacer-xl);
      --footer-padding: 0;
    }
  }
}
.footer {
  &__socials {
    display: flex;
    justify-content: space-between;
    margin: 0 0 var(--spacer-lg) 0;
    padding: var(--spacer-base) var(--spacer-xl);
    @include for-desktop {
      justify-content: flex-start;
      padding: var(--spacer-base) 0;
    }
  }
  &__social-image {
    margin: 0 var(--spacer-lg) 0 0;
  }
}
</style>
