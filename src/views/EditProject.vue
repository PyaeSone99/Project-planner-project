<template>
    <h1>Edit Project</h1>
    <form @submit.prevent="addProject">
    <label for="">Project Title</label>
    <input type="text" v-model="title">
    <p v-if="errMsgTitle" class="error">{{errMsgTitle}}</p>
    <label for="">Details</label>
    <input type="text" v-model="detail">
    <p v-if="errMsgDetail" class="error">{{errMsgDetail}}</p>
    <button @click="updateProject">Update Project</button>
    </form>
</template>

<script>
export default {
    data(){
        return{
            id : this.$route.params.id,
            //props also available
            title : "",
            detail : "",
            errMsgTitle : '',
            errMsgDetail : ''
        }
    },
    mounted(){
        fetch('http://localhost:3000/projects'+'/'+this.id)
        .then((res)=>{
            return res.json()
        })
        .then((datas)=>{
            this.title = datas.title
            this.detail = datas.detail
        })
        .catch((err)=>{
            console.log(err);
        })
    },
    methods:{
        updateProject(){
            if(this.title.length < 1 ){
                this.errMsgTitle = "Title must not be empty"
            }else{
                if(this.detail.length < 1){
                    this.errMsgDetail = "Detail must not be empty"
                }else{
                    fetch('http://localhost:3000/projects'+'/'+this.id,{
                        method : "PATCH",
                        headers : {
                            "Content-type" : "application/json"
                        },
                        body : JSON.stringify({
                            title : this.title,
                            detail : this.detail
                        })
                    })
                    .then(()=>{
                        this.$router.push({name : 'home'})
                    })
                    .catch((err)=>{
                        console.log(err);
                    })
                }
            }
        }
    }
}
</script>

<style>

</style>

