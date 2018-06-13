<template>
<div>
	<v-text-field :color="color" light v-model="vm_search" @keyup.enter.native="fn_do" autofocus
		:label="label" single-line prepend-icon="search" clearable solo />
</div>
</template>

<script>
export default {
	name: 'cs-search',
	props: {
		color: {
			type: [String,Object],
			default: 'black'
		},
		searchKey: {
			type: [String,Object],
      default: 'Keyword'
		},
		label: {
			type: String,
			default(){
				return 'Search'
			}
		}
	},
	watch:{
		vm_search(){
			if((this.vm_search == null) || (this.vm_search == undefined) || (this.vm_search == '')){
				let params = {};
				params[this.searchKey] = this.vm_search;
				this.$emit('search',params);
			}
			return false
		}
	},
	data(){
		return {
			vm_search: null,
		}
	},
	methods: {
		fn_do(){
			let params = {};
			params[this.searchKey] = this.vm_search;
			this.$emit('search',params);
		}
	}
}
</script>
