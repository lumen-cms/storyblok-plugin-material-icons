<template>
    <div>
        <div class="uk-form-row">
            <label class="uk-form-label">Icon Name</label>
            <div class="select select--inline" :class="{'select--open':openSelect}">
                <a class="select__btn ellipsis" @click.stop="openSelect=!openSelect">
                    {{model.name || 'Please select...'}}
                </a>

                <div class="select__dropdown" :style="{'display':openSelect?'block':'none'}">
                    <div class="select__type-search">
                        <input type="text" class="uk-width-1-1 js-term-input" v-model="search" placeholder="Search">
                    </div>
                    <div style="max-height: 200px; overflow-y: auto; ">
                        <div class="select__opt-box"
                             v-for="item in listItems"
                             :key="item.id">
                            <div @click="selectItem(item)">
                                {{item.name}}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
  // source of material icon list: https://gist.githubusercontent.com/AmirOfir/daee915574b1ba0d877da90777dc2181/
  import iconObject from './material-icnos.list'

  let preparedList = []
  iconObject.categories.forEach(category => {
    preparedList = preparedList.concat(category.icons)
  })
  console.log(preparedList)
  export default {
    mixins: [window.Storyblok.plugin],
    data () {
      return {
        search: '',
        openSelect: false
      }
    },
    computed: {
      listItems () {
        if (this.search) {
          return preparedList.filter(item => item.name.includes(this.search))
        }
        return preparedList
      }
    },
    methods: {
      selectItem (item) {
        this.$set(this.model, 'name', item.ligature)
        this.openSelect = false
      },
      initWith () {
        return {
          plugin: 'material-icons',
          name: null
        }
      },
      pluginCreated () {
        // console.log('plugin:created')
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
    .p-metatags__google-title {
        color: blue;
        text-decoration: underline;
    }

    .p-metatags__google-link {
        color: green;
    }

    .p-metatags__preview {
        margin: 5px 0 15px;
        padding: 10px;
        color: #000;
        background: #FFF;
    }
</style>
