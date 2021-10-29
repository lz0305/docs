


#### 行/列合并 

4.0.0 设置属性 `span-method` 可以指定合并行或列的算法。

该方法参数为 4 个对象：

- **row**: 当前行
- **column**: 当前列
- **rowIndex**: 当前行索引
- **columnIndex**: 当前列索引

该函数可以返回一个包含两个元素的数组，第一个元素代表 rowspan，第二个元素代表 colspan。 也可以返回一个键名为 rowspan 和 colspan 的对象。




![wvuAW4.png](https://s1.ax1x.com/2020/09/23/wvuAW4.png)



```vue
<template>
    <Table :columns="columns14" :data="data5" border :span-method="handleSpan"></Table>
</template>
<script>
    export default {
        data () {
            return {
                columns14: [
                    {
                        title: 'Date',
                        key: 'date'
                    },
                    {
                        title: 'Name',
                        key: 'name'
                    },
                    {
                        title: 'Age',
                        key: 'age'
                    },
                    {
                        title: 'Address',
                        key: 'address'
                    }
                ],
                data5: [
                    {
                        name: 'John Brown',
                        age: 18,
                        address: 'New York No. 1 Lake Park',
                        date: '2016-10-03'
                    },
                    {
                        name: 'Jim Green',
                        age: 24,
                        address: 'London No. 1 Lake Park',
                        date: '2016-10-01'
                    },
                    {
                        name: 'Joe Black',
                        age: 30,
                        address: 'Sydney No. 1 Lake Park',
                        date: '2016-10-02'
                    },
                    {
                        name: 'Jon Snow',
                        age: 26,
                        address: 'Ottawa No. 2 Lake Park',
                        date: '2016-10-04'
                    }
                ]
            }
        },
        methods: {
            handleSpan ({ row, column, rowIndex, columnIndex }) {
                if (rowIndex === 0 && columnIndex === 0) {
                    return [1, 2];
                } else if (rowIndex === 0 && columnIndex === 1) {
                    return  [0, 0];
                }
                if (rowIndex === 2 && columnIndex === 0) {
                    return {
                        rowspan: 2,
                        colspan: 1
                    };
                } else if (rowIndex === 3 && columnIndex === 0) {
                    return {
                        rowspan: 0,
                        colspan: 0
                    };
                }
            }
        }
    }
</script>
```



官方文档直接合并。现有一个需求，第一列数据相同才合并，不相同则不合并。



完整代码如下

```vue
<template>
  <div>
    <!--span-method 是 iView 4.0以后才有的属性，用于合并行/列-->
    <Table :columns="columns" :data="tableData" :span-method="handleSpan"></Table>
  </div>
</template>

<script>
  const KEY_NAME = "name";
  const KEY_ROWSPAN = "rowSpan";

  export default {
    data() {
      return {
        resultData: [],
        tableData: [
          // 见下文数据tableData
        ],
        columns: [
          // 见下文columns
        ]
      };
    },
    mounted() {
      // 挂载的时候处理数据
      this.formatData();
    },
    methods: {
      // 根据名字相同的数据算出rowspan  
      formatData() {
        var k = 0;
        while (k < this.tableData.length) {
          this.tableData[k][KEY_ROWSPAN] = 1;
          for (var i = k + 1; i <= this.tableData.length - 1; i++) {
            if (
              this.tableData[k][KEY_NAME] === this.tableData[i][KEY_NAME] &&
              this.tableData[k][KEY_NAME] !== ""
            ) {
              this.tableData[k][KEY_ROWSPAN]++;
            } else {
              break;
            }
          }
          k = i;
        }
        this.resultData = this.tableData;
      },
      // 合并行/列
      handleSpan ({ row, column, rowIndex, columnIndex }) {
        if (columnIndex === 0) {
          if (row[KEY_ROWSPAN]) {
            return {
              rowspan: row[KEY_ROWSPAN],
              colspan: 1
            };
          } else {
            return {
              rowspan: 0,
              colspan: 0
            };
          }
        } else {
          return  [1, 1];
        }
      }
    },
  };
</script>

<style>
</style>
```



数据tableData

```json
[
    {
        name: "赵伟",
        gender: "男",
        birthday: "1963-7-9",
        height: "183",
        email: "zhao@gmail.com",
        tel: "156*****1987",
        hobby: "钢琴、书法、唱歌",
        address: "上海市黄浦区金陵东路569号17楼",
        win: 100,
        loss: 20
    },
    {
        name: "赵伟",
        gender: "男",
        birthday: "1963-7-9",
        height: "183",
        email: "zhao@gmail.com",
        tel: "156*****1987",
        hobby: "钢琴、书法、唱歌",
        address: "上海市黄浦区金陵东路569号17楼",
        win: 100,
        loss: 30
    },
    {
        name: "赵伟",
        gender: "男",
        birthday: "1963-7-9",
        height: "183",
        email: "zhao@gmail.com",
        tel: "156*****1987",
        hobby: "钢琴、书法、唱歌",
        address: "上海市黄浦区金陵东路569号17楼",
        win: 100,
        loss: 40
    },
    {
        name: "李伟",
        gender: "男",
        birthday: "2003-12-7",
        height: "166",
        email: "li@gmail.com",
        tel: "182*****1538",
        hobby: "钢琴、书法、唱歌",
        address: "上海市奉贤区南桥镇立新路12号2楼",
        win: 100,
        loss: 10
    },
    {
        name: "孙伟",
        gender: "女",
        birthday: "1993-12-7",
        height: "186",
        email: "sun@gmail.com",
        tel: "161*****0097",
        hobby: "钢琴、书法、唱歌",
        address: "上海市崇明县城桥镇八一路739号",
        win: 100,
        loss: 0
    },
    {
        name: "周伟",
        gender: "女",
        birthday: "1993-12-7",
        height: "188",
        email: "zhou@gmail.com",
        tel: "197*****1123",
        hobby: "钢琴、书法、唱歌",
        address: "上海市青浦区青浦镇章浜路24号",
        win: 100,
        loss: 20
    },
    {
        name: "周伟",
        gender: "女",
        birthday: "1993-12-7",
        height: "188",
        email: "zhou@gmail.com",
        tel: "197*****1123",
        hobby: "钢琴、书法、唱歌",
        address: "上海市青浦区青浦镇章浜路24号",
        win: 100,
        loss: 30
    },
    {
        name: "周伟",
        gender: "女",
        birthday: "1993-12-7",
        height: "188",
        email: "zhou@gmail.com",
        tel: "197*****1123",
        hobby: "钢琴、书法、唱歌",
        address: "上海市青浦区青浦镇章浜路24号",
        win: 100,
        loss: 20
    },
    {
        name: "吴伟",
        gender: "男",
        birthday: "1993-12-7",
        height: "160",
        email: "wu@gmail.com",
        tel: "183*****6678",
        hobby: "钢琴、书法、唱歌",
        address: "上海市松江区乐都西路867-871号",
        win: 100,
        loss: 40
    },
    {
        name: "冯伟",
        gender: "女",
        birthday: "1993-12-7",
        height: "168",
        email: "feng@gmail.com",
        tel: "133*****3793",
        hobby: "钢琴、书法、唱歌",
        address: "上海市金山区龙胜路143号一层",
        win: 100,
        loss: 50
    }
]
```



columns

```json
[
    {
        title: "姓名",
        key: "name"
    },
    {
        title: "性别",
        key: "gender"
    },
    {
        title: "手机号码",
        key: "tel"
    },
    {
        title: "出生日期",
        key: "birthday"
    },
    {
        title: "爱好",
        key: "hobby"
    },
    {
        title: "地址",
        key: "address"
    }
]
```

