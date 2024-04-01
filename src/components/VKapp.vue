<script>
import VK from 'vk-openapi';
import draggable from 'vuedraggable'
VK.init({ apiId: 51797715 })
export default {
    components: {
        draggable
    },
    data() {
        return {
            jsonobject: null,
            jsonobject2: [],
            show_friends: false,
            vk_button_seen: true,
            APP_ID: 51797715,
            VERSION: "5.131",
            alert_friends: ''
        }
    },
    methods: {
        auth() {
            VK.Auth.login((response) => {
                if (response.session) {
                    VK.Api.call('friends.get', { fields: 'photo_100', order: 'name', v: '5.131' }, (response) => {
                        this.jsonobject = response;
                        this.show_friends = true;
                        this.vk_button_seen = false;
                    })
                }
            })
        },
        pxp() {
            console.log(this.show_friends);
            console.log(this.jsonobject);
        },
        export_to_console() {
            for (let i = 0; i <= this.jsonobject2.length - 1; i++) {
                if (i < this.jsonobject2.length - 1) {
                    this.alert_friends = this.alert_friends + this.jsonobject2[i].first_name + ' ' + this.jsonobject2[i].last_name + ', ';
                }
                else {
                    this.alert_friends = this.alert_friends + this.jsonobject2[i].first_name + ' ' + this.jsonobject2[i].last_name + '.';
                }
            }
            alert('Ваши любимые друзья: ' + this.alert_friends);
            this.alert_friends = '';
        }
    }
};


</script>

<template>
    <div class="cont" id="all">
        <div class="conteiner_1">

            <div class="header_cont">
                <p class="header_text">FRIENDS IN VK</p>
                <!-- <button v-on:click="pxp">PXP</button> -->
            </div>


            <draggable class="middle_cont" :list="jsonobject.response.items" group="people" @change="log" itemKey="name"
                v-if="show_friends">
                <template #item="{ element }">
                    <div class="slot_for_friends">
                        <div class="foto_friends">
                            <img :src='element.photo_100'>
                        </div>
                        <div class="name_surname_friends">
                            <p class="name_friend">{{ element.first_name }}</p>
                            <p class="surname_friend">{{ element.last_name }}</p>
                        </div>
                    </div>
                </template>
            </draggable>

            <div class="btn_cont">

                <template v-if="vk_button_seen">
                    <button type="button" class="btn btn-primary" v-on:click="auth">
                        login in
                    </button>
                </template>
            </div>

        </div>
        <div class="conteiner_2">
            <div class="header_cont">
                <p class="header_text">CHOOSEN FRIENDS</p>
            </div>

            <draggable class="middle_cont" :list="jsonobject2" group="people" @change="log" itemKey="name"
                v-if="show_friends">

                <template #item="{ element }">
                    <div class="slot_for_friends">
                        <div class="foto_friends">
                            <img :src='element.photo_100'>
                        </div>
                        <div class="name_surname_friends">
                            <p class="name_friend">{{ element.first_name }}</p>
                            <p class="surname_friend">{{ element.last_name }}</p>
                        </div>
                    </div>
                </template>
            </draggable>

            <div class="btn_cont">
                <button type="button" class="btn btn-primary" v-on:click="export_to_console">export to console</button>


            </div>
        </div>
    </div>
</template>



<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@1,200;1,900&display=swap');

* {
    margin: 0;
    padding: 0;
}

p {
    margin-bottom: 0;
}

body {
    background: linear-gradient(#fc5400, #860000);
    height: 100%;
    width: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: center;
}

.cont {
    border-radius: 25px;
    width: 100vh;
    height: 97vh;
    margin: 10px;
    display: flex;
    justify-content: center;
    background: wheat;
}

.conteiner_1 {
    box-shadow: 0 0 3px rgba(0, 0, 0, 0.5);
    border-radius: 25px;
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: space-between;
    margin: 10px;
    color: #000;
    width: 100%;
    background: white;
}

.conteiner_2 {
    box-shadow: 0 0 3px rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: space-between;
    border-radius: 25px;
    margin: 10px;
    margin-left: 0px;
    color: #000;
    width: 100%;
    background: white;
}

.header_cont {
    background: rgb(255, 255, 255);
    border-radius: 25px;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    padding: 20px;
}

.header_text {
    font-family: 'Poppins', sans-serif;
    font-weight: 900;
    font-size: 20px;
    margin: 0px;
}

.middle_cont {
    background: rgb(246, 250, 251);
    height: 100%;
    width: 100%;
    overflow: auto;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}

.btn_cont {
    display: flex;
    justify-content: center;
    padding: 20px;
    font-family: 'Poppins', sans-serif;
}

.slot_for_friends {
    box-shadow: 0 0 3px rgba(0, 0, 0, 0.5);
    border-radius: 25px;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    margin: 10px;
    color: #000000;
    background: white;
    width: 90%;
}


.foto_friends {
    border-radius: 50%;
    overflow: hidden;
    width: 50px;
    height: 50px;
    margin: 5px;
    background: rgb(135, 40, 40);
}

.foto_friends img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.name_surname_friends {
    display: flex;
    align-items: flex-start;
    flex-direction: column;
    font-size: 1.25em;
}

.name_friend {
    margin: 5px;
}

.surname_friend {
    margin-bottom: 5px;
    margin-left: 5px;
    margin-right: 5px;
}
</style>
