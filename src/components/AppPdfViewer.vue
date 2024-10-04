<template>
    <div v-if="isPdfLoading" class="my-loading-class">
        Loading...
    </div>
    <div v-else class="my-pdf-class" :style="{ width: '100%', height: '100%'}">
        <VPdfViewer
            v-if="pdfUrl"
            :src="pdfUrl"
        />
    </div>
</template>

<script>
import { VPdfViewer } from '@vue-pdf-viewer/viewer';
import axios from 'axios'

export default {
    name: "component-name",
    components: {
        VPdfViewer
    },
    props: {
        articlePath: String,
    },
    data() {
        return {
            pdfUrl: null,
            isPdfLoading: true,
        }
    },
    created() {
        this.getPdfUrl();
    },
    methods: {
        getPdfUrl() {
            this.isPdfLoading = true;
            axios.get('https://raw.githubusercontent.com/mozilla/pdf.js/ba2edeae/web/compressed.tracemonkey-pldi-09.pdf', {responseType: 'blob'})
            .then(async(response) => {
                const blob = await response.data;
                this.pdfUrl = URL.createObjectURL(blob);
            })
            .catch(error => console.error(error.message))
            .finally(() => this.isPdfLoading = false)
            // axios.get(`/api/getter-endpoint/${this.articlePath}`)
            //     .then(response => this.pdfUrl = `${response.data.url}`)
            //     .catch(error => console.error(error.message))
            //     .finally(() => this.isPdfLoading = false);
        },
    },
}
</script>