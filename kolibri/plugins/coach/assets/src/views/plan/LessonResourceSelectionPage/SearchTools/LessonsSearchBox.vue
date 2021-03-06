<template>

  <form
    class="box"
    :class="searchClasses"
    @submit.prevent="search"
    @keydown.esc.prevent="clearSearchTerm"
  >
    <div
      class="box-row"
      :style="{ borderColor: $themeTokens.fineLine }"
    >
      <label
        class="visuallyhidden"
        for="searchfield"
      >
        {{ $tr('searchBoxLabel') }}
      </label>

      <input
        ref="searchinput"
        v-model.trim="searchTerm"
        type="search"
        class="input"
        :class="$computedClass(inputPlaceHolderStyle)"
        :style="{ color: $themeTokens.text }"
        dir="auto"
        :placeholder="$tr('searchBoxLabel')"
      >

      <div class="buttons-wrapper">
        <UiIconButton
          color="black"
          size="small"
          class="clear-button"
          :class="searchTerm === '' ? '' : 'clear-button-visible'"
          :ariaLabel="$tr('clearButtonLabel')"
          @click="clearSearchTerm"
        >
          <mat-svg
            name="clear"
            category="content"
          />
        </UiIconButton>

        <div class="submit-button-wrapper" :style="{ backgroundColor: $themeTokens.primary }">
          <UiIconButton
            type="secondary"
            color="white"
            class="submit-button"
            :disabled="!searchTermHasChanged"
            :ariaLabel="$tr('startSearchButtonLabel')"
            :style="{ fill: $themeTokens.textInverted }"
            @click="search"
          >
            <mat-svg
              name="search"
              category="action"
            />
          </UiIconButton>
        </div>
      </div>
    </div>

  </form>

</template>


<script>

  import themeMixin from 'kolibri.coreVue.mixins.themeMixin';
  import UiIconButton from 'kolibri.coreVue.components.UiIconButton';

  export default {
    name: 'LessonsSearchBox',
    components: {
      UiIconButton,
    },
    mixins: [themeMixin],
    data() {
      return {
        searchTerm: this.$route.params.searchTerm || '',
      };
    },
    computed: {
      searchClasses() {
        return this.$computedClass({
          borderColor: this.$themeTokens.annotation,
        });
      },
      searchTermHasChanged() {
        return this.searchTerm !== this.$route.params.searchTerm;
      },
      inputPlaceHolderStyle() {
        return {
          '::placeholder': {
            color: this.$themeTokens.annotation,
          },
        };
      },
    },
    methods: {
      clearSearchTerm() {
        this.searchTerm = '';
        this.$refs.searchinput.focus();
      },
      search() {
        if (this.searchTerm !== '') {
          this.$emit('searchterm', this.searchTerm);
        }
      },
    },
    $trs: {
      searchBoxLabel: 'Search',
      clearButtonLabel: 'Clear',
      startSearchButtonLabel: 'Start search',
    },
  };

</script>


<style lang="scss" scoped>

  @import '~kolibri.styles.definitions';

  // TODO - consolidate with kFilterTextbox
  .box {
    margin-right: 8px;
  }

  .box-within-action-bar {
    width: 235px;
  }

  .box-row {
    display: table;
    width: 100%;
    border-style: solid;
    border-width: 2px;
    border-radius: $radius;
  }

  .input {
    position: relative;
    left: 8px;
    display: table-cell;
    width: 100%;
    height: 36px;
    padding: 0;
    margin: 0;
    vertical-align: middle;
    border: 0;

    // removes the IE clear button
    &::-ms-clear {
      display: none;
    }
  }

  .buttons-wrapper {
    display: table-cell;
    width: 78px;
    height: 36px;
    text-align: right;
    vertical-align: middle;
  }

  .clear-button {
    width: 24px;
    height: 24px;
    margin-right: 6px;
    margin-left: 6px;
    vertical-align: middle;
    visibility: hidden;
  }

  .clear-button-visible {
    visibility: visible;
  }

  .submit-button {
    width: 36px;
    height: 36px;
  }

  .submit-button-wrapper {
    display: inline-block;
    vertical-align: middle;
    border-top-right-radius: 2px;
    border-bottom-right-radius: 2px;
  }

  .filter-icon {
    position: absolute;
    top: 50%;
    bottom: 50%;
    margin-left: 12px;
    transform: translate(-50%, -50%);
  }

  .filter:nth-of-type(1) {
    margin-right: 16px;
  }

  .filter {
    margin-bottom: 16px;
    margin-left: 28px;
  }

  .filters {
    margin-top: 24px;
  }

  .ib {
    position: relative;
    display: inline-block;
  }

</style>
