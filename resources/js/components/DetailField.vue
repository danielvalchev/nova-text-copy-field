<template>
    <panel-item :field="field">
        <div slot="value" class="flex" @mouseover="hover = true" @mouseleave="hover = false">
            <div class="flex-no-shrink" v-if="!shouldDisplayAsHtml">{{ fieldDisplayValue }}</div>
            <div class="flex-no-shrink" v-if="shouldDisplayAsHtml" v-html="fieldDisplayValue"></div>
            <copy-button
                :value="copyFieldValue"
                :title="copyButtonTitleValue"
                v-if="hasCopyValue"
                :class="['w-4 mx-3', {'invisible': ! shouldShowButton}]" />
        </div>
    </panel-item>
</template>

<script>
import CopyButton from './CopyButton'
import { filterField, copyButtonTitle, copyValue } from '../utilities'

export default {
    props: ['resource', 'resourceName', 'resourceId', 'field'],
    components: {
       CopyButton
    },
    data() {
        return {
            hover: false
        }
    },
    computed: {
        fieldDisplayValue() {
            return filterField(this.field)
        },
        copyButtonTitleValue() {
            return copyButtonTitle(this.field)
        },
        copyFieldValue() {
            return copyValue(this.field)
        },
        shouldShowButton() {
            if (this.field.show_button_on_hover) {
                return this.hover
            }

            return true
        },
        shouldDisplayAsHtml() {
            return this.field.asHtml
        },
        hasCopyValue() {
            return this.field.copy_value && this.field.value !== '—';
        }
    }
}
</script>
