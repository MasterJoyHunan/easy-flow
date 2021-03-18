<template>
    <div>
        <div class="ef-node-form">
            <div class="ef-node-form-header">
                编辑
            </div>
            <div class="ef-node-form-body">
                <el-form :model="node"
                         label-position="top"
                         ref="dataForm"
                         v-show="type === 'node'">
                    <el-form-item label="名称">
                        <el-input v-model="node.name"></el-input>
                    </el-form-item>
                    <el-form-item label="之后任务">
                        <el-input v-model="node.afterTask"></el-input>
                    </el-form-item>
                    <!-- <el-form-item label="指派类型">
                        <el-input v-model="node.afterTask"></el-input>
                    </el-form-item> -->
                    <el-form-item label="指派人">
                        <el-input v-model="node.afterTask"></el-input>
                    </el-form-item>
                    <el-form-item label="对表单操作">
                        <el-input v-model="node.afterTask"></el-input>
                    </el-form-item>
                    <el-form-item label="会签">
                        <el-input v-model="node.afterTask"></el-input>
                    </el-form-item>
                    <el-form-item label="主动接单">
                        <el-input v-model="node.afterTask"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary"
                                   @click="save">保存</el-button>
                    </el-form-item>
                </el-form>

                <el-form :model="line"
                         ref="dataForm"
                         label-width="80px"
                         v-show="type === 'line'">
                    <el-form-item label="条件">
                        <el-input v-model="line.label"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary"
                                   @click="saveLine">保存</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </div>

</template>

<script>
import { cloneDeep } from 'lodash'

export default {
    data() {
        return {
            visible: true,
            // node 或 line
            type: 'node',
            node: {},
            line: {},
            data: {},
            stateList: [{
                state: 'success',
                label: '成功'
            }, {
                state: 'warning',
                label: '警告'
            }, {
                state: 'error',
                label: '错误'
            }, {
                state: 'running',
                label: '运行中'
            }]
        }
    },
    methods: {
        /**
         * 表单修改，这里可以根据传入的ID进行业务信息获取
         * @param data
         * @param id
         */
        nodeInit(data, id) {
            this.type = 'node'
            this.data = data
            data.nodeList.filter((node) => {
                if (node.id === id) {
                    this.node = cloneDeep(node)
                }
            })
        },
        lineInit(line) {
            this.type = 'line'
            this.line = line
        },
        // 修改连线
        saveLine() {
            this.$emit('setLineLabel', this.line.from, this.line.to, this.line.label)
        },
        save() {
            this.data.nodeList.filter(node => {
                if (node.id === this.node.id) {
                    node.name = this.node.name
                    node.afterTask = this.node.afterTask
                    // TODO....
                    this.$emit('repaintEverything')
                }
            })
        }
    }
}
</script>
<style scoped>
.ef-node-form-header {
    height: 32px;
    border-top: 1px solid #dce3e8;
    border-bottom: 1px solid #dce3e8;
    background: #f1f3f4;
    color: #000;
    line-height: 32px;
    padding-left: 12px;
    font-size: 14px;
}

.ef-node-form-body {
    margin-top: 10px;
    padding-left: 10px;
    padding-right: 10px;
}
</style>