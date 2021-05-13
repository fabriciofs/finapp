<script>
export default {
  props: {
    type: {
      type: Number,
      default: 3
    }
  },

  computed: {
    formatedDate () {
      const today = this.$day()
      const filterDate = this.$store.state.filter.date
      const filterPeriod = this.$store.state.filter.period
      let format = 'MMMM'

      switch (filterPeriod) {
        case 'day':
          if (today.isSame(filterDate, 'year')) {
            format = 'DD MMMM'
          }
          else {
            format = 'DD MMMM YYYY'
          }
          break

        case 'week':
          if (today.isSame(filterDate, 'week')) {
            return this.$t('dates.week.current')
          }
          else if (today.subtract(1, filterPeriod).isSame(filterDate, 'week')) {
            return this.$t('dates.week.last')
          }
          const date = this.$day(filterDate)
          const startDate = date.startOf('week').format('D MMMM')
          const endDate = date.endOf('week').format('D MMMM')
          return `${startDate} - ${endDate}`

        case 'month':
          if (today.isSame(filterDate, 'year')) {
            format = 'MMMM'
            break
          }
          format = 'MMMM YYYY'
          break

        case 'year':
          format = 'YYYY'
          break
      }

      return this.$day(filterDate).format(format)
    }
  }
}
</script>

<template lang="pug">
.date
  template(v-if="$store.state.filter.period === 'all'") {{ $t('dates.all.simple') }}
  template(v-else) {{ formatedDate }}
</template>

<style lang="stylus" scoped>
.date
  display flex
  align-items center

  &:first-letter
    text-transform uppercase
</style>
