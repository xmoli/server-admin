<template>
    <content-base class="container">
        <ul class="title">
            <li class="first">
                <span>ID</span>
                <span>名称</span>
                <span class="last-span">
                    操作
                </span>
            </li>
        </ul>
        <ul class="content">
            <li 
                v-for="(item, index) in items" :key="item.id"
                v-on:click.stop="openMenu(index)"
            >
                <span>{{item.id}}</span>
                <span>{{item.name}}</span>
                <span
                    class="last-span"
                >
                <i class="fa fa-ellipsis-h" />
                <item-menu 
                    class="item-menu"
                    v-bind:open="menuAnchor[index]"
                    @close-menu="closeMenu"
                    >
                    <ul>
                        <li  class="warn" 
                            v-on:click.stop="delTargetTask($event,index)">
                            <button tabindex="0">删除</button>
                        </li>
                        <li 
                            v-on:click.stop="$emit('change-task', index)" 
                        >
                            <button tabindex="0">修改</button>
                        </li>
                        <li 
                            v-on:click.stop="$emit('clone-task', index)"
                        >
                            <button tabindex="0">克隆</button>
                        </li>
                    </ul>
                </item-menu>
                </span>
            </li>
        </ul>
    </content-base>
</template>
<script>
export default {
    components: {
        "item-menu": () => import('./Menu'),
        "content-base": ()=> import('./Content')
    },
    props: ["items"],
    data (){
        return {
            showItem: null,
            detail: null,
            menuAnchor: [],
            openAchor: 0,
        }
    },
    methods: {
        showDetail (e) {
            if (this.showItem) {
                this.showItem.className = ''
            }
            e.current.target.className = 'showed-item'
            this.showItem = e.current.target
            this.detail = this.items[this.showItem]
        },
        openMenu (index) {
            this.openAchor = index
            this.menuAnchor[index] = true
            this.menuAnchor = Object.assign({},this.menuAnchor)
        },
        closeMenu () {
            this.menuAnchor[this.openAchor] = false
            this.menuAnchor = Object.assign({},this.menuAnchor)
        },
        delTargetTask (e, index) {
            this.$emit('delete-task',index)
        }
    }
}
</script>
<style scoped>
    .container {
        position: absolute;
        display: flex;
        padding: 1em 16px;
        flex-direction: column;
    }
    .container>ul {
        background: white;
        margin: 0 8px;
        border-spacing: 0; 
    }
    .container>ul.title {
        border-radius: .3em .3em 0 0;
    }
    ul li {
        padding: 1em 0;
        display: flex;
        border-bottom: 1px solid rgb(226, 226, 226);
    }
    ul li span {
        display: inline-block;
        min-width: 13vw;
    }
    ul.title li span {
        text-indent: .5em;
    }
    ul.content>li:hover {
        cursor: pointer;
        background: rgba(0,0,0,.1);
    }
    .container ul.content {
        margin-bottom: 6em;
    }
    .container>ul>li>span.last-span:hover{
        background: white;
    }
    .container>ul.content>li {
        padding: .9em 0;
    }
    .container>ul.title>li:first-child {
        font-weight: bold;
        border-top: none;
    }
    .item-menu {
        transform: translateX(50%);
        overflow: hidden;
        width:fit-content;
    }
    .item-menu ul li {
        padding: 0;
    }
    .item-menu ul button {
        border-radius: none;
        background: transparent;
        display: block;
        padding: 8px 2em;
        border: none;
    }
    .item-menu li.warn{
        background: rgb(173, 16, 16);
    }
    .item-menu li.warn button {
        color: white;
    }
    .item-menu ul li button:hover, .item-menu ul li button:focus {
        background: rgba(0,0,0,.3);
    }
</style>
<style scoped>
@media only screen and (max-width: 800px) {
    .container {
        padding: 0;
    }
    ul li span {
        width: 33%;
    }
}
</style>