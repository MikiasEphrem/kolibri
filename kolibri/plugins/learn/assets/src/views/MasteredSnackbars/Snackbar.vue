<template>

  <div class="snackbar" :style="{ backgroundColor: $themeColors.palette.grey.v_100 }">
    <div class="table">
      <div class="row">
        <div class="icon-container cell">
          <slot name="icon"></slot>
        </div>
        <div class="content-container cell">
          <slot name="content"></slot>
        </div>
        <div class="close-container cell">
          <UiIconButton
            size="small"
            :ariaLabel="$tr('close')"
            @click="$emit('close')"
          >
            <mat-svg name="close" category="navigation" />
          </UiIconButton>
        </div>
      </div>
    </div>
    <slot name="alert"></slot>
  </div>

</template>


<script>

  import themeMixin from 'kolibri.coreVue.mixins.themeMixin';
  import UiIconButton from 'kolibri.coreVue.components.UiIconButton';

  export default {
    name: 'Snackbar',
    components: {
      UiIconButton,
    },
    mixins: [themeMixin],
    $trs: {
      close: 'Close',
    },
  };

</script>


<style lang="scss" scoped>

  @import '~kolibri.styles.definitions';

  .snackbar {
    @extend %dropshadow-12dp;

    position: fixed;
    right: 8px;
    bottom: 8px;
    z-index: 24;
    width: 304px;
    padding: 8px;
    font-size: 14px;
    animation-duration: 0.3s;
    animation-timing-function: ease;
    animation-fill-mode: both;
  }

  .table {
    display: table;
  }

  .row {
    display: table-row;
  }

  .cell {
    display: table-cell;
    vertical-align: middle;
  }

  .content-container {
    width: 99%;
    padding: 0 16px;
  }

</style>
