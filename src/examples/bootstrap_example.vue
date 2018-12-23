<script>

  const Fieldtype = {
    mixins: [window.Storyblok.plugin],
    template: `<div><div class="uk-grid">
    <div class="uk-width-1-3">
        <label class="uk-form-label">X-Small</label>
        <select style="padding-right:15px" class="uk-width-1-1" v-model="model.xsmall"><option :value="option" v-for="option in options">{{ option }}</option></select>
    </div>
    <div class="uk-width-1-3">
        <label class="uk-form-label">Small</label>
        <select style="padding-right:15px" class="uk-width-1-1" v-model="model.small"><option :value="option" v-for="option in options">{{ option }}</option></select>
    </div>
    <div class="uk-width-1-3">
        <label class="uk-form-label">Medium</label>
        <select style="padding-right:15px" class="uk-width-1-1" v-model="model.medium"><option :value="option" v-for="option in options">{{ option }}</option></select>
    </div>
</div>
    <div class="uk-grid uk-margin-small-top">
        <div class="uk-width-1-3">
            <label class="uk-form-label">Large</label>
            <select style="padding-right:15px" class="uk-width-1-1" v-model="model.large"><option :value="option" v-for="option in options">{{ option }}</option></select>
        </div>
        <div class="uk-width-1-3">
            <label class="uk-form-label">X-Large</label>
            <select style="padding-right:15px" class="uk-width-1-1" v-model="model.xlarge"><option :value="option" v-for="option in options">{{ option }}</option></select>
        </div>
    </div></div>`,
    methods: {
      initWith () {
        return {
          plugin: 'example_plugin',
          xsmall: '12',
          small: '',
          medium: '',
          large: '',
          xlarge: ''
        }
      }
    },
    data () {
      return {
        options: ['', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12']
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
