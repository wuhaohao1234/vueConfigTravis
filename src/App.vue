<template>
    <div class="app" >
        <h1>webpack搭建ts中的vue</h1>
        <Helloword/>
    </div>
</template>

<script lang="ts">
import {Vue,Component} from "vue-property-decorator"
import Helloword from './components/Helloword.vue'
@Component({
    components: {
        Helloword
    }
})
export default class App extends Vue {
    
}
</script>
<style>
html,body{
    height: 100%;
}
.app{
    padding: 2rem;
}
body{
    background:linear-gradient(to bottom, #1867c0, #5CBBF6);
    color: #fff;
    font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}
h1{
    text-align: center;
}
</style>
