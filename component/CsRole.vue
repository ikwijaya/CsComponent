<template>
  <div>
    <v-toolbar flat>
      <v-list>
        <v-list-tile>
          <v-list-tile-title class="title">
            {{ header }}                  
          </v-list-tile-title>
        </v-list-tile>                  
      </v-list>
    </v-toolbar>
    <v-divider></v-divider>
    <v-list>
      <v-container>
        <v-layout row wrap v-if="users">
          <v-flex xs12>
            <v-select color="black" v-bind:items="users" v-model="v_selectedUser" label="Selected User" item-text="Name" item-value="NIK" autocomplete></v-select>
          </v-flex>
        </v-layout>
      </v-container>
    </v-list>
    <div>
      <v-data-table :headers="headers" :items="items" :search="v_filter" flat>
        <template slot="items" slot-scope="props">                 
          <td class="text-xs-left">{{ props.item.Form_Name }}</td>            
          <td class="text-xs-left"><v-switch slot="activator" v-model="props.item.isRead"></v-switch></td>
          <td class="text-xs-left"><v-switch slot="activator" :disabled="props.item.isReadOnly" v-model="props.item.isInsert"></v-switch></td>
          <td class="text-xs-left"><v-switch slot="activator" :disabled="props.item.isReadOnly" v-model="props.item.isUpdate"></v-switch></td>
          <td class="text-xs-left"><v-switch slot="activator" :disabled="props.item.isReadOnly" v-model="props.item.isDelete"></v-switch></td>                
        </template>
      </v-data-table>
      <br>           
      <v-tooltip bottom>
        <span>Save</span>                                   
        <v-btn color="grey" dark fab small slot="activator" @click="fn_close()" v-show="!isInsert">
          <v-icon>clear</v-icon></v-btn>
      </v-tooltip>              
      <v-tooltip bottom>
        <span>Save</span>                                   
        <v-btn color="blue lighten-1" dark fab small slot="activator" @click="open = false, fn_go()" v-show="isInsert">
          <v-icon>save</v-icon></v-btn>
      </v-tooltip>  
      <v-tooltip bottom>
        <span>Update</span>                                   
        <v-btn color="blue lighten-1" dark fab small slot="activator" @click="open = false, fn_go()" v-show="!isInsert">
          <v-icon>save</v-icon></v-btn>
      </v-tooltip>   
    </div>      
  </div>
</template>

<script>
export default {
  name: 'cs-role',
  props: {
    isInsert: true,
    id: null,
    header: null,
    users: null,
    items: null
  },
  data(){
    return {
      open: true,
      v_selectedUser: null,
      v_filter: null,
      headers: [
        { text: "Form", align: "left", sortable: true },
        { text: "Read", value: "isRead", align: "left", sortable: true },
        { text: "Insert", value: "isInsert", align: "left", sortable: true },
        { text: "Update", value: "isUpdate", align: "left", sortable: true },
        { text: "Delete", value: "isDelete", align: "left", sortable: true }
      ],
    }
  },
  methods: {
    fn_close(){
      this.$emit('close',false);
    },
    fn_go(){
      let rudi = [];
      let id;
      for (var i in this.items) {
        var el = this.items[i];
        if(el.isRead == true){
          el.isRead = 1
        }
        else if(el.isRead == false){
          el.isRead = 0
        }
        else if(el.isUpdate == true){
          el.isUpdate = 1
        }
        else if(el.isUpdate == false){
          el.isUpdate = 0
        }
        else if(el.isDelete == true){
          el.isDelete = 1
        }
        else if(el.isDelete == false){
          el.isDelete = 0
        }
        else if(el.isInsert == true){
          el.isInsert = 1
        }
        else if(el.isInsert == false){
          el.isInsert = 0
        }
        rudi.push({
          i: el.IDX_M_Forms,
          r: el.isRead,
          u: el.isUpdate,
          d: el.isDelete,
          n: el.isInsert
        });
      }
      
      if(this.isInsert){
        id = JSON.stringify({
          IDX_M_Users: this.v_selectedUser,
          UserAccess: rudi,
          Session_ID: this.$session.get('SESSION_ID')
        });
      }else{
        id = JSON.stringify({
          IDX_M_Users: this.id,
          UserAccess: rudi,
          Session_ID: this.$session.get('SESSION_ID')
        });
      }

      this.$emit('submit',id);
    }
  }
}
</script>

<style>

</style>
