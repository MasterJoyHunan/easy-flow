<template>
    <div class="flow-menu"
         ref="tool">
        <div v-for="menu  in  menuList"
             :key="menu.id">
            <span class="ef-node-pmenu"
                  @click="menu.open = !menu.open"><i :class="{'el-icon-caret-bottom': menu.open,'el-icon-caret-right': !menu.open}"></i>&nbsp;{{menu.name}}</span>
            <ul v-show="menu.open"
                class="ef-node-menu-ul">
                <draggable @end="end"
                           @start="move"
                           v-model="menu.children"
                           :options="draggableOptions">
                    <li v-for="subMenu in menu.children"
                        :key="subMenu.id"
                        :type="subMenu.type">
                        <div style="display: inline-block;">
                            <div class="ef-node-menu-li"
                                 :style="{borderColor:subMenu.color}">
                                <div class="ef-node-left-ico">
                                    <svg-icon :icon-class="subMenu.ico"
                                              :fill="subMenu.color"></svg-icon>
                                </div>
                                <!-- 节点名称 -->
                                <div class="ef-node-text">
                                    {{subMenu.name}}
                                </div>
                            </div>
                        </div>
                    </li>
                </draggable>
            </ul>
        </div>
    </div>
</template>
<script>
import draggable from 'vuedraggable'

var mousePosition = {
    left: -1,
    top: -1
}

export default {
    data() {
        return {
            activeNames: '1',
            // draggable配置参数参考 https://www.cnblogs.com/weixin186/p/10108679.html
            draggableOptions: {
                preventOnFilter: false,
                sort: false,
                disabled: false,
                ghostClass: 'tt',
                // 不使用H5原生的配置
                forceFallback: true,
                // 拖拽的时候样式
                // fallbackClass: 'flow-node-draggable'
            },
            // 默认打开的左侧菜单的id
            defaultOpeneds: ['1', '2'],
            menuList: [
                {
                    id: '1',
                    type: 'group',
                    name: '基础节点',
                    ico: 'el-icon-video-play',
                    open: true,
                    children: [
                        {
                            id: '11',
                            type: 'ks',
                            name: '开始',
                            ico: 'ks',
                            color: '#5bf395'
                        }, {
                            id: '12',
                            type: 'js',
                            name: '结束',
                            ico: 'js',
                            color: '#FF6343',
                        }
                    ]
                },
                {
                    id: '2',
                    type: 'group',
                    name: '流程节点',
                    ico: 'el-icon-video-pause',
                    open: true,
                    children: [
                        {
                            id: '21',
                            type: 'sp',
                            name: '审批节点',
                            ico: 'sp',
                            color: '#742BB6'
                        }, {
                            id: '22',
                            type: 'rw',
                            name: '处理节点',
                            ico: 'rw',
                            color: '#742BB6',
                        },
                    ]
                },
                {
                    id: '3',
                    type: 'group',
                    name: '网关节点',
                    ico: 'el-icon-video-pause',
                    open: true,
                    children: [
                        {
                            id: '31',
                            type: 'ptgw',
                            name: '排他网关',
                            ico: 'pt',
                            color: '#008DBC',
                        }, {
                            id: '32',
                            type: 'bxwg',
                            name: '并行网关',
                            ico: 'bx',
                            color: '#008DBC',
                        }
                    ]
                }
            ],
            nodeMenu: {}
        }
    },
    components: {
        draggable
    },
    methods: {
        // 根据类型获取左侧菜单对象
        getMenuByType(type) {
            for (let i = 0; i < this.menuList.length; i++) {
                let children = this.menuList[i].children;
                for (let j = 0; j < children.length; j++) {
                    if (children[j].type === type) {
                        return children[j]
                    }
                }
            }
        },
        // 拖拽开始时触发
        move(evt, a, b, c) {
            var type = evt.item.attributes.type.nodeValue
            this.nodeMenu = this.getMenuByType(type)
        },
        // 拖拽结束时触发
        end(evt, e) {
            this.$emit('addNode', evt, this.nodeMenu, mousePosition)
        },
    }
}
</script>
<style scoped>
/*节点菜单*/
.flow-menu {
    overflow-x: scroll;
}
.ef-node-pmenu {
    cursor: pointer;
    height: 32px;
    line-height: 32px;
    width: 225px;
    display: block;
    font-weight: bold;
    color: #4a4a4a;
    padding-left: 5px;
}

.ef-node-pmenu:hover {
    background-color: #e0e0e0;
}

.ef-node-menu-ul {
    list-style: none;
    padding-left: 20px;
    margin: 0;
}

.ef-node-menu-li {
    display: flex;
    margin-bottom: 10px;
    max-width: 140px;
    height: 40px;
    border: 8px solid #5bf395;
    border-radius: 50px;
}

.ef-node-menu-li:hover {
    /* 设置移动样式*/
    cursor: move;
    box-shadow: 0px 0px 10px 10px #a1afa6;
}

.ef-node-left-ico {
    margin: 5px;
    height: 30px;
    line-height: 30px;
    font-size: 30px;
    fill: #5bf395;
}

/*节点显示的文字*/
.ef-node-text {
    margin-right: 8px;
    color: #565758;
    font-size: 12px;
    font-weight: bold;
    line-height: 40px;
    /* width: 100px; */
    /* 设置超出宽度文本显示方式*/
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    text-align: center;
}
</style>