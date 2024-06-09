<script>
import Card from 'primevue/card';

export default {
  name: 'dynamic-card',
  created() {
    window.addEventListener('resize', this.titleResized);
  },
  mounted() {
    this.titleResized();
    if(this.titleHeight) {
      this.$refs.titleElement.style.height = `${this.titleHeight}px`;
    }
  },
  unmounted() {
    window.removeEventListener("resize", this.titleResized);
  },
  updated() {
    console.log('updated ', this.title, this.titleHeight, this.$refs.titleElement.clientHeight);

    if(this.titleHeight && this.$refs.titleElement.clientHeight !== this.titleHeight) {
      console.log('updated ', this.title, this.titleHeight);
      // this.$refs.titleElement.style.height = undefined;
      // this.titleResized();
      this.$refs.titleElement.style.height = `${this.titleHeight}px`;
    }
  },
  props: {
    id: String,
    title: String,
    titleHeight: Number
  },
  emits: {
    heightChange: null
  },
  components: {
    Card
  },
  methods: {
    titleResized() {
      const height = this.$refs.titleElement.clientHeight
      // print to console
      // console.log('titleResized, height: ', height);
      this.$emit('heightChange', this.id, height);
    }
  }
}

</script>

<template>
  <Card>
    <template #title>
      <p ref="titleElement" @click="titleResized">{{title}}</p>
    </template>
    <template #content>
      <p class="m-0">
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Inventore sed consequuntur error repudiandae numquam deserunt quisquam repellat libero asperiores earum nam nobis, culpa ratione quam perferendis esse, cupiditate neque
        quas!
      </p>
    </template>
  </Card>
</template>

<style scoped>

</style>