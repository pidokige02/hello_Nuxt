<template>
  <div>
    <!-- <h1>Event #{{ id }}</h1> -->
    <h1>{{ event.title }}</h1>
  </div>
</template>
<script>
export default {
  async asyncData({ $axios, error, params }) {
    // paremeter 가 넘어오는 종류는 clear 하지 않다.
    try {
      const { data } = await $axios.get(
        'http://localhost:3000/events/' + params.id
      )
      return {
        event: data
        // return 값은 component 의 data 로 merge 가 된다
      }
    } catch (e) {
      error({
        statusCode: 503,
        message: 'Unable to fetch event #' + params.id
      })
    }
  },
  head() {
    return {
      // title: 'Event #' + this.id, // SEO 에 dynamic id 가 표시가 된다
      title: this.event.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          // content: 'What you need to know about event #' + this.id
          content: 'What you need to know about ' + this.event.title
        }
      ]
    }
  },


  // computed: {
  //   id() {
  //     return this.$route.params.id
  //     // http:localhost:3000/event/3 에서 this.$route.params.id 은 3을 얻어올 수 있다.
  //   }
  // }
}
</script>