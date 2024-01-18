<script setup>
import { ref } from 'vue'

const greeting = ref('Hello World!')


</script>

<template>
    <p class="greeting">SMS Application</p>

    <div id="app-div">


        <br />
        <label for="userId">User ID</label> <br />
        <input type="text" id="userId" style="width: 100%;" required/>
        <br />
        <br />
        <button type="button" style="width: 15%;" class="btn" @click="fetchRegId">Get ID</button>
        <br /><br />
        <label for="regId">Registration ID</label> <br />
        <input type="text" id="regId" style="width: 100%;" disabled/>
        <br />
        <label for="message">Message</label> <br />
        <input type="text" id="message" style="width: 100%;" disabled />
        <div id="sendMsg" >

                <button type="button" style="width: 15%;" class="btn " @click="sendMessage">Send</button>
            
            

                <button type="button" style="width: 15%;" class="btn " @click="getAllMessages">Get Messages</button>
              
        </div>
        
        <!-- <textarea rows=10 id="getMessages" style="width: 100%;" disabled >
           
   
        </textarea> -->
        <label for="getMessages" id="msgLabel">All Messages</label> <br />
        <div id="data">
            <ul>
      <li v-for="item in apiData" :key="item.regId" style="color: white;"><strong>Registration Id:</strong> {{item.regId}},<b> Message: </b>{{ item.message }}</li>
    </ul>
        </div>

    </div>
</template>

<script>

import axios from 'axios'

export default {
    data(){
        return {
            apiData:'',
        }
    },
    methods:{
        getAllMessages(){
            axios.get('http://localhost:8080/message/getMsgs')
         .then(response => {
                console.log(response.data)
                document.getElementById("msgLabel").style.display = "block"
                this.apiData = response.data
            })
         .catch(error => {
                console.log(error)
            })
        },
        fetchRegId(){
            let data=document.getElementById("userId").value
            if(data.length>0){

                axios.post('http://localhost:8080/register',{
                    "userId":data
                })
                .then(response => {
                    document.getElementById("regId").disabled = false
                    document.getElementById("regId").readonly = true
                    document.getElementById("regId").value = response.data.regId
                    document.getElementById("message").disabled = false
                    document.getElementById("sendMsg").style.display ='block'
                    document.getElementById("sendMsg").style.display ='flex'
                    document.getElementById("sendMsg").style.justifyContent ='space-between'


                })
                .catch(error => {
                    console.log(error)
                })
            }
            else{
                alert("Please Enter User ID")
            }
        },
        sendMessage(){
            // console.log(this.apiData)
            let regIds=document.getElementById("regId").value;
                    let message=document.getElementById("message").value 
                    if(regIds.length>0 && message.length>0){
            axios.post('http://localhost:8080/message/sendMsg', {
                'regId': regIds,
              'message': message
            })
            
         .then(response => {
                console.log(response.data)
              
                alert("Message Added Successfully")
            })
         .catch(error => {
                console.log(error)
            })
        }
        else{
            alert("Please Enter Message")
        }
        },
    }

}
</script>

<style>

#data{
    max-height: 200px;
    overflow: auto;
    background-color: rgb(92, 92, 92);
    color: white;
    border-radius: 10px;
}
#msgLabel{
    display: none;
}

#sendMsg {
   
    display: flex;
    justify-content: space-between;
    margin-top: 2%;
    margin-bottom: 2%;
    display: none;
    
}


.btn {
    --border-color: linear-gradient(-45deg, #ffae00, #7e03aa, #00fffb);
    --border-width: .125em;
    --curve-size: .5em;
    --blur: 30px;
    --bg: #080312;
    --color: #afffff;
    color: var(--color);
    /* use position: relative; so that BG is only for .btn */
    position: relative;
    isolation: isolate;
    display: inline-grid;
    place-content: center;
    padding: .5em 1.5em;
    font-size: 12px;
    border: 0;
    text-transform: uppercase;
    cursor: pointer;
    box-shadow: 10px 10px 20px rgba(0, 0, 0, .6);
    clip-path: polygon(
            /* Top-left */
            0% var(--curve-size),

            var(--curve-size) 0,
            /* top-right */
            100% 0,
            100% calc(100% - var(--curve-size)),

            /* bottom-right 1 */
            calc(100% - var(--curve-size)) 100%,
            /* bottom-right 2 */
            0 100%);
    transition: color 250ms;
}

.btn::after,
.btn::before {
    content: '';
    position: absolute;
    inset: 0;
}

.btn::before {
    background: var(--border-color);
    background-size: 300% 300%;
    animation: move-bg7234 5s ease infinite;
    z-index: -2;
}

@keyframes move-bg7234 {
    0% {
        background-position: 31% 0%
    }

    50% {
        background-position: 70% 100%
    }

    100% {
        background-position: 31% 0%
    }
}

.btn::after {
    background: var(--bg);
    z-index: -1;
    clip-path: polygon(
            /* Top-left */
            var(--border-width) calc(var(--curve-size) + var(--border-width) * .5),

            calc(var(--curve-size) + var(--border-width) * .5) var(--border-width),

            /* top-right */
            calc(100% - var(--border-width)) var(--border-width),

            calc(100% - var(--border-width)) calc(100% - calc(var(--curve-size) + var(--border-width) * .5)),

            /* bottom-right 1 */
            calc(100% - calc(var(--curve-size) + var(--border-width) * .5)) calc(100% - var(--border-width)),
            /* bottom-right 2 */
            var(--border-width) calc(100% - var(--border-width)));
    transition: clip-path 500ms;
}

.btn:where(:hover, :focus)::after {
    clip-path: polygon(
            /* Top-left */
            calc(100% - var(--border-width)) calc(100% - calc(var(--curve-size) + var(--border-width) * 0.5)),

            calc(100% - var(--border-width)) var(--border-width),

            /* top-right */
            calc(100% - var(--border-width)) var(--border-width),

            calc(100% - var(--border-width)) calc(100% - calc(var(--curve-size) + var(--border-width) * .5)),

            /* bottom-right 1 */
            calc(100% - calc(var(--curve-size) + var(--border-width) * .5)) calc(100% - var(--border-width)),

            /* bottom-right 2 */
            calc(100% - calc(var(--curve-size) + var(--border-width) * 0.5)) calc(100% - var(--border-width)));
    transition: 200ms;
}

.btn:where(:hover, :focus) {
    color: #fff;
}

textarea {
    resize: none;
    overflow: auto;
}

#app-div {
    width: 80% ;
    margin: 0 auto;
}

.greeting {
    font-size: 30px;
    font-weight: bold;
    text-align: center;
}
</style>