<template>
  <div class="approval_flow">

    <!-- using v-bind:list instead of v-model because array passed as prop -->
    <draggable
      class="actual_approval_flow"
      v-bind:list="employees"
      v-if="employees.length > 0">

      <!--Need to have arrow and employee in the same div because of draggable -->
      <div
        class="flow_item"
        v-for="(employee, index) in employees"
        v-bind:key="`flow_member_${index}`">

        <arrow-right-icon
          class="arrow"
          v-if="index>0"/>

        <!-- the actual employee -->
        <div class="employee">
          <close-icon
            class="delete_button"
            v-on:click="$emit('deleteEmployee',index)"/>
          <div class="name">
            {{employee.properties.display_name
              || employee.properties.name_kanji
              || employee.properties.full_name}}
          </div>
          <div
            class="role"
            v-if="employee.properties.role">
            ({{employee.properties.role}})
          </div>
        </div>
      </div>

    </draggable>

    <p v-else class="error_message">承認者が選ばれていません　/　No recipient selected</p>

  </div>
</template>

<script>
// This component is used when creating an application

import draggable from 'vuedraggable'

export default {
  name: 'ApprovalFlow',
  components: {
    draggable
  },
  props: {
    employees: Array
  }
}
</script>

<style scoped>

.approval_flow_header {
  padding: 10px;
  border-bottom: 1px solid #dddddd;

}

.actual_approval_flow {
  display: flex;
  flex-wrap: wrap;
}

.flow_item{
  display: flex;
  align-items: center;
}

.arrow {
  font-size: 150%;
}
.employee{
  /* used to position delete button */
  position: relative;

  border: 1px solid #dddddd;
  border-radius: 5px;
  margin: 10px;
  padding: 15px;

  width: 8em;

  text-align: center;

  transition: border-color 0.25s;
  cursor: pointer;

}

.employee:hover{
  border-color: #444444;
}

.employee > *{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.delete_button {
  opacity: 0;
  position: absolute;
  font-size: 120%;
  top: 0;
  right: 0;
  padding: 2px;
  cursor: pointer;

  transition: color 0.25s, opacity 0.25s;
}

.employee:hover .delete_button{
  opacity: 1;
}

.delete_button:hover {
  color: #c00000;
}

</style>
