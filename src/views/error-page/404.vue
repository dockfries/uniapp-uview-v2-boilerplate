<template>
  <view :class="['container', { dark: darkMode }]">
    <Navbar />
    <u-empty mode="page" icon="http://cdn.uviewui.com/uview/empty/page.png" />
  </view>
</template>
<script>
import Navbar from "@/components/common/Navbar.vue";
export default {
  components: { Navbar },
};
</script>
