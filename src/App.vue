<script setup lang="ts">
import { ref,watch } from "vue";

// 定义待办事项类型
interface Task {
  content: string;
  completed: boolean;
}

// 定义待办事项列表
const tasks = ref<Task[]>([]);

// 添加待办事项的输入框
const newTaskContent = ref<string>("");

// 已完成的待办事项数量
const completedCount = ref<number>(0);

// 添加待办事项同时添加到本地存储
const saveTasksToLocalStorage = () => {
  completedCount.value = tasks.value.filter(task => task.completed).length;
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

// 从本地存储加载待办事项
const loadTasksFromLocalStorage = () => {
  const storedTasks = localStorage.getItem("tasks");
  if (storedTasks) {
    tasks.value = JSON.parse(storedTasks);
    // 计算已完成的待办事项数量
    completedCount.value = tasks.value.filter(task => task.completed).length;
  } else {
    tasks.value = []; // 如果没有存储的任务，则初始化为空数组
  }
};


// 添加待办事项
const addTask = () => {
  if (newTaskContent.value.trim() === "") return; // 如果输入为空则不添加
  const newTask: Task = {
    content: newTaskContent.value.trim(),
    completed: false,
  };
  tasks.value.push(newTask);
  newTaskContent.value = ""; // 清空输入框
  saveTasksToLocalStorage(); // 保存到本地存储
};

// 删除待办事项
const deleteTask = (index: number) => {
  tasks.value.splice(index, 1);
  saveTasksToLocalStorage(); // 更新本地存储
};

// 监听任务列表的变化，保存到本地存储
watch(tasks, saveTasksToLocalStorage, { deep: true });

// 在组件挂载时加载待办事项
loadTasksFromLocalStorage();

</script>
<template>
  <div class="container">
    <!-- 顶部 -->
    <div class="header flex-ct-x">
      <img src="@/assets/github.svg" alt="Logo" class="logo" />
      <span class="title-text text-blue">Todo</span>
      <span class="title-text text-purple">List</span>
    </div>

    <!-- 输入框和按钮 -->
    <div class="input-container flex-fs">
      <input
        type="text"
        placeholder="请输入待办事项"
        class="input-box"
        maxlength="42"
        v-model="newTaskContent"
         @keyup.enter="addTask"
      />
      <button class="add-button flex-ct-x" @click="addTask">
        <!-- <span class="btn-text">添加</span> -->
        <img src="@/assets/plus.svg" class="btn-icon" alt="Add Icon" />
      </button>
    </div>

    <!-- takes -->
    <div class="takes-container">
      <div class="takes-titles flex-fs">
        <div class="titles-box flex-ct-x">
          <span class="titles-text color-blue">待办事项</span>
          <div class="titles-nums flex-ct-x">{{ tasks.length }}</div>
        </div>
        <div class="titles-box flex-ct-x">
          <span class="titles-text color-purple">总计</span>
          <div class="titles-nums flex-ct-x">{{ completedCount }} de {{ tasks.length }}</div>
        </div>
      </div>

      <div class="empty-list flex-ct-y" v-if="tasks.length === 0">
        <img src="@/assets/empty.png" alt="Empty List" class="empty-icon" />
        <span class="empty-text empty-blod">还没有待办事项</span>
        <span class="empty-text">快添加第一个待办事项吧</span>
      </div>

      <div class="tasks-list" v-else>
        <div
          class="task"
          v-for="(task, index) in tasks"
          :key="index"
          :class="{ 'task-checked': task.completed }"
        >
          <div class="option flex-ct-x checkbox">
            <input type="checkbox" v-model="task.completed" class="check" />
          </div>
          <span class="task-text">{{ task.content }}</span>
          <div class="option flex-ct-x delete" @click="deleteTask(index)"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.container {
  width: 100%;
  height: 100vh;
  background: var(--Gray-600);

  .header {
    height: 200px;
    background: var(--Gray-700);
    .logo {
      width: 44px;
      height: 44px;
    }
    .title-text {
      font-size: 40px;
      font-weight: 900;
    }
    .text-blue {
      color: var(--Blue);
      margin-left: 20px;
    }
    .text-purple {
      color: var(--Purple-Dark);
    }
  }

  .input-container {
    width: 736px;

    margin: 0 auto;
    transform: translateY(-50%);

    .input-box {
      flex: 1 0 0;
      margin-right: 8px;
      padding: 16px;
      border-radius: 8px;
      border: 1px solid var(--Gray-700);
      background: var(--Gray-500);

      font-size: 16px;
      line-height: 140%;
      color: var(--Gray-100);
      font-weight: 400;

      cursor: pointer;
    }
    .input-box::placeholder {
      color: var(--Gray-300);
    }
    .input-box:focus {
      outline: none;
      border-color: var(--Purple-Dark);
    }
    .add-button {
      padding: 16px 30px;
      border-radius: 8px;
      background: var(--Blue-Dark);
      border: none;

      cursor: pointer;
      .btn-text {
        font-size: 14px;
        color: var(--Gray-100);
        font-weight: 700;
        line-height: 140%;
        margin-right: 8px;
      }
      .btn-icon {
        width: 16px;
        height: 16px;
      }
    }
    .add-button:hover {
      background: var(--Blue);
    }
  }

  .takes-container {
    width: 736px;
    margin: 0 auto;
    margin-top: 64px;

    .takes-titles {
      margin-bottom: 24px;
      .titles-text {
        font-weight: 700;
        margin-right: 8px;
      }
      .color-blue {
        color: var(--Blue, #4ea8de);
      }
      .color-purple {
        color: var(--Purple, #8284fa);
      }

      .titles-nums {
        padding: 4px 8px;
        border-radius: 999px;
        background: var(--Gray-400, #333);

        color: var(--Gray-200, #d9d9d9);
        font-size: 12px;
        font-weight: 700;
      }
    }

    .empty-list {
      padding: 64px 24px;
      border-radius: 8px;
      border-top: 1px solid var(--Grey-400, #333);
      .empty-icon {
        width: 56px;
        height: 56px;
        margin-bottom: 16px;
      }
      .empty-text {
        color: var(--Gray-300, #808080);
        font-size: 16px;
        font-weight: 400;
        line-height: 140%;
      }
      .empty-blod {
        font-weight: 700;
      }
    }

    .tasks-list {
      max-height: 360px;
      // 隐藏滚动条
      overflow-y: auto;
      &::-webkit-scrollbar {
        display: none;
      }

      &::-webkit-scrollbar-thumb {
        background: var(--Gray-400, #333);
        border-radius: 8px;
      }
      &::-webkit-scrollbar-track {
        background: transparent;
      }
      &::-webkit-scrollbar-corner {
        background: transparent;
      }
      &::-webkit-scrollbar-button {
        display: none;
      }

      .task {
        display: flex;
        align-items: center;
        padding: 16px;
        border-radius: 8px;
        border: 1px solid var(--Gray-400, #333);
        background: var(--Gray-500, #262626);
        box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.06);
        margin-bottom: 12px;
        transition: all 0.2s;
        &:last-child {
          margin-bottom: 0;
        }

        .task-text {
          color: var(--Gray-100, #f2f2f2);
          font-weight: 400;
          // line-height: 140%;
          flex: 1 0 0;
          width: 0;
          margin: 0 12px;
        }
        .option {
          width: 24px;
          height: 24px;
        }
        .delete {
          border-radius: 4px;
          background-image: url("@/assets/delete.svg");
          background-repeat: no-repeat;
          background-position: center center;
          cursor: pointer;
          transition: 0.2s all;
        }
        .delete:hover {
          background-color: var(--Gray-400);
          background-image: url("@/assets/delete-hover.svg");
        }

        .checkbox {
          .check {
            width: 18px;
            height: 18px;
            cursor: pointer;
            appearance: none;
            background: transparent;
            border: none;
            background-image: url("@/assets/unchecked.svg");
            background-repeat: no-repeat;
            background-position: center center;
            transition: 0.2s all;
            &:checked {
              background-image: url("@/assets/checked.svg");
            }
            &:hover {
              background-image: url("@/assets/unchecked-hover.svg");
            }
            &:checked:hover {
              background-image: url("@/assets/checked-hover.svg");
            }
          }
        }
      }
      .task-checked {
        border-color: var(--Gray-500);
        .task-text {
          text-decoration: line-through;
          color: var(--Gray-300, #808080);
        }
      }
    }
  }
}
</style>
