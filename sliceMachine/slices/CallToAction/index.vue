<template>
  <div>
    <prismic-image
          class="header__image"
          :field="slice.primary.icon_image"
        />
    <section class="container container--full-height">
    <div class="header">
      <slot name="header" v-bind="slice.primary">
        <h1 class="header__title">
          {{ $prismic.asText(slice.primary.title) }}
        </h1>
        <h4 class="header__subtitle">
          {{ $prismic.asText(slice.primary.paragraph) }}
        </h4>
      </slot>
    </div>
    <div class="cta">
      <slot name="cta" v-bind="slice.primary">
        <button
          class="cta__button cta__button"
          @click="e => handleLink(e, slice.primary.button_link)"
        >
          {{ slice.primary.button_label }}
        </button>
      </slot>
    </div>
  </section>
  </div>
  
</template>
<script>
export default {
  name: 'CallToAction',
  props: {
    slice: {
      type: Object,
      required: true
    }
  },
  methods: {
    handleLink({ metaKey }, linkData) {
      /** Logic could be extracted from prismic-vue/Link instead */
      const link = this.$prismic.asLink(linkData)
      if (linkData.link_type === 'Web') {
        if (metaKey) {
          return window.open(link, '_newtab')
        }
        return (location.href = link)
      }
      return this.$router.push(link)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../styles/variables';

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  padding: 2rem 0;
  width: 90%;
  min-height: 50vh;
  max-width: $screen-lg-min;
  text-align: center;
  // &--full-height {
  //   min-height: 100vh;
  // }
}

.header {
  * {
    margin: 0 auto;
    margin-bottom: 2rem;
    width: 100%;
  }

  &__image {
    margin: 80px 0 80px;
    padding: 10em 0 8em;
    background-size: cover;
    background-position: center;
    line-height: 1.75;
    text-align: center;
  }

  &__title {
    font-size: 42px;
    line-height: 48px;
    @include md {
      font-size: 5vw;
    }
    @include lg {
      font-size: 70px;
      line-height: 84px;
    }
  }
  &__subtitle {
    width: 90%;
    max-width: calc((940px / 3) * 2);
  }
}

.cta {
  margin: 0 auto;
  max-width: 640px; // Should be sm
  *:not(:last-child) {
    margin-right: 0.6rem;
  }
  &__button {
    background: $color-primary;
    border: 1px solid $color-primary;
    cursor: pointer;
    padding: 12px 16px;
    border-radius: 3px;
    font-size: $font-size-button;
    color: #fff;
    -webkit-box-shadow: 0 2px 4px 0 rgba(136, 136, 136, 0.24);
    -moz-box-shadow: 0 2px 4px 0 rgba(136, 136, 136, 0.24);
    box-shadow: 0 2px 4px 0 rgba(136, 136, 136, 0.24);

    &--secondary {
      background: #fff;
      border: 1px solid #fff;
      color: $text-primary;
    }
  }
}
</style>
