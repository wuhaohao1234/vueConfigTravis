<template>
    <div>
        <h1>webpack搭建ts中的vue</h1>
        <Hello/>
    </div>
</template>

<script lang="ts">
import {Vue,Component} from "vue-property-decorator"
import Hello from './components/Hello.vue'
@Component({
    components: {
        Hello
    }
})
export default class App extends Vue {
    
}
</script>
