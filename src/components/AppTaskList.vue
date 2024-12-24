<template>
    <div 
        class="task"
        v-for="task in tasks" 
        :key="task.id"
        @remove="$emit('remove', task)"> 
           <div>
                <div><strong>Название: </strong>{{ task.title }}</div>
                <div><strong>Описание: </strong>{{ task.body }}</div>
           </div>
           <div>
                <button
                    class="btn"
                    @click="$emit('done', task)">
                Выполнено!</button>
                <button
                    class="btn"
                    @click="$emit('edit', task)">
                Редактировать</button>
                <button
                    class="btn"
                    @click="$emit('remove', task)">
                Удалить</button>
           </div>
            
    </div> 
</template>

<script>
    export default {
        props: {
            tasks: {
                type: Array,
                required: true,
            }
        },
    }
</script>

<style scoped>
.task{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 66%;
    margin-left: 10px;
    padding: 15px;
    border: 1px solid black;
    margin-top: 10px;
    border-radius: 5px;
    margin-bottom: 10px;
}
.btn{
    align-self: center;
    background-color: #3963FA;
    border: 0;
    color: white;
    border-radius: 5px;
    height: 40px;
    margin-right: 5px;
}
.circle {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: #3963FA;
}
</style>