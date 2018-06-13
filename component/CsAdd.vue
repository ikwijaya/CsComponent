<template>
  <div v-if="(items.length > 0) || (items == null)">
    <v-toolbar flat>
        <v-list>
            <v-list-tile>
                <v-list-tile-title class="title">{{ header }}</v-list-tile-title>
            </v-list-tile>
        </v-list>
    </v-toolbar>
    <v-divider></v-divider>
    <v-container grid-list-md>
      <v-list v-for="(k,v) in items" :key="v" dense>
          <v-select 
            v-if="(k.type == 'select') && (k.show == null ? true : k.show)" 
            :label="k.text" 
            v-model="k.value"
            :items="k.data" 
            :color="k.color == null ? 'black' : k.color"
            :item-text="k.itext" 
            :item-value="k.ivalue" 
            :multiple="k.multi" 
            :prepend-icon="k.icon"
            :rules="k.rules"
            :disabled="k.disabled"
            :required="k.required"
            chips 
            deletable-chips 
            clearable 
            autocomplete 
            ></v-select>
          <v-text-field 
            v-if="(k.type == 'textfield') && (k.show == null ? true : k.show)" 
            :type="k.vtype"
            :label="k.text" 
            v-model="k.value" 
            :multi-line="k.multi" 
            :color="k.color == null ? 'black' : k.color"
            :prepend-icon="k.icon"
            :textarea="k.textarea"
            :rules="k.rules"
            :disabled="k.disabled"
            :required="k.required"
            ></v-text-field>
          <v-switch 
            v-if="(k.type == 'switch') && (k.show == null ? true : k.show)" 
            :label="k.Text"
            v-model="k.value"
            :value="k.value"
            :prepend-icon="k.icon"
            :rules="k.rules"
            :disabled="k.disabled"
            :required="k.required"
            :color="k.color == null ? 'black' : k.color"></v-switch>
          <v-checkbox v-if="(k.type == 'checkbox') && (k.show == null ? true : k.show)"  
            :label="k.text" 
            v-model="k.value" 
            :rules="k.rules"
            :disabled="k.disabled"
            :required="k.required"
            :value="k.value"></v-checkbox>
          <p v-if="(k.type == 'info')" class="text-xs-left">{{ k.text }} {{ k.info }}</p>
      </v-list>
    </v-container>
    <span>
      <v-btn fab small dark color="grey" @click.stop="close">
        <v-icon small>clear</v-icon> 
      </v-btn>
      <v-btn fab small dark color="blue" @click="send">
        <v-icon small>save</v-icon>  
      </v-btn>
    </span>
  </div>
</template>
<script>
  /** 
     @description
      Dynamic create search by array javascript or JSON, will use $emit event to parent. Then supported type is :
      - select  => <v-select>
      - input   => <v-text-field>
      - switch  => <v-switch>
      - span    => <span>

    @example :
      <search header="Header Title" :items="array|object" @search="fn" @clear="fn" @toggle="fn" />
    
    @prop untuk items is Array :
    - type    => select | textfield | switch
    - show    => for show to data
    - vtype   => only textfield: text|number|date
    - text    => label
    - vmodel  => vmodel name
    - value   => value dipake buat ngisi data via vmodel
    - data    => only select
    - color   => color
    - itext   => only select
    - ivalue  => only select
    - multi   => bool multiline|multiple,
    - required => true | false,
    - rules   => object
    - disabled => true | false,
    - required => true | false
  */
  export default {
    name: 'cs-add',
    props: {
      header: [String,Object],
      items: null
    },
    data() {
      return {

      }
    },
    methods: {
      send(k){
        let out = {}, key, val
        for(let i in this.items){
          key = this.items[i].vmodel;
          val = Array.isArray(this.items[i].value) ? this.items[i].value.toString() : this.items[i].value;
          out[key] = val;
        }
        this.$emit('save',out);

        for(let i in this.items){
          if(this.items[i].type == 'textfield'){
            this.items[i].value = null;
          }else if(this.items[i].type == 'select'){
            this.items[i].value = [];
          }else{
            this.items[i].value = false;
          }
        }
      },
      close(){
        for(let i in this.items){
          if(this.items[i].type == 'textfield'){
            this.items[i].value = null;
          }else if(this.items[i].type == 'select'){
            this.items[i].value = [];
          }else{
            this.items[i].value = false;
          }
        }
        this.$emit('close');
      },
    },
  }
</script>

