<template>
  <div>
    <p>Task List</p>
    <table class="tasks">
      <tr class="tasks-header">
        <th class="task-h-switch"></th>
        <th class="task-h-state">状態</th>
        <th class="task-h-priority">優先度</th>
        <th class="task-h-name">名前</th>
        <th class="task-h-remove"></th>
      </tr>
      <tr
        class="task"
        v-for="task in sortedTaskByRevPriority"
        :key="task.id"
        :class="{ todo: task.stateId === 0, done: task.stateId === 1 }"
      >
        <td class="switch">
          <button @click="switchTaskState(task.id)">
            {{ getSwitchStateLabel(task.stateId) }}
          </button>
        </td>
        <td>
          <div class="state">{{ getStateById(task.stateId).name }}</div>
        </td>
        <td>
          <div class="priority">
            {{ getPriorityByVal(task.priorityId).name }}
          </div>
        </td>
        <td>
          <div class="name">{{ task.name }}</div>
        </td>
        <td class="remove">
          <button @click="deleteTask(task.id)">削除</button>
        </td>
      </tr>
    </table>
  </div>
  <div class="new-task">
    <label>
      タスク名:
      <input v-model="newTaskName" />
    </label>
    <br />
    <label>
      優先度:
      <select v-model="newTaskPriorityId">
        <option
          v-for="priority in priorities"
          v-bind:value="priority.val"
          v-bind:key="priority.val"
        >
          優先度{{ priority.val }}: {{ priority.name }}
        </option>
      </select>
    </label>
    <button @click="addTask" class="add-button">追加</button>
  </div>
</template>

<script>
import { computed, ref } from "vue";

export default {
  setup() {
    const tasks = ref([]);
    const nextId = ref(0);
    const newTaskName = ref("");
    const newTaskPriorityId = ref(0);
    const states = [
      { id: 0, name: "TODO" },
      { id: 1, name: "DONE" },
    ];
    const priorities = [
      { val: 0, name: "NONE" },
      { val: 1, name: "LOW" },
      { val: 2, name: "MIDDLE" },
      { val: 3, name: "HIGH" },
      { val: 4, name: "URGENT" },
    ];

    const getStateById = (id) => {
      return states.find((elm) => elm.id === id);
    };

    const getPriorityByVal = (val) => {
      return priorities.find((elm) => elm.val === val);
    };

    const clearNewTask = () => {
      newTaskName.value = "";
      newTaskPriorityId.value = 0;
    };

    const getSwitchStateLabel = (state) => {
      if (state === 0) {
        return "完了";
      } else {
        return "戻す";
      }
    };

    const getTaskIndexById = (id) => {
      return tasks.value.findIndex((elm) => elm.id === id);
    };

    const sortedTaskByRevPriority = computed(() =>
      tasks.value.sort((a, b) => -(a.priorityId - b.priorityId))
    );

    const addTask = () => {
      if (newTaskName.value === "") {
        alert("タスク名を入力してください");
        return;
      }

      let newTask = {
        id: nextId.value,
        name: newTaskName.value,
        stateId: 0,
        priorityId: newTaskPriorityId.value,
      };
      nextId.value++;
      tasks.value.push(newTask);
      clearNewTask();
    };

    const deleteTask = (id) => {
      const i = getTaskIndexById(id);
      if (i === -1) {
        return;
      }

      tasks.value.splice(i, 1);
    };

    const switchTaskState = (id) => {
      const i = getTaskIndexById(id);
      if (i === -1) {
        return;
      }

      const task = tasks.value[i];
      if (task.stateId === 0) {
        task.stateId = 1;
      } else {
        task.stateId = 0;
      }
    };

    return {
      sortedTaskByRevPriority,
      newTaskName,
      getStateById,
      getPriorityByVal,
      newTaskPriorityId,
      priorities,
      addTask,
      deleteTask,
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
  border-collapse: collapse;
}
.tasks td,
.tasks th {
  border: 1px solid #333;
}

.todo td{
  background-color: cornsilk;
}
.done td{
  background-color: gray;
}

.name {
  margin-left: 10px;
  margin-right: 10px;
}
.state {
  margin-left: 10px;
  margin-right: 10px;
}

th.task-h-remove,
th.task-h-switch {
  min-width: 3em;
  border: 0;
}
.task-h-name {
  min-width: 6em;
}
.task-h-state {
  min-width: 5em;
}
.task-h-priority {
  min-width: 7em;
}

td.switch,
td.remove {
  border: 0;
  background-color: unset;
}


.new-task {
  margin-top: 20px;
}

.add-button {
  margin-left: 10px;
}
</style>
