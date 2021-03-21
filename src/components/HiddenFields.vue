<script>
  import { isNaN } from '../utils'

  function stringifyValue(value) {
    if (typeof value === 'string') return value
    // istanbul ignore else
    if (value != null && !isNaN(value)) return JSON.stringify(value)
    // istanbul ignore next
    return ''
  }

  export default {
    name: 'vue-treeselect--hidden-fields',
    inject: [ 'instance' ],
    functional: true,

    render(_, context) {
      const { instance } = context.injections

      if (!instance.name || instance.disabled || !instance.hasValue) return null

      let stringifiedValues = instance.internalValue.map(stringifyValue)

      if (instance.multiple && instance.joinValues) stringifiedValues = [
        stringifiedValues.join(instance.delimiter),
      ]

      console.log('instance.name is:', instance.name)
      console.log('stringified values is:', JSON.stringify(stringifiedValues))
      return stringifiedValues.map((stringifiedValue, i) => (
        <input type="hidden"
          name={instance.name}
          value={stringifiedValue}
          key={'hidden-field-' + i}
        />
      ))
    },
  }
</script>
