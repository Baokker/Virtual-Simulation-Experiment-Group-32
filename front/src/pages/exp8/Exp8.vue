<!-- 8.软件项目/产品的财务评价实验
包括: 净现值法、IRR 法、投资回收期法等 -->
<template>
  <div style="padding: 2%">
    <a-back-top />
    <div type="flex" justify-content="center" style="height: 50px">
      <div style="float: left">
        <h1 class="title">{{ $route.meta.title }}</h1>
      </div>
      <div style="float: right">
        <a-button style="margin-right: 20px" type="primary" shape="round">
          <template #icon> <DownloadOutlined /> </template>实验指导书下载
        </a-button>
        <a-button class="button2" type="primary" shape="round">
          <template #icon> <DownloadOutlined /> </template>实验报告模板下载
        </a-button>
      </div>
    </div>

    <hr />
    <RouterView />
  </div>
</template>

<script lang="ts">
  export default {
    name: 'Exp8',
  };
</script>

<style scoped lang="less"></style>
