<script setup>

    import {ref} from 'vue'
    import axios from 'axios'

    const link = ref('')
    const shortenedLink = ref('')
    const loading = ref(false)
    const token = '7ed9b44598093c5a723c5907d662e54376fedcd0' //YOUR TOKEN //my token => 7ed9b44598093c5a723c5907d662e54376fedcd0
    const guid = 'Bn7r266WM7y' //GROUP_GUID // my guid => Bn7r266WM7y

    async function validURL() {
        loading.value = true
        var pattern = new RegExp('^(https?:\\/\\/)?'+ // protocol
            '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|'+ // domain name
            '((\\d{1,3}\\.){3}\\d{1,3}))'+ // OR ip (v4) address
            '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*'+ // port and path
            '(\\?[;&a-z\\d%_.~+=-]*)?'+ // query string
            '(\\#[-a-z\\d_]*)?$','i'); // fragment locator
        const valid = !!pattern.test(link.value);
        if(valid) {

            const url = 'https://api-ssl.bitly.com/v4/shorten'
            const data = {
                "group_guid": `${guid}`,
                "domain": "bit.ly",
                "long_url": link.value
            }  
            const config = {
                headers: {
                    Authorization: `Bearer ${token}`,
                    'Content-Type': "application/json"
                }
            }

            await axios.post(url, data, config)
            .then(result => {
                shortenedLink.value = result.data.link
                loading.value = false
            })
            .catch(err => {
                shortenedLink.value = err.message
                loading.value = false
            })

            // const result = fetch('https://api-ssl.bitly.com/v4/shorten', {
            //     method: 'POST',
            //     headers: {
            //         'Authorization': `Bearer ${token}`,
            //         'Content-Type': 'application/json'
            //     },
            //     body: JSON.stringify({ "long_url": "https://dev.bitly.com", "domain": "bit.ly", "group_guid": "o_4sh2f1psh2" })
            // });
            // console.log(result)
        }
    }

</script>

<template>
    <v-container>
        <v-text-field label="Enter a URL" class="mt-5" prepend-inner-icon="mdi-link" v-model="link" @keydown.enter="validURL()">

        </v-text-field>
        <v-row class="justify-center">
            <v-btn color="green" @click="validURL()">Shorten</v-btn>
            
        </v-row>
        <v-container>
            <h3> {{ shortenedLink }} </h3>
        <v-row>
            <v-col cols="5">
                <v-progress-linear
            color="deep-purple-accent-4"
            indeterminate
            rounded
            v-show="loading"
            height="3"
          ></v-progress-linear>
            </v-col>
        </v-row>
        </v-container>
</v-container>
</template>