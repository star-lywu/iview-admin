<template>
    <div>
        <Table border :columns="columns" :data="tableData"></Table>
        <Modal v-model="deleteShow" width="360">
            <p slot="header" style="color:#f60;text-align:center">
                <Icon type="ios-information-circle"></Icon>
                <span>删除</span>
            </p>
            <div style="text-align:center">
                <p>确认要删除【{{delRowName}}】卡片吗？</p>
            </div>
            <div slot="footer">
                <Button type="error" size="large" long :loading="delete_loading" @click="del">删除</Button>
            </div>
        </Modal>
    </div>
</template>
<script>
import { getEntranceCards } from '@/api/data'
export default {
  data () {
    return {
      delRowName: '',
      deleteShow: false,
      delete_loading: false,
      columns: [
        {
          title: 'ID',
          key: 'id',
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong', params.row.id)
            ])
          }
        },
        {
          title: '卡片名称',
          key: 'title'
        },
        {
          title: '卡片英文名称',
          key: 'titleEn'
        },
        {
          title: '是否是新入口',
          key: 'newAdd'
        },
        {
          title: '排序字段',
          key: 'orderNumber'
        },
        {
          title: '卡片跳转路径',
          key: 'actionUrl'
        },
        {
          title: '卡片图片',
          key: 'imageUrl'
        },
        {
          title: '是否需要登录',
          key: 'needLogin'
        },
        {
          title: '是否可以展示',
          key: 'canShow'
        },
        {
          title: '创建时间',
          key: 'createTime'
        },
        {
          title: '操作',
          key: 'action',
          width: 150,
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h('Button', {
                props: {
                  type: 'primary',
                  size: 'small'
                },
                style: {
                  marginRight: '5px'
                },
                on: {
                  click: () => {
                    this.show(params.index)
                  }
                }
              }, 'View'),
              h('Button', {
                props: {
                  type: 'error',
                  size: 'small'
                },
                on: {
                  click: () => {
                    this.remove(params.index)
                  }
                }
              }, 'Delete')
            ])
          }
        }
      ],
      tableData: []
    }
  },
  methods: {
    show (index) {
      this.modalShow = true
    },
    remove (index) {
      this.delRowName = this.tableData[index].title
      this.deleteShow = true
      this.tableData.splice(index, 1)
    },
    del () {
      this.delete_loading = true
      setTimeout(() => {
        this.delete_loading = false
        this.deleteShow = false
        this.$Message.success('Successfully delete')
      }, 1000)
    }
  },
  mounted () {
    getEntranceCards().then(res => {
      console.log(res.data)
      this.tableData = res.data.data
    })
  }
}
</script>
