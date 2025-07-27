<script setup lang="ts"></script>

<template>
  <div class="container">
    <!-- 顶部 -->
    <div class="header flex-ct-x">
      <img src="@/assets/logo.svg" alt="Logo" class="logo" />
      <span class="title-text text-blue">Todo</span>
      <span class="title-text text-purple">List</span>
    </div>

    <!-- 输入框和按钮 -->
    <div class="input-container flex-fs">
      <input type="text" placeholder="请输入待办事项" class="input-box" />
      <button class="add-button flex-ct-x">
        <span class="btn-text">Criar</span>
        <img src="@/assets/plus.svg" class="btn-icon" alt="Add Icon" />
      </button>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100vh;
  background: var(--Gray-600);
}

.container .header {
  height: 200px;
  background: var(--Gray-700);
}

.header .logo {
  width: 22px;
  height: 36px;
}

.header .title-text {
  font-size: 40px;
  font-weight: 900;
}

.header .text-blue {
  color: var(--Blue);
  margin-left: 20px;
}
.header .text-purple {
  color: var(--Purple-Dark);
}

.input-container {
  width: 736px;

  margin: 0 auto;
  transform: translateY(-50%);
}

.input-container .input-box {
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

.input-container .input-box::placeholder {
  color: var(--Gray-300);
}

.input-container .input-box:focus {
  outline: none;
  border-color: var(--Purple-Dark);
}

.input-container .add-button {
  padding: 16px;
  border-radius: 8px;
  background: var(--Blue-Dark);
  border: none;

  cursor: pointer;
}

.input-container .add-button:hover {
  background: var(--Blue);
}

.add-button .btn-text {
  font-size: 14px;
  color: var(--Gray-100);
  font-weight: 700;
  line-height: 140%;
  margin-right: 8px;
}

.add-button .btn-icon {
  width: 16px;
  height: 16px;
}
</style>
