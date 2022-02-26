<template>
  <div id="app">
    <GoogleChart low='top10--low--has_cppi--cf455040-22de-4972-afc5-98d5c1a645e5'
                 mid='top10--low--has_cppi--cf455040-22de-4972-afc5-98d5c1a645e5'
                 high='top10--low--has_cppi--cf455040-22de-4972-afc5-98d5c1a645e5'/>
  </div>
</template>

<script>


import GoogleChart from "@/components/GoogleChart.ce";

export default {
  name: 'App',
  components: {
    GoogleChart
  }
}
</script>
