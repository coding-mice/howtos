### How to map ID to the value using the attributes index?
Using a mixin:

1. create a file `src/themes/default/mixins/attributeValueById/index.js` with the following content
```
import { mapGetters } from 'vuex'

export default {
  data () {
    return {
      label: ''
    }
  },
  computed: {
    ...mapGetters({
      attributesByCode: 'attribute/attributeListByCode'
    })
  },
  methods: {
    getAttributeValueById (id, code) {
      if (id && code && this.attributesByCode[code]) {
        let attr = this.attributesByCode[code].options.filter(el => el.value === id)
        try {
          return attr.length ? attr[0].label : '?';
        } catch (e) {
          console.debug(e)
        }
      }
    }
  }
}

```
2. In your template `src/themes/default/components/core/ProductDetail.vue` , modify the `export` node so it contains:
```
import fakeLabel from 'theme/mixins/attributeValueById'

export default {
  mixins: [ attributeValueById ],
  components: {
    (...)
  }
}
```

3. In the template itself, add:
```
<li v-if="product.material">
  <span class="uppercase">{{ $t('material') }}</span>: {{ getAttributeValueById(product.material, 'material') }}
</li>
```
