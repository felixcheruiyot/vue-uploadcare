<template>
  <input type="hidden" v-bind:data-images-only="imagesOnly">
</template>

<script>
  import uploadcare from 'uploadcare-widget';
  
  export default {
    props: {
      url: {
        required: true
      },
      imagesOnly: {
        required: false
      }
    },
    data() {
      return {
        uploadcareUrl: "",
        widget: ''
      }
    },
    mounted() {
      this.initWidget();
    },
    watch: {
      uploadcareUrl() {
        this.updatePropUrl();
      },
      url() {
        this.updateWidgetValue();
      }
    },
    methods: {

      initWidget() {
        this.widget = uploadcare.Widget(this.$el);
        this.updateWidgetValue();

        this.widget.onUploadComplete((info) => {
          this.uploadcareUrl = info.cdnUrl;
        });
      },
      
      updatePropUrl() {
        this.$emit('update:url', this.uploadcareUrl);
      },

      updateWidgetValue() {
        this.widget && this.widget.value(this.url);
      }
    }
  };
</script>
