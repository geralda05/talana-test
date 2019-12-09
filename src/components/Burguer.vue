<template>
    <div id="burger"
         :class="{ 'active' : isBurgerActive }"
         @click.prevent="toggle">
        <slot>
            <button type="button" class="burger-button btn" style="background: transparent !important; border:0 !important; cursor: pointer;" title="Menu">
                <span class="hidden"><i class="fa fa-bars"></i></span>
                <span class="burger-bar burger-bar--1"></span>
                <span class="burger-bar burger-bar--2"></span>
                <span class="burger-bar burger-bar--3"></span>
            </button>
        </slot>
    </div>
</template>
<script>
    import { store, mutations } from '../store.js' 
    
    export default {
        name:'Burguer',
        computed: {
            isBurgerActive() {
                return store.isNavOpen
            }
        },
        methods: {
            toggle() {
                mutations.toggleNav()
            }
        }
    }
</script>
