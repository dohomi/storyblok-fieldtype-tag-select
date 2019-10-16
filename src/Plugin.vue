<template>
    <div>
        <div class="uk-form-row" :style="{'min-height': openSelect ? '400px' : 'auto'}">
            <multiselect multiple
                         v-model="model.values"
                         class="uk-width-1-1"
                         :options="optionValues"
                         placeholder="Select class names"
                         :hide-selected="true"
                         :clear-on-select="false"
                         @open="onMultiOpen"
                         @close="onMultiClose">
            </multiselect>
        </div>
    </div>
</template>

<script>
  import Multiselect from 'vue-multiselect'
  import 'vue-multiselect/dist/vue-multiselect.min.css'

  export default {
    components: {
      Multiselect
    },
    mixins: [window.Storyblok.plugin],
    data () {
      return {
        search: '',
        selected: 0,
        openSelect: false,
        optionValues: []
      }
    },
    methods: {
      onMultiOpen () {
        this.openSelect = true
      },
      onMultiClose () {
        this.openSelect = false
      },
      initWith () {
        return {
          // needs to be equal to your storyblok plugin name
          plugin: 'tags-select'
        }
      },
      pluginCreated () {
        console.log('plugin:created')
        this.api.get('cdn/tags', {version: 'draft'})
          .then((res) => {
            this.optionValues = res.data.tags && res.data.tags.map(i => i.name)
          })
      }
    },
    watch: {
      'model': {
        handler: function (value) {
          this.$emit('changed-model', value)
        },
        deep: true
      }
    }
  }
</script>


<style>

    .multiselect__input {
        border: none !important;
    }

    .text-block {
        display: block;
    }

    .text-block:hover {
        background-color: floralwhite;
    }

    .text-block.active {
        background-color: floralwhite;
    }

    .padding-sm {
        padding: 5px;
    }

    .margin-0 {
        margin: 0;
    }

    .color-danger {
        color: lightcoral;
    }

    .color-danger:hover {
        color: darkred;
    }
</style>
