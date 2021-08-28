<template>
  <div class="uk-navbar-right">
    <ul class="uk-navbar-nav">
      <li class="uk-margin-right">IP Address: {{ ipInfo.address }}</li>
      <li class="uk-margin-right">Location: {{ ipInfo.state }}</li>
      <li class="uk-margin-right">Timezone: {{ ipInfo.timezone }}</li>
      <li class="uk-margin-right">ISP: {{ ipInfo.isp }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name:'ip-panel',
  props: ['ipInfo']
}
</script>