<template>
    <div ref="boxRef" id="virtual-list" class="box-wrap" @scroll="scrollHandle">
        <!-- 高度为所有item的高度，用于撑开滚动条 -->
        <div ref="scrollRef"></div>
        <!-- 展示的虚拟列表内容 -->
        <div class="show-wrap" :style="{transform: `translateY(${offset}px)`}">
            <div class="item-wrap" v-for="item in showList" :key="item.id">
                {{ item.val }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "virtual-list",
    props: {
        size: Number, // 每个item的高度
        items: Number, // 可见区域展示的item个数
        arrayData: Array, // 需要渲染的列表数据
    },
    data () {
        return {
            start: 0, // 首部标记
            end: this.items, // 尾部标记 = 展示的item个数
            offset: 0, // 列表内容偏移量
        }
    },
    mounted() {
        // 初始化滚动条的高度
        this.$refs.scrollRef.style.height = `${this.arrayData.length * this.size}px`;
        // 初始化外部容器box-wrap的高度
        this.$refs.boxRef.style.height = `${this.size * this.items}px`;
    },
    computed: {
        showList() {
            // 返回可视区的数据列表
            // 选一个大于start，小于item个数items的值
            const prevCount = Math.min(this.start, this.items);
            // 真正渲染前面的 prevcount 项
            const prevStart = this.start - prevCount;
            // 选一个大于end，小于item个数items的值
            const nextCount = Math.min(this.arrayData.length - this.end, this.items);
            // 真正渲染的后面的 nextcount 项
            const nextEnd = this.end + nextCount;
            return this.arrayData.slice(prevStart, nextEnd);
        }
    },
    methods: {
        scrollHandle() {
            // 监听滚动，滚动时，需要调整 start 和 end ，还有偏移量 offset
            let boxScrollTop = this.$refs.boxRef.scrollTop;
            // start是下标，所以减1
            this.start = Math.ceil(boxScrollTop / this.size) - 1 >= 0 ? Math.ceil(boxScrollTop / this.size) - 1 : 0;
            this.end = this.start + this.items;
            // 滚动时内容需要偏移，使它出现在可视区域,向上（下）滚动，向下（上）偏移，首部标记 x 每个item的高度就是偏移量
            const prevCount = Math.min(this.start, this.items);
            // 前面比 items 小的时候，就不设置offset，让他滚动，直到大于 items，就可以开始设置偏移offset来调整中间区域
            this.offset = (this.start - prevCount) * this.size;
        }
    }
}
</script>

<style scoped>
.box-wrap {
    position: relative;
    overflow-y: scroll;
    background-color: darkseagreen;
}
.show-wrap {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
}
.item-wrap {
    height: 40px;
    border: 1px solid darkred;
    box-sizing: border-box;
}
</style>