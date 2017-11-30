<template>
  <div class="intro">
    <h4>demo简单说明</h4>
    <ul>
      <li>可新增事项</li>
      <li>待办用红色表示，完成用灰色表示</li>
      <li>可分页显示</li>
      <li>双击可编辑待办事项</li>
    </ul>
  </div>
</template>

<style scoped>
 .intro{
   background: rgba(255,255,255,0.3);
   border: 1px solid #ccc;
   padding: 10px;
   border-radius: 10px;
 }

 ul{
   margin: 0;
   padding: 0;
 }

 li{
   font-size: 10px;
 }
</style>

<script>
export default {
  name: 'Introduction'
}
</script>


