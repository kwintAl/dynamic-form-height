<template>
  <div class="grid">
    <!-- DynamicCard component for cards in data value -->
    <div :key="index" class="col" v-for="(card, index) in cards">
      <!-- title-height is the headerHeight -->
      <DynamicCard :key="index" :title="card.title" @heightChange="updateTitleHeight" :id="card.id"
                   :title-height="headerHeight"/>
    </div>
  </div>
</template>

<script>
import DynamicCard from "@/components/DynamicCard.vue";
import {ref} from "vue";

export default {
  name: 'CardContainer',
  components: {DynamicCard},
  props: {
    msg: String
  },
  data: () => ({
    cards: [
      {title: 'Card 1', id: 'card1'},
      {title: 'Card 2', id: 'card2'},
      {title: 'Lorem ipsum dolor sit amet, consectetur adipisicing', id: 'card3'}
    ],
  }),
  setup() {
    let headerHeight = ref(undefined);
    let cardTitleSizes = ref({});
    let biggestTitle = ref(undefined);

    return {headerHeight, cardTitleSizes, biggestTitle};
  },
  methods: {
    updateTitleHeight(id, h) {
      console.log(id, h);

      if (Object.entries(this.cardTitleSizes).length === 0) {
        console.log('resetting cardTitleSizes');
        this.$data.cards.forEach(card => {
          this.cardTitleSizes[card.id] = undefined;
        });
      }

      this.cardTitleSizes[id] = h;
      if (this.biggestTitle === undefined && Object.entries(this.cardTitleSizes).every(e => e[1] !== undefined)) {
        // get the element with the highest value
        let elem = Object.entries(this.cardTitleSizes).reduce((a, b) => a[1] > b[1] ? a : b);
        console.log('biggest element:', elem);

        this.biggestTitle = elem;
      }
      if (this.biggestTitle) {
        console.log('biggest title height:', this.biggestTitle[1]);
        this.headerHeight = this.cardTitleSizes[this.biggestTitle[0]];
      }
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
