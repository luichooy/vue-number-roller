<!--
- example
- <number-roller
    :num="1234.5678"
    :bit="2"
    prefix="$"
    suffix="%"></number-roller>
-->

<template>
  <span ref="count">{{ c }}</span>
</template>

<script>
export default {
  name: 'number-roller',
  props: {
    num: {
      type: Number,
      required: true
    },
    bit: {
      type: Number,
      default: 0
    },
    prefix: {
      type: String,
      default: ''
    },
    suffix: {
      type: String,
      default: ''
    },
    duration: {
      type: Number,
      default: 2000
    },
    delay: {
      type: Number,
      default: 0
    }
  },
  watch: {
    num () {
      this.count()
    }
  },
  data () {
    return {
      d: 0,
      i: 50
    }
  },
  computed: {
    c () {
      return this.prefix + this.d.toFixed(this.bit) + this.suffix
    }
  },
  methods: {
    count () {
      let num = this.num
      let duration = this.duration
      let i = this.i
      let times = Math.ceil(duration / i)
      let step = num / times
      let temp = 0

      this.ti = setInterval(() => {
        if (times < 1 || (temp >= num - step)) {
          this.d = num
          this.clear()
          return
        }
        this.d = temp += step
        times--
      }, i)
    },
    clear () {
      clearInterval(this.ti)
      clearTimeout(this.to)
    },
    isInViewport (el) {
      const viewportHeight =
        window.innerHeight ||
        document.documentElement.clientHeight ||
        document.body.clientHeight
      const elTop = el.getBoundingClientRect && el.getBoundingClientRect().top
      return elTop < viewportHeight
    },
    triggerChange () {
      const element = this.$refs.count
      if (this.isInViewport(element)) {
        this.to = setTimeout(this.count, this.delay)
        document.removeEventListener('scroll', this.triggerChange, false)
      }
    }
  },
  mounted () {
    const element = this.$refs.count
    if (this.isInViewport(element)) {
      this.to = setTimeout(this.count, this.delay)
    } else {
      document.addEventListener('scroll', this.triggerChange, false)
    }
  }
}
</script>