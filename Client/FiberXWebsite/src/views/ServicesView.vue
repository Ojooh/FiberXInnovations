<script>
import ServicesWelcome from '../components/Services/ServicesWelcome.vue';
import ServicesList from '../components/Services/ServicesList.vue';
import ContactUsForm from '../components/ContactUs/ContactUsForm.vue';


export default {
    data() { 
        return { 
            external_content: this.$root.content_data.ServicesForm,
        }
    },
    methods: {

    },
    created: async function() { },
    mounted: function() { 
        const mainSection = document.getElementById('ScrollArea');
        mainSection.scrollTo(0, 0);
    },
    components: { ServicesWelcome, ServicesList, ContactUsForm },
}
</script>

<template>
    <div class="w-full">
        <ServicesWelcome />
        <ServicesList />
        <ContactUsForm :external_content="external_content" />
    </div>
</template>
