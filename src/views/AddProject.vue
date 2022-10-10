<template>
    <h1>Add Project</h1>
    <form @submit.prevent="addProject">
    <label for="">Project Title</label>
    <input type="text" v-model="title">
    <p v-if="TitleerrorMsg" class="error">{{TitleerrorMsg}}</p>
    <label for="">Details</label>
    <input type="text" v-model="detail">
    <p v-if="DetailerrorMsg" class="error">{{DetailerrorMsg}}</p>
    <button>Add Project</button>
    </form>
</template  >

<script>
export default {
    data(){
        return{
            title : '',
            detail : '',
            TitleerrorMsg :'',
            DetailerrorMsg : ''
        }
    },
    methods: {
        addProject(){
            if(this.title.length < 1 ){
              this.TitleerrorMsg = "Title must Not be empty"
            }else{
              if(this.detail.length < 1){
                this.DetailerrorMsg = "Detail must not be empty"
              }else{
                fetch('http://localhost:3000/projects',{
                  method : 'POST',
                  headers : {
                      "Content-Type" : "application/json"
                  },
                  body : JSON.stringify({
                    title : this.title,
                    detail : this.detail,
                    complete : false
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
form {
    background: white;
    padding: 20px;
    border-radius: 10px;
  }
  label {
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0
  }
  input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
  }
  textarea {
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
  }
  form button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
  .error{
    color: crimson;
  }
</style>