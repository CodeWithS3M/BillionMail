<template>
	<Collapse :default-expanded-names="['1', '2']">
		<n-collapse-item title="Header" name="1">
			<div class="py-8px">
				<HeaderForm v-model:value="blockStyle" />
			</div>
		</n-collapse-item>
		<n-collapse-item title="Container" name="2">
			<div class="py-8px">
				<ContainerForm v-model:value="containerStyle" />
			</div>
		</n-collapse-item>
	</Collapse>
</template>

<script lang="tsx" setup>
import { useConfig } from '../../hooks/useConfig'
import { useSetData } from '../../hooks/useSetData'
import { useNormalForm } from '../style/useNormalForm'

import Collapse from '../shared/Collapse.vue'

const { selectedBlockKey, blockConfigMap } = useConfig()
const { autoSaveFn } = useSetData()

const attr = computed({
	get() {
		return blockConfigMap.value[selectedBlockKey.value].attr
	},
	set(newVal) {
		blockConfigMap.value[selectedBlockKey.value].attr = newVal
	},
})

const blockStyle = computed({
	get() {
		return blockConfigMap.value[selectedBlockKey.value].style
	},
	set(newVal) {
		blockConfigMap.value[selectedBlockKey.value].style = newVal
	},
})

const containerStyle = computed({
	get() {
		return blockConfigMap.value[selectedBlockKey.value].containerStyle
	},
	set(newVal) {
		blockConfigMap.value[selectedBlockKey.value].containerStyle = newVal
	},
})

watch(
	() => [attr.value, blockStyle.value, containerStyle.value],
	() => {
		autoSaveFn()
	},
	{
		deep: true,
	}
)

const [HeaderForm] = useNormalForm([
	{
		attrKey: 'head',
		key: 'fontSize',
	},
	{
		attrKey: 'fontWeight',
	},
	{
		attrKey: 'fontSize',
	},
	{
		attrKey: 'color',
		label: 'Text Color',
	},
	{
		attrKey: 'lineHeight',
	},
	{
		attrKey: 'letterSpacing',
	},
])

const [ContainerForm] = useNormalForm([
	{
		attrKey: 'textAlign',
	},
	{
		attrKey: 'padding',
	},
])
</script>
