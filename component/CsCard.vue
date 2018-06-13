<template>
  <div>
    <v-layout column>
      <v-container fluid>
        <v-layout row wrap>
          <v-flex v-bind="{ [`xs${card.flex}`]: true }" v-for="card in items" :key="card.label">
            <v-card :to="card.href" height="100px" flat>
              <v-container fluid>
                <v-layout row>
                  <v-flex xs12>
                    <v-layout column>
                      <v-flex xs6>
                        <v-icon v-if="card.icon" large :color="card.color != null ? card.color : color" class="mt-3">{{ card.icon != null ? card.icon : v_icon }}</v-icon> 
                        <img v-if="card.src" :src="card.src" width="46" height="46" />
                        <!-- <span v-else>{{ card.value }}</span> -->
                      </v-flex>
                      <v-flex xs6>
                        <span class="body-2">{{ card.label }}</span>
                      </v-flex>
                    </v-layout>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-layout>
  </div>
</template>

<script>
/**
 * {
 *    flex,
 *    label,
 *    value,
 *    href,
 *    color,
 *    icon
 * }
 */
    export default {
        name: 'cs-card',
        props: {
          items: null,
          color: null
        },
        data(){
          return {
            v_urlImg: 'https://apps.alam-sutera.com/SharedAsset/general/type.png',
            v_icon: 'info',
          }
        },
    }    
</script>
