<template>
  <div>
    <h1>Events</h1>
    <EventCard
      v-for="(event, index) in events"
      :key="index"
      :event="event"
      :data-index="index"
    />
  </div>
</template>

<script>
    import EventCard from '@/components/EventCard.vue'

    export default {
      components: {
        EventCard
      },
      async asyncData({ $axios, error }) {
        // async await function is very useful for nested promise function.
        try {
          const { data } = await $axios.get('http://localhost:3000/events')
          return {
            events: data
          }
        } catch (e) {
          error({
            statusCode: 503,
            message: 'Unable to fetch events at this time. Please try again.'
          })
        }
      },
      // asyncData({ $axios, error }) {
      //   return $axios
      //     .get('http://localhost:3000/events')
      //     .then(response => {
      //       return {
      //         events: response.data // returned data is merged to the commponent data
      //       }
      //     })
      //     .catch(e => {
      //       error({
      //         statusCode: 503,
      //         message: 'Unable to fetch events at this time. Please try again.'
      //       })
      //     })
      // },
      head() { // <-- property used by vue-meta to add header tags
        return {
          title: 'Event Listing', // <-- For our title tag
          meta: [
            {
              hid: 'description',
              name: 'description', // <-- for our meta description tag
              content:
                'Where you can find all the events taking place in your neighborhood'
            }
          ]
        }
      },

    }
</script>