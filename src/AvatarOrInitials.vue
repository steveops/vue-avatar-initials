<template>
  <div>
    <div class="avatar">
      <img class="avatar-image"
           v-if="image"
           :style="radius"
           :width="width"
           :height="height"
           :src="image">
      <div v-else class="avatar-initials" :style="initialsStyle">
        <span class="initials strong">
          {{ initials }}
        </span>
      </div>
    </div>
  </div>
</template>

<style>
  .avatar-initials {
    display: flex;

    background: white;
    justify-content: center;
    align-items: center;
  }
</style>

<script>
  const md5 = require('md5')

  export default {
    name: 'avatar-or-initials',
    props: {
      image: {
        required: false,
        default: false
      },
      title: {
        type: String,
        required: true
      },
      size: {
        required: false,
        default: 40
      },
      round: {
        type: Boolean,
        required: false,
        default: false
      }
    },
    data() {
      return {
        colours: [
          '#f44336',
          '#e91e63',
          '#9c27b0',
          '#673ab7',
          '#3f51b5',
          '#2196f3',
          '#03a9f4',
          '#00bcd4',
          '#009688',
          '#4caf50',
          '#8bc34a',
          '#cddc39',
          '#ffeb3b',
          '#ffc107',
          '#ff9800',
          '#ff5722',
          '#795548',
          '#9e9e9e',
          '#607d8b'
        ]
      }
    },
    methods: {
      colour() {
        const length = this.colours.length
        const seed = md5(this.title)
        const selected = seed.replace(/[A-Za-z]/g, '') % length
        return this.colours[selected]
      },
      textColour() {
        const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(this.colour())
        const red = parseInt(result[1], 16)
        const green = parseInt(result[2], 16)
        const blue = parseInt(result[3], 16)
        const luminosity = ((0.299 * red) + (0.587 * green) + (0.114 * blue))

        if (luminosity > 186) {
          return '#000000'
        }

        return '#FFFFFF'
      }
    },
    computed: {
      width() {
        return this.size + 'px'
      },
      height() {
        return this.size + 'px'
      },
      initials() {
        return this.title.charAt(0)
      },
      initialsStyle() {
        return 'width: ' + this.size + 'px; height: ' + this.size + 'px; border-radius: ' + this.size + 'px; background-color: ' + this.colour() + '; text-transform: uppercase; color: ' + this.textColour() + ';'
      },
      radius() {
        if (this.round) {
          return 'border-radius: ' + this.size + 'px'
        }
      }
    }
  }
</script>
