<template>
  <div class="Kanban">
    <div
      class="Column"
      :style="{ backgroundColor: column.color }"
      v-for="(column, index) in data"
      :key="index"
    >
      <div class="Column-header">
        {{ column.name }}
      </div>
      <div class="Column-body">
        <div v-for="(task, task_index) in column.tasks" :key="task_index">
          <div
            class="task"
            :draggable="true"
            @dragstart="start_move(task_index, index)"
          >
            {{ task.task_name }}
          </div>
          <div
            class="drop_zone"
            @dragenter.prevent="drop_zone_enter"
            @dragleave.prevent="drop_zone_leave"
            @dragover.prevent
          ></div>
        </div>

        <div class="Create-task" @click="create_task(index)">Create Task</div>
      </div>
    </div>
    <b-modal ref="create-task-modal" title="Create Task">
      <input
        class="input-task-name"
        v-model="task_name"
        @keyup.13="submit_create_task"
      />
    </b-modal>
  </div>
</template>

<script>
export default {
  props: {
    data: Array,
    create_task_submit: Function,
  },
  methods: {
    create_task(index_column) {
      console.log(index_column);
      this.current_column_index = index_column;
      this.$refs["create-task-modal"].show();
    },
    submit_create_task() {
      this.create_task_submit(this.current_column_index, {
        task_name: this.task_name,
      });
    },
    start_move(task_index, column_index) {
      this.current_column_index = column_index;
      this.current_column_index = task_index;
    },
    drop_zone_enter(event) {
      event.target.style.height = "100px";
      event.target.style.borderStyle = "dotted";
      event.target.style.transition = "height 0.5s";
    },
    drop_zone_leave() {
      event.target.style.height = "10px";
      event.target.style.borderStyle = "none";
      event.target.style.transition = "height 0.5s";
    },
  },
  data() {
    return {
      task_name: "",
      current_column_index: "",
      current_task_index: "",
    };
  },
};
</script>

<style>
.Kanban {
  width: 100%;
  height: 100%;
  background-color: aquamarine;
}
.Column {
  height: 600px;
  width: 300px;
  border-radius: 10px;
  display: inline-block;
  margin: 25px;
  -webkit-box-shadow: 9px 10px 6px -2px rgba(0, 0, 0, 0.42);
  -moz-box-shadow: 9px 10px 6px -2px rgba(0, 0, 0, 0.42);
  box-shadow: 9px 10px 6px -2px rgba(0, 0, 0, 0.42);
  padding: 15px;
}
.Column-header {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  font-size: 32px;
  font-weight: bold;
}
.Column-body {
  height: calc(100% - 50px);
  border-radius: 10px;
  padding: 5px;
  background-color: rgba(255, 255, 255, 0.678);
}
.Create-task {
  width: 100%;
  height: auto;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
}
.Create-task:hover {
  background-color: rgba(53, 35, 44, 0.411);
}
.input-task-name {
  width: 100%;
}
.task {
  position: relative;
  width: auto;
  height: 100px;
  border-radius: 2px;
  margin: 10px;
  background-color: rgba(208, 241, 131, 0.74);
}
.drop_zone {
  height: 10px;
}
</style>