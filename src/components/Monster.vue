<template lang="pug">
    .monster-wrapper
        .monster
            img(:src="pic" v-if="pic" ref="monster")

        .effect
            img(src="@/assets/img/zangeki.png" ref="effect")
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import { TimelineMax } from "gsap";

@Component
export default class Monster extends Vue {
  ///////////
  /* props */
  ///////////
  @Prop({
    default: ""
  })
  readonly pic!: string;
  @Prop({
    default: 0
  })
  readonly finishCount!: boolean;

  //////////
  /* data */
  //////////
  timeline: TimelineMax | null = null;

  /////////////
  /* mounted */
  /////////////
  mounted(): void {
    this.setupTimeline();
  }

  /////////////
  /* methods */
  /////////////
  setupTimeline(): void {
    this.$nextTick(() => {
      this.timeline = new TimelineMax();
      this.timeline
        .set(this.$refs.effect, {
          autoAlpha: 1.0
        })
        .set(
          this.$refs.monster,
          {
            autoAlpha: 0
          },
          "+=0.1"
        )
        .set(
          this.$refs.monster,
          {
            autoAlpha: 1
          },
          "+=0.1"
        )
        .set(
          this.$refs.monster,
          {
            autoAlpha: 0
          },
          "+=0.1"
        )
        .set(
          this.$refs.monster,
          {
            autoAlpha: 1
          },
          "+=0.1"
        )
        .set(
          this.$refs.effect,
          {
            autoAlpha: 0
          },
          "+=0.1"
        );
      this.timeline.pause();
    });
  }

  playTimeline(): void {
    if (this.timeline) this.timeline.restart();
  }
  ///////////
  /* watch */
  ///////////
  @Watch("finishCount")
  onValueChange(): void {
    this.playTimeline();
  }
}
</script>

<style scoped lang="stylus">
.monster-wrapper
    position: relative
.monster
    margin-bottom: 20px;
    height: 200px;
    text-align: center
    > img
        width: 100%
        height: 100%
        object-fit contain
.effect
    position: absolute
    top: 0
    left: 0
    width: 300px
    height: 200px
    pointer-events none
    text-align: center
    & > img
        height: 100%;
        opacity 0
</style>
