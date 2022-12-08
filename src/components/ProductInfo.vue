<template>
    <div class="text-center product-info">
        <h2>Información del producto</h2>
        <img :src="productInfo.image"  />
        <br>
        <h6 class="card-title fw-bold">Nombre: {{ productInfo.nombre }}</h6>
        <p class="card-text">Descripción: {{ productInfo.descripcion }}</p>
        <h3 class="pt-3 pb-2">Precio: $ {{ productInfo.precio }}</h3>
        <br>
        <hr />
        <button type="button" class="btn btn-primary btn-sm" @click="$emit('backToList')">Volver...</button>
    </div>
</template>

<script>
export default {
    name: 'ProductInfo',

    props: {
        productInfo: Object,
    },
    data() {
        return {
            
        };
    },

    mounted() {
        
    },

    methods: {
        
    },
};
</script>

<style scoped>
.product-info {
    border: #CCC solid 1px;
    border-radius:  10px;
    padding: 20px;
    margin-top: 20px;
}
</style>