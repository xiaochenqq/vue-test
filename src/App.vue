<template>
    <div id="app">
        <button v-show="prompt" @click="deferredPrompt(deferred)">添加到桌面</button>
        <img alt="Vue logo" src="../public/timg.jpg">
        <HelloWorld msg="Welcome to Your Vue.js App"/>
    </div>
</template>

<script>
    import HelloWorld from './components/HelloWorld.vue'

    export default {
        name: 'app',
        data() {
            return {
                prompt: false,
                deferred: null,
            }
        },
        created() {
            window.onbeforeinstallprompt = (e) => {     //当浏览器触发横幅显示事件
                console.log(e);
                this.prompt = true;
                this.deferred = e;
            }
        },
        methods: {
            deferredPrompt(e) {
                if (e) {
                    e.prompt(); //拉起添加横幅事件

                    // 通过按钮点击事件触发横幅显示
                    e.prompt();
                    // 监控用户的安装行为
                    e.userChoice.then((choiceResult) => {
                        console.log(choiceResult.outcome);
                        if (choiceResult.outcome === 'dismissed') {
                            console.log('用户取消了安装');
                        }
                        else {
                            console.log('用户已安装程序');

                            this.prompt = false; //隐藏按钮
                            this.deferred = null;
                        }
                    });
                }
            }
        },
        components: {
            HelloWorld
        }
    }
</script>

<style lang="less">
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    img {
        max-width: 100%;
    }
</style>
