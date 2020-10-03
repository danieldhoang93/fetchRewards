<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="navColor shadow">
      <q-toolbar class="text-black">
        <div>
          <img src="https://www.fetchrewards.com/assets/FetchRewardsHorizontalLogo.png" class="logo">
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MainLayout',
  data () {
    return {
    }
  }
}
</script>

<style lang="scss">
.q-toolbar{
  height:75px;
}

.logo {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 170px;
}
</style>


