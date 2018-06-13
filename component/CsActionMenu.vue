<template>
  <div>
    <v-menu bottom :left="left" :right="right">
      <v-btn icon slot="activator" flat><v-icon :color="color">more_vert</v-icon></v-btn>
      <v-list dense>
        <!-- for dynamic array then go -->
        <v-list-tile v-for="(k, v) in items" :key="v" @click="submit(k.code)" :to="(k.to != null) || (k.to != undefined) ? k.to + ($aid.isArray(idx) ? idx[1] : idx) : null">
          <v-list-tile-title><v-icon v-if="k.icon != null" :color="k.color">{{ k.icon }}</v-icon> {{ k.text }}</v-list-tile-title>
        </v-list-tile>

        <!-- Default Master -->
        <v-list-tile @click="submit('isUpdate')" v-if="isUpdate" :to="isUpdateTo != null ? isUpdateTo + this.id : null">
          <v-list-tile-title><v-icon small color="green" left>mode_edit</v-icon> Edit</v-list-tile-title>
        </v-list-tile>
        <v-list-tile @click="submit('isEnable')" v-if="isEnable != null">
          <v-list-tile-title v-if="isEnable"><v-icon small color="green">visibility</v-icon> Enable</v-list-tile-title>
          <v-list-tile-title v-if="!isEnable"><v-icon small color="red">visibility_off</v-icon> Disable</v-list-tile-title>
        </v-list-tile>
        <v-list-tile @click="submit('isDelete')" v-if="!skip ? isDelete && isEnable : isDelete">
          <v-list-tile-title><v-icon small color="black">delete</v-icon> Delete</v-list-tile-title>
        </v-list-tile>
      </v-list>
    </v-menu>
  </div>
</template>

<script>
export default {
  name: 'cs-action-menu',
  props: {
    id: null,
    items: null,
    color: null,
    left: true,
    right: false,
    divider: false,
    isUpdateTo: null,

    /** Default Master */
    isUpdate: false,
    isDelete: false,
    isEnable: false,
    isReadOnly: false,
    skip: false
  },
  watch: {
    id(){
      this.id;
    }
  },
  methods: {
    submit(code){
      this.$emit('submit',{id:this.id,code:code});
    }
  }
}
</script>

<style>

</style>
