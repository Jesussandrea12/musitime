<template lang="pug">
  main
    pm-loader(v-show="isLoading")

    .container(v-if="track && track.id")
      .columns
        .column.is-3.has-text-centered
          figure.media-left
            p.image
              img(:src="track.album.images[0].url")
            p.button-bar
              a.button.is-primary.is-large
                span.icon(@click="selectTrack") ▶️

        .column.is-8
          .panel
            .panel-heading
              h1.title {{ trackTitle }}
            .panel-block
              article.media
                .content
                  ul(v-for="(v, k) in track")
                    li
                      strong {{ k }}:&nbsp;
                      span {{ v }}

                nav.level
                  .level-left
                    a.level-item
</template>

<script>
import { mapState, mapActions, mapGetters } from 'vuex'

import PmLoader from '@/components/shared/Loader.vue'

import trackMixin from '@/mixins/track'

export default {
  mixins: [trackMixin],
  components: { PmLoader },

  data () {
    return {
      isLoading: false
    }
  },

  computed: {
    ...mapState(['track']),
    ...mapGetters(['trackTitle'])
  },

  created () {
    const id = this.$route.params.id
    this.isLoading = true
    if (this.track.album) {
      this.isLoading = false
    }

    if (!this.track || !this.track.id || this.track.id !== id) {
      this.getTrackByid({ id })
        .then(() => {
          console.log('track loaded ...')
        })
    }
  },

  methods: {
    ...mapActions(['getTrackById'])
  }
}
</script>

<style lang="scss">
  .column {
    margin: 20px;
  }

  .button-bar {
    margin-top: 20px;;
  }
</style>
