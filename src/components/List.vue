<template>
  <div class="c-list">
    <div class="c-list__title">
      <slot></slot>
    </div>
    <ul class="c-list__content u-list--bare u-small-scrollbar">
      <li v-for="element in items" :key="element.key">{{ element }}</li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Vue } from "vue-class-component";
import { Prop } from "vue-property-decorator";

export default class List extends Vue {
  @Prop() title!: string;
  @Prop({ required: true }) items!: string[];
}
</script>

<style lang="scss" scoped>
@import "~@/assets/scss";

.c-list {
  display: flex;
  flex-direction: column;
  flex: 1;
  margin: 0 15px 15px;

  &:not(:first-of-type) {
    margin-left: 0;
  }
}

.c-list__title {
  font-weight: $font-weight--large;
  font-size: $font-size--large;
  margin-top: 15px;
  margin-bottom: 5px;
}

.c-list__content {
  line-height: 16px;
  flex: 1;
  background-color: $colour-background-secondary--transparent;
  border: 1px solid $colour-background--dark;
  margin: 0;
  padding: 6px 10px 10px;
  font-size: $font-size--small;
  overflow-y: scroll;
}
</style>
