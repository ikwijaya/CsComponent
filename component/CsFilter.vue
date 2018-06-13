<template>
	<div v-if="items.length > 0">
			<v-menu transition="slide-x-transition" bottom right :close-on-content-click="true" :nudge-width="140">
				<v-btn color="grey" dark slot="activator" flat >
						<v-icon color="white">filter_list</v-icon>
					<span class="white--text"> {{ ct_title[0] }} {{ ct_title[1] }} ({{ total }})  </span>
				</v-btn>
				<v-list dense>
					<v-list-tile v-for="(k,v) in items" :key="v" @click="changeView(k,items)">
						<v-list-tile-content>
							<v-list-tile-title v-text="k.text"></v-list-tile-title>
						</v-list-tile-content>
						<v-list-tile-action>
							<v-icon v-if="k.select" color="black">check</v-icon>
						</v-list-tile-action>
					</v-list-tile>
				</v-list>
				<v-divider v-if="items2.length > 0"></v-divider>
				<v-list dense>
					<v-list-tile v-for="(k,v) in items2" :key="v" @click="changeView2(k,items2)">
						<v-list-tile-content>
							<v-list-tile-title v-text="k.text"></v-list-tile-title>
						</v-list-tile-content>
						<v-list-tile-action>
							<v-icon v-if="k.select" color="black">check</v-icon>
						</v-list-tile-action>
					</v-list-tile>
				</v-list>
			</v-menu>
	</div>
</template>

<script>
	export default {
		name: 'cs-filter',
		props: {
			items: {
				type: [Array,Object]
			},
			items2: {
				type: [Array,Object],
				default(){
					return []
				}
			},
			filterKey: {
				type: [String,Object],
				default: 'value'
			},
			filterKey2: {
				type: [String,Object],
				default: 'value'
			},
			total: {
				type: [String,Object,Function,Number],
				default(){
					return 0;
				}
			}
		},
		computed:{
			ct_title(){
				return this.check();
			}
		},
		methods: {
			changeView(i,arr){
				this.title_one = i.text;
				this.checkView(i,arr);
				let params = {};
				params[this.filterKey] = i.value;
				console.log(params)
				this.$emit('filter',params);
			},
			changeView2(i,arr){
				this.title_two = i.text;
				this.checkView(i,arr);
				let params = {};
				params[this.filterKey2] = i.value;

				this.$emit('filter2',params);
			},
			checkView(item,arr){
				arr.filter(k => {
					if((k.value == item.value) && (k.text == item.text)){ return k.select = true }	
						return k.select = false
				})
			},
			check(){
				let a,b;
					for(let i in this.items){
						if(this.items[i].select){
							a = this.items[i].text
						}
					}
				for(let i in this.items2){
						if(this.items2[i].select){
							b = this.items2[i].text
						}
					}
					return [a,b];
				}
			},
			data(){
				return {
					title_one: null,
					title_two: null,
					title: null,
				}
			}
		}
</script>
