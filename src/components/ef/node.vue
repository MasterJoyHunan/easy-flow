<template>
    <div ref="node"
         :style="nodeContainerStyle"
         @click="clickNode"
         @mouseup="changeNodeSite"
         :class="nodeContainerClass">
        <!-- 节点类型的图标 -->
        <div class="ef-node-left-ico">
            <svg-icon :icon-class="node.ico"
                      :fill="node.color"></svg-icon>
        </div>
        <!-- 节点名称 -->
        <div class="ef-node-text"
             :show-overflow-tooltip="true">
            {{node.name}}
        </div>
        <div class="zero zero-top flow-node-drag"></div>
        <div class="zero zero-bottom flow-node-drag"></div>
        <div class="zero zero-left flow-node-drag"></div>
        <div class="zero zero-right flow-node-drag"></div>
    </div>
</template>

<script>
export default {
    props: {
        node: Object,
        activeElement: Object
    },
    data() {
        return {}
    },
    computed: {
        nodeContainerClass() {
            return {
                'ef-node-container': true,
                'ef-node-active': this.activeElement.type == 'node' ? this.activeElement.nodeId === this.node.id : false
            }
        },
        // 节点容器样式
        nodeContainerStyle() {
            return {
                top: this.node.top,
                left: this.node.left,
                borderColor: this.node.color,
            }
        },
    },
    methods: {
        // 点击节点
        clickNode() {
            this.$emit('clickNode', this.node.id)
        },
        // 鼠标移动后抬起
        changeNodeSite() {
            // 避免抖动
            if (this.node.left == this.$refs.node.style.left && this.node.top == this.$refs.node.style.top) {
                return;
            }
            this.$emit('changeNodeSite', {
                nodeId: this.node.id,
                left: this.$refs.node.style.left,
                top: this.$refs.node.style.top,
            })
        }
    }
}
</script>
<style scoped>
/*节点的最外层容器*/
.ef-node-container {
    position: absolute;
    display: flex;
    max-width: 140px;
    height: 40px;
    border: 8px solid #5bf395;
    border-radius: 50px;
}

.ef-node-container:hover {
    /* 设置移动样式*/
    cursor: move;
    box-shadow: 0px 0px 10px 10px #a1afa6;
}

/*节点激活样式*/
.ef-node-active {
    box-shadow: 0px 0px 10px 10px #a1afa6;
}

/*节点左侧的图标*/
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

.zero {
    background: #fff;
    border: 1px solid #000;
    position: absolute;
    width: 6px;
    height: 6px;
    border-radius: 6px;
    opacity: 0;
}

.ef-node-container:hover .zero {
    opacity: 1;
}

.zero:hover {
    /* 设置拖拽的样式 */
    cursor: crosshair;
}

.zero-top {
    top: -8px;
    left: 50%;
    transform: translateX(-50%);
}
.zero-bottom {
    bottom: -8px;
    left: 50%;
    transform: translateX(-50%);
}
.zero-left {
    left: -8px;
    top: 50%;
    transform: translateY(-50%);
}
.zero-right {
    right: -8px;
    top: 50%;
    transform: translateY(-50%);
}
</style>