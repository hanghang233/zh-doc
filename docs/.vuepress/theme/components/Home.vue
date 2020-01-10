<template>
    <main class="home" aria-labelledby="main-title">
          <header class="hero">
               <div class="model-left"></div>
               <div class="model-right"></div>
               <div style="position: relative;z-index: 2">
                   <img
                           v-if="data.heroImage"
                           :src="$withBase(data.heroImage)"
                           alt="hero"
                   >
                   <h1 id="main-title">{{ data.heroText || $title || 'Hello' }}</h1>
                   <p class="description">
                       {{ data.tagline || $description || 'Welcome to your VuePress site' }}
                   </p>

                   <p
                           class="action"
                           v-if="data.actionText && data.actionLink"
                   >
                       <NavLink
                               class="action-button"
                               :item="actionLink"
                       />
                   </p>
               </div>
          </header>
          <div
                  class="features"
                  v-if="data.features && data.features.length"
          >
            <div
                    class="feature"
                    v-for="(feature, index) in data.features"
                    :key="index"
            >
              <h2>{{ feature.title }}</h2>
              <p>{{ feature.details }}</p>
            </div>
          </div>

          <Content class="custom"/>

          <div
                  class="footer"
                  v-if="data.footer"
          >
            {{ data.footer }}
          </div>
    </main>
</template>

<script>
import NavLink from './NavLink.vue'
import './../styles/homeBg.styl'
export default {

  components: { NavLink },
  computed: {
    data () {
      return this.$page.frontmatter
    },

    actionLink () {
      return {
        link: this.data.actionLink,
        text: this.data.actionText
      }
    }
  }
}
</script>

<style lang="stylus">
  .home
    padding $navbarHeight 2rem 0
    max-width 980px
    margin 0px auto
    display block
    .hero
      text-align center
      position relative
      .home-bg
        height 100%
        width 100%
        max-width 100%
      img
        max-width: 100%
        max-height 280px
        display block
        margin 3rem auto 1.5rem
        border-radius: 50%;
        background-color: #d9dce0;
      h1
        font-size 3rem
      h1, .description, .action
        margin 1.8rem auto
      .description
        //max-width 35rem
        //color lighten($textColor, 40%)
        font-size 1.6rem
        line-height 1.3
        color: #ffffff;
        font-weight: 200;
        background: black;
        border-top: 2px solid #fff;
        border-bottom: 2px solid #fff;
      .action-button
        padding: 0.8rem 1.6rem;
        background: #474644;
        color #fff;
        border-radius: 15px;
        border-bottom: 5px solid #33322f;
        border-left: 3px solid #000000;
        box-shadow: -3px 5px 10px 3px rgba(51, 50, 47, 0.5), 5px -5px 20px 10px rgba(51, 50, 47, 0.5) inset, 0px 2px 1px rgba(0, 0, 0, .7);
        -webkit-transition: -webkit-box-shadow .15s ease-in-out;
        &:active
            border-bottom: 1px solid #33322f;
            top: 5px;
            border-left: 1px solid #000000;
            margin-right: 2px;
            box-shadow: -2px 4px 7px 1px rgba(51, 50, 47, 0.7), 5px -5px 20px 10px rgba(51, 50, 47, 0.5) inset, -1px 2px 1px rgba(0, 0, 0, .7);
            z-index: -1;
        &:hover
            color #3cba92
    .features
      border-top 1px solid $borderColor
      padding 1.2rem 0
      margin-top 2.5rem
      display flex
      flex-wrap wrap
      align-items flex-start
      align-content stretch
      justify-content space-between
    .feature
      flex-grow 1
      flex-basis 30%
      max-width 30%
      h2
        font-size 1.4rem
        font-weight 500
        border-bottom none
        padding-bottom 0
        color lighten($textColor, 10%)
      p
        color lighten($textColor, 25%)
    .footer
      padding 2.5rem
      border-top 1px solid $borderColor
      text-align center
      color lighten($textColor, 25%)

  @media (max-width: $MQMobile)
    .home
      .features
        flex-direction column
      .feature
        max-width 100%
        padding 0 2.5rem

  @media (max-width: $MQMobileNarrow)
    .home
      padding-left 1.5rem
      padding-right 1.5rem
      .hero
        img
          max-height 210px
          margin 2rem auto 1.2rem
        h1
          font-size 2rem
        h1, .description, .action
          margin 1.2rem auto
        .description
          font-size 1.2rem
        .action-button
          font-size 1rem
          padding 0.6rem 1.2rem
      .feature
        h2
          font-size 1.25rem
</style>
