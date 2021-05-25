<template>
  <div>
    <table class="tasks">
      <tr class="tasks-title">
        <th></th>
        <th>状態</th>
        <th>名前</th>
      </tr>
      <tr class="task" v-for="task in tasks" :key="task.id">
        <td>
          <button @click="switchTaskState(task.id)">
            {{ getSwitchStateLabel(task.state) }}
          </button>
        </td>
        <td>
          <div class="state">{{ getStateName(task.state) }}</div>
        </td>
        <td>
          <div class="name">{{ task.name }}</div>
        </td>
      </tr>
    </table>
  </div>
  <div>
    <label>
      タスク名
      <input v-model="newTaskName" />
    </label>
    <button @click="addTask">追加</button>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const tasks = ref([]);
    const nextId = ref(0);
    const newTaskName = ref("");
    const StatesName = ["TODO", "DONE"];

    const clearNewTask = () => {
      newTaskName.value = "";
    };

    const getStateName = (n) => {
      return StatesName[n];
    };

    const getSwitchStateLabel = (state) => {
      if (state == 0) {
        return "完了";
      } else {
        return "戻す";
      }
    };

    const addTask = () => {
      if (newTaskName.value == "") {
        alert("タスク名を入力してください");
        return;
      }
      const newTask = {
        id: nextId.value,
        name: newTaskName.value,
        state: 0,
      };
      nextId.value++;
      tasks.value.push(newTask);
      clearNewTask();
    };

    const switchTaskState = (id) => {
      const i = tasks.value.findIndex((elm) => elm.id == id);
      if (i == -1) {
        return;
      }

      const task = tasks.value[i]
      if (task.state == 0) {
        task.state = 1;
      } else {
        task.state = 0;
      }
    };

    return {
      tasks,
      newTaskName,
      addTask,
      getStateName,
      switchTaskState,
      getSwitchStateLabel,
    };
  },
};
</script>

<style>
.tasks {
  margin-left: auto;
  margin-right: auto;
}
.tasks,
.tasks td,
.tasks th {
  border-collapse: collapse;
  border: 1px solid #333;
}

.name {
  margin-left: 10px;
  margin-right: 10px;
}
.state {
  margin-left: 10px;
  margin-right: 10px;
}
</style>
