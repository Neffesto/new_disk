<template>
  <section>
    <NewDisk />
  </section>
</template>

<script>
// @ is an alias to /src
import NewDisk from "@/components/newdisk/NewDisk";

export default {
  name: 'HomeView',
  components: {
    NewDisk,
  }
}
</script>
