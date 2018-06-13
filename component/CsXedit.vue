<template>
    <div>
        <div v-if="!v_edit">
          <div v-if="(type == 'textfield') || (type == 'select')">
            <v-layout row wrap>
              <v-flex xs12>
                <span v-if="isUpdate">{{ value }}</span>    
                <span v-else>{{ value }}</span>
                <v-btn v-if="isUpdate" flat icon color="grey" @click="fn_enable"><v-icon small>edit</v-icon></v-btn>
              </v-flex>
            </v-layout>  
          </div>
        </div>
        <div v-if="v_edit">
          <div v-if="type == 'textfield'">
            <v-container fluid>
              <v-layout row>
                <v-flex xs8>
                  <v-text-field v-on:keyup.esc="fn_disable" v-focus="v_focus" @focus="v_focus = true" @blur="v_focus = false" v-model="vm_tempVal" />
                </v-flex>
                <v-flex xs4>
                  <v-layout row wrap>
                    <v-flex xs6>
                    <v-btn @click="fn_do" small fab dark>
                      <v-icon>save</v-icon>
                    </v-btn>
                    </v-flex>
                    <v-flex xs6>
                    <v-btn @click="fn_disable" small light fab>
                      <v-icon>clear</v-icon>
                    </v-btn>    
                    </v-flex>
                  </v-layout>
                </v-flex>
              </v-layout>
            </v-container>
          </div>
          <div v-if="type == 'select'">
            <v-select
              v-on:keyup.esc="fn_disable"
              :items="items"
              v-model="vm_tempVal"
              label="Choose..."
              prepend-icon="lists"
              :item-text="keyText"
              :item-value="keyValue"
              autocomplete
            ></v-select>
            <v-btn @click="fn_do" small color="success">save</v-btn>
            <v-btn @click="fn_disable" small color="error">cancel</v-btn>
          </div>
        </div>
    </div>
</template>

<script>
import { VueFocus } from 'vue-focus';
export default {
    name: 'cs-xedit',
    directives: {
        focus: VueFocus
    },
    props: {
        type: String,
        isUpdate: {
            type: [Boolean,Number],
            default(){
                return false
            }
        },
        id: [String,Object,Number],
        value: [String,Object,Number],
        code: [String,Object,Number],

        /** v-select */
        items: [Array,Object],
        keyText: [String,Object],
        keyValue: [String,Object]
    }, 
    data(){
        return {
            v_edit: false,
            v_focus: false,
            vm_tempVal: null,
            w_value: null,
        }   
    },
    watch: {
        w_value(){
            this.vm_tempVal = this.w_value;
            this.fn_do();
        }
    },
    methods: {
        fn_enable(){
            this.vm_tempVal = this.value
            this.v_edit = true
        },
        fn_disable(){
            this.vm_tempVal = null
            this.v_edit = false
        },
        fn_do(){
            this.$emit('send-xedit',[
                this.id,
                (this.value != this.vm_tempVal) ? this.vm_tempVal : this.value,
                this.code
            ]);
            this.fn_disable();
        }
    }
}
</script>

<style>
    .edit-text {
        border-bottom: 1px blue dashed;
        text-decoration: none;
    }
</style>
