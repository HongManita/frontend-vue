<template>
  <v-container>
    <v-row>
        <v-col cols="12">
            <Table/>
        </v-col>
    </v-row>

  </v-container>
</template>

<script>
export default {
    components:{
        Table:() => import('../components/table.vue')
    }
}
</script>

<style>

</style>