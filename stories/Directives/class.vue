<script>
import Dump from '../../components/Dump'

import classDirective from '../../directives/class'
import classDirectivePrefix from '../../mixins/classDirectivePrefix'
import { normalizeBindingValue } from 'linna-util/composeClassnamesDirective'

import codeSample from './code/class.txt'

export default {
  name: 'ClassPage',

  components: {
    Dump
  },

  directives: {
    'class': classDirective
  },

  mixins: [classDirectivePrefix],

  data () {
    return {
      codeSample,
      interval: null,

      prefix: 'MyPrefix',
      rawNumberValue: 1,
      stringValue: 'mod',

      appliedClasses: null,

      value1: 'one',
      value2: 'one two three',
      value3: ['one two three', 'one-two-three', 'three']
    }
  },

  computed: {

    numberValue: {
      get () {
        return this.rawNumberValue
      },
      set (value) {
        this.rawNumberValue = value ? parseInt(value) : 0
      }
    },

    value4 () {
      return {
        'one': true,
        'two': false,
        'three': this.stringValue,
        'four': this.numberValue,
        'one two three': true,
        'one-two-three': false
      }
    }

  },

  mounted () {
    this.interval = setInterval(this.getAppliedClasses, 100)
  },

  beforeDestroy () {
    if (this.interval) {
      clearInterval(this.interval)
    }
  },

  methods: {
    normalizeBindingValue,

    getAppliedClasses () {
      this.appliedClasses = (this.$el ? this.$el.getAttribute('class') : null)
    }

  }

}

</script>

<template>
  <div
    v-class="value4"
    class="foo bar"
    :class="{
      bar: 'foo'
    }"
  >

    <h1><code>v-class</code></h1>

    <p class="disclaimer">
      Like <code>:class</code> but prefixes class names. Works on any element within a component. Supports a custom prefix defined with <code>classesDirectivePrefix</code> and automatically adds the component name to the prefix as well.
    </p>

    <p v-class="'test'">
      If you want to use the same static prefix for all components throughout your application, register a global mixin that adds <code>classesDirectivePrefix</code> to every component.
    </p>

    <dump :value="appliedClasses" />

    <p>
      <input v-model="classesDirectivePrefix">
      <input v-model="stringValue">
      <input
        v-model="numberValue"
        type="number"
      >
    </p>

    <html-block :source="codeSample" />

    <h3>Normalization</h3>

    <p>
      <input v-model="prefix">
    </p>

    <table>
      <tbody>

        <tr>
          <td><js-block :source="value1" /></td>
          <td><js-block :source="normalizeBindingValue(value1, prefix)" /></td>
        </tr>

        <tr>
          <td><js-block :source="value2" /></td>
          <td><js-block :source="normalizeBindingValue(value2, prefix)" /></td>
        </tr>

        <tr>
          <td><js-block :source="value3" /></td>
          <td><js-block :source="normalizeBindingValue(value3, prefix)" /></td>
        </tr>

        <tr>
          <td><js-block :source="value4" /></td>
          <td><js-block :source="normalizeBindingValue(value4, prefix)" /></td>
        </tr>

      </tbody>
    </table>

  </div>
</template>

<style lang="scss">

.c-class-page-four-1 {
  .disclaimer {
    color: $red;
  }
}

.c-class-page-four-2 {
  .disclaimer {
    color: $green;
  }
}

.c-class-page-four-3 {
  .disclaimer {
    color: $blue;
  }
}

.c-class-page-four-4 {
  .disclaimer {
    color: $purple;
  }
}

.c-class-page-four-5 {
  .disclaimer {
    color: $orange;
  }
}

</style>
