<template>
  <div class="ratingselect">
    <div class="ratingType border-1px">
      <span @click="select(2)" class="block positive" :class="{'active': selectTypeForSelf === 2}">{{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span @click="select(0)" class="block positive" :class="{'active': selectTypeForSelf === 0}">{{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span @click="select(1)" class="block negative" :class="{'active': selectTypeForSelf === 1}">{{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div @click="toggleContent" class="switch" :class="{'on':onlyContentForSelf === true}">
      <i class="icon-check_circle"></i>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
  const NEGATIVE = 1;
  const POSITIVE = 0;
  const ALL = 2;
  export default{
    props: {
      ratings: {
        type: Array,
        default() {
          return [];
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default() {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          };
        }
      }
    },
    data() {
      return {
        selectTypeForSelf: this.selectType,
        onlyContentForSelf: this.onlyContent
      };
    },
    computed: {
      positives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        });
      },
      negatives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE;
        });
      }
    },
    methods: {
      select(type) {
        // vue2移除了props的双向绑定，在组件内，不能修改由外层传来的props数据。
        this.selectTypeForSelf = type;
        this.$bus.emit('ratingtypeSelect', type);
      },
      toggleContent() {
        this.onlyContentForSelf = !this.onlyContentForSelf;
        this.$bus.emit('toggleOnlyContent', this.onlyContentForSelf);
      }
    }
  };
</script>

<style lang="stylus" rel="sytlesheet/stylus">
  @import '../../common/stylus/mixin.styl';
  .ratingselect
    .ratingType
      padding: 18px 0
      margin: 0 18px
      border-1px(rgba(7, 17, 27, 0.1))
      font-size: 0
      .block
        display: inline-block
        padding: 8px 12px
        margin-right: 8px
        border-radius: 1px
        line-height: 16px
        font-size: 12px
        color: rgb(77, 85, 93)
        &.active
          color: #fff
        .count
          font-size: 8px
        &.positive
          background: rgba(0, 160, 220, 0.2)
          &.active
            background: rgba(0, 160, 220, 1)
        &.negative
          background: rgba(77, 85, 93, 0.2)
          &.active
            background: rgba(77, 85, 93, 1)
    .switch
      padding: 12px 18px
      border-bottom: 1px solid rgba(7, 17, 27, 0.1)
      font-size: 0
      &.on
        .icon-check_circle
          color: #00c850
      .icon-check_circle
        display: inline-block
        vertical-align: middle
        line-height: 24px
        font-size: 24px
        color: rgb(147, 153, 159)
      .text
        display: inline-block
        vertical-align: middle
        line-height: 24px
        font-size: 12px
        color: rgb(147, 153, 159)
</style>
