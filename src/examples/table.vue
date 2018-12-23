<script>

  (function e (t, n, r) {
    function s (o, u) {
      if (!n[o]) {
        if (!t[o]) {
          var a = typeof require == 'function' && require
          if (!u && a) return a(o, !0)
          if (i) return i(o, !0)
          var f = new Error('Cannot find module \'' + o + '\'')
          throw f.code = 'MODULE_NOT_FOUND', f
        }
        var l = n[o] = {exports: {}}
        t[o][0].call(l.exports, function (e) {
          var n = t[o][1][e]
          return s(n ? n : e)
        }, l, l.exports, e, t, n, r)
      }
      return n[o].exports
    }

    var i = typeof require == 'function' && require
    for (var o = 0; o < r.length; o++) s(r[o])
    return s
  })({
    1: [function (require, module, exports) {
      'use strict'

      Object.defineProperty(exports, '__esModule', {
        value: true
      })
      exports.default = {
        watch: {
          'model': {
            handler: function handler (value) {
              this.$emit('changed-model', value)
            },
            deep: true
          },

          'open': function open (val) {
            var el = document.querySelector('#table-field-wrapper')

            if (!el) {
              var el = document.createElement('div')
              el.setAttribute('id', 'table-field-wrapper')
              document.body.appendChild(el)
            }

            el.innerHTML = ''

            if (val) {
              el.appendChild(this.$el.querySelector('.overlay'))
            }
          }
        },
        data: function data () {
          return {
            open: false,
            tableData: {
              plugin: 'tableblok',
              thead: ['Head 1', 'Head 2'],
              tbody: [['one', 'two']]
            }
          }
        },
        props: ['model'],
        computed: {
          cols: function cols () {
            if (this.model && this.model.plugin == 'tableblok') {
              return this.model.thead.length
            }
            return 0
          }
        },
        created: function created () {
          if (!this.model || this.model.plugin != 'tableblok') {
            this.model = this.tableData
          }
        },
        methods: {
          resetFn: function resetFn (e) {
            var textarea = e.currentTarget
            textarea.style.overflowY = 'scroll'
            textarea.style.height = '42px'
          },
          autogrowFn: function autogrowFn (e) {
            var maxAllowedHeight = 300
            var newHeight = 0
            var hasGrown = false
            var textarea = e.currentTarget

            if (textarea.scrollHeight > maxAllowedHeight) {
              textarea.style.overflowY = 'scroll'
              newHeight = maxAllowedHeight
            } else {
              textarea.style.overflowY = 'hidden'
              textarea.style.height = 'auto'
              newHeight = textarea.scrollHeight + 2
              hasGrown = true
            }

            textarea.style.height = newHeight + 'px'
            return hasGrown
          },
          initWith: function initWith () {
            return {
              plugin: 'table'
            }
          },
          moveRow: function moveRow (index, offset) {
            this.move(this.model.tbody, index, offset)
          },
          move: function move (elements, index, offset) {
            var newIndex = index + offset

            if (newIndex > -1 && newIndex < elements.length) {
              var removedElement = elements.splice(index, 1)[0]
              elements.splice(newIndex, 0, removedElement)
            }
          },
          moveCol: function moveCol (index, offset) {
            var _this = this

            this.move(this.model.thead, index, offset)
            this.model.tbody.forEach(function (item) {
              _this.move(item, index, offset)
            })
          },
          close: function close () {
            this.open = false
          },
          openOverlay: function openOverlay () {
            this.open = true
          },
          addRow: function addRow (index) {
            var row = []
            for (var i = 0; i < this.cols; i++) {
              row[i] = ''
            }

            if (typeof index !== 'undefined') {
              this.model.tbody.splice(index + 1, 0, row)
            } else {
              this.model.tbody.push(row)
            }
          },
          addCol: function addCol () {
            this.model.thead.push('')
            this.model.tbody.forEach(function (item) {
              item.push('')
            })
          },
          removeCol: function removeCol (index) {
            this.model.thead.splice(index, 1)
            this.model.tbody.forEach(function (item) {
              item.splice(index, 1)
            })
          },
          removeRow: function removeRow (index) {
            this.model.tbody.splice(index, 1)
          }
        }
      }
      if (module.exports.__esModule) module.exports = module.exports.default

      (typeof module.exports === 'function' ? module.exports.options : module.exports).template = '<div><button class="uk-width-1-1 uk-button" v-on:click.prevent=openOverlay>Edit Table</button><div v-if=open><div class="overlay overlay--open uk-form"><div class=overlay__inner><div class=uk-clearfix><div class=uk-float-right><a v-on:click=close class=uk-button><i class=uk-icon-close></i> Ready</a></div><h1 class=overlay__headline>Table</h1></div><div class=overlay__list style="padding: 50px 58px"><div class=uk-position-relative><table class="uk-table uk-table-hover plugin__tabledata"><thead><tr><template track-by=$index v-for="th in model.thead"><th><div style="position: absolute; top: -20px"><a style="margin-right: 8px" v-on:click=removeCol($index)><i class=uk-icon-close></i></a> <a style="margin-right: 8px" v-on:click="moveCol($index, -1)"><i class=uk-icon-caret-left></i></a> <a v-on:click="moveCol($index, 1)"><i class=uk-icon-caret-right></i></a></div><textarea @focus=autogrowFn @blur=resetFn @input=autogrowFn($event) v-model=th type=text class=uk-width-1-1 rows=1></textarea></template><tbody><template track-by=$index v-for="(trkey, tr) in model.tbody"><tr><template track-by=$index v-for="td in tr"><td><div v-if=!$index style="position: absolute; left: -56px"><a style="margin-right: 8px" v-on:click=removeRow(trkey)><i class=uk-icon-close></i></a> <a style="margin-right: 8px" v-on:click="moveRow(trkey, 1)"><i class=uk-icon-caret-down></i></a> <a style="margin-right: 8px" v-on:click="moveRow(trkey, -1)"><i class=uk-icon-caret-up></i></a><br><a v-on:click=addRow(trkey)><i class=uk-icon-plus></i></a></div><textarea @focus=autogrowFn @blur=resetFn @input=autogrowFn($event) v-model=td type=text class=uk-width-1-1 rows=1></textarea></template></template></table><button v-on:click.prevent=addRow() class=uk-button>Add Row</button> <button style="position: absolute;right: -80px;top: 35px;transform: rotate(90deg);width: 100px" v-on:click.prevent=addCol class=uk-button>Add Col</button></div><div style=margin-top:50px>JSON-Output: {{model|json}}</div></div></div></div></div></div>'

    }, {}], 2: [function (require, module, exports) {
      'use strict'

      var _Plugin = require('../Plugin.vue')

      var _Plugin2 = _interopRequireDefault(_Plugin)

      function _interopRequireDefault (obj) {
        return obj && obj.__esModule ? obj : {default: obj}
      }

      var init = _Plugin2.default.methods.initWith()

      window.storyblok.field_types[init.plugin] = _Plugin2.default

    }, {'../Plugin.vue': 1}]
  }, {}, [2])
</script>
