<template>
    <div>
        <Head>
            <title>Import data</title>
        </Head>

        <heading class="mb-6">CSV Import</heading>

        <card class="flex flex-col items-center justify-center" style="min-height: 300px">
            <!-- TODO: Put some history here -->
            <!-- TODO: Replace with Nova's own file field for sexier results -->
            <input type="file" name="file" ref="file" @change="handleFile" class="mb-3">
            <DefaultButton v-bind:disabled="!file" @click="upload">Upload &amp; Configure &rightarrow;</DefaultButton>
        </card>
    </div>
</template>

<script>
export default {
    data() {
        return {
            file: '',
        };
    },

    methods: {
        handleFile: function (event) {
            this.file = this.$refs.file.files[0];
        },

        upload: function (event) {
            let formData = new FormData();
            // send it to the server
            formData.append('file', this.file);

            const self = this;

            // if it's valid, move to the next screen
            return Nova.request()
                .post('/nova-vendor/laravel-nova-csv-import/upload',
                    formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                )
                .then((response) => {
                    Nova.success('File uploaded!')
                    Nova.visit(response.data.configure);
                })
                .catch((e) => {
                    Nova.error(e.response.data.message);
                });
        }
    }
}
</script>
