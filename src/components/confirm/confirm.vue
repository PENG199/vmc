<template>
    <dialog :show="show" class="vmc-confirm" :class="{'no-content': !content}">
        <child name="header">
            <div><p>{{title}}</p></div>
        </child>
        <child name="content" v-show="content">{{{content}}}</child>
        <child name="footer">
            <span class="vmc-1px-right" :class="type === 0 ? 'default' : 'primary'" @click.stop="_onCancel">
                {{btn1}}
            </span>
            <span class="primary" @click.stop="_onConfirm">
                {{btn2}}
            </span>
        </child>
    </dialog>
</template>

<script type="text/ecmascript-6">
    import Dialog from '../dialog';

    export default {
        components: {
            Dialog
        },
        props: {
            show: Boolean,
            title: String,
            content: String,
            confirm: {
                type: Function,
                default: null
            },
            cancel: {
                type: Function,
                default: null
            },
            btn1: {
                type: String,
                default: '取消'
            },
            btn2: {
                type: String,
                default: '确定'
            },
            type: {
                type: Number,
                default: 0
            }
        },
        methods: {
            _onConfirm(e) {
                this.show = false;
                this.$emit('on-confirm', e);

                if (this.confirm && typeof this.confirm === 'function') {
                    this.confirm();
                }
            },
            _onCancel(e) {
                this.show = false;
                this.$emit('on-cancel', e);

                if (this.cancel && typeof this.cancel === 'function') {
                    this.cancel();
                }
            },
            _show(options) {
                Object.keys(options).forEach(v => this[v] = options[v]);
                this.show = true;
            }
        }
    }
</script>