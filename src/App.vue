<template>
  <div id="app">
    <GoogleChart :risk_ids="{
		low: 'top10--low--has_cppi--cf455040-22de-4972-afc5-98d5c1a645e5',
		mid: 'top10--low--has_cppi--cf455040-22de-4972-afc5-98d5c1a645e5',
		high: 'top10--high--has_cppi--cf455040-22de-4972-afc5-98d5c1a645e5'
}"/>
  </div>
</template>

<script>


import GoogleChart from "@/components/GoogleChart";

export default {
  name: 'App',
  components: {
    GoogleChart
  }
}
</script>
