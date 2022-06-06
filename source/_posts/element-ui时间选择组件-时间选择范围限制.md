---
title: element-ui时间选择组件-时间选择范围限制
date: 2021-09-01 09:09:55
tags:
- Vue
- ElementUI
- 原创
categories:
- 前端
---
element-ui时间选择组件-时间选择范围限制...

<!--more-->



##### 限制当天前后45天可选

```
    pickerOptions: {
      disabledDate: (time) => {
        const month = 45 * 24 * 60 * 60 * 1000 // 设定日期范围
        const startDate = new Date(new Date(new Date().toLocaleDateString()).getTime()).getTime();
        const minTime = startDate - month
        const maxTime = startDate + month
        if (startDate) {
          // 选中第一个时，前后45天[-45,45]共90天可选选择，超出的不可选，
          return time.getTime() < minTime || time.getTime() > maxTime
        }
        // 不选任何日期执行下面的代码
        // return time.getTime() > Date.now()
      }
    }
```



##### 限制选择30天时间范围，且不大于当前时间

```vue
    mounted () {
       // 保存this指向
        thisVue = this;
    },
    data: {
        // 日期
        valueDate: '',
        timeOptionRange: null,
        pickerOptions: {
            disabledDate(time) {
                // 获取选中时间
                let timeOptionRange = thisVue.timeOptionRange;
                // 获取时间范围(30天的毫秒数)
                let secondNum = 30 * 24 * 60 * 60 * 1000;
                if (timeOptionRange) {
                    //如果有选中时间 设置超过选中时间后的30天||超过选中前的30天||大于当前时间 不可选
                    return time.getTime() > timeOptionRange.getTime() + secondNum || time.getTime() < timeOptionRange.getTime() - secondNum || time.getTime() > (Date.now() - 8.64e6);
                } else {
                    //如果没有选中时间（初始化状态） 设置当前时间后的时间不可选
                    return time.getTime() > (Date.now() - 8.64e6);
                }
            },
            onPick(maxDate, minDate) {
                // 当选中了第一个日期还没选第二个
                // 只选中一个的时候自动赋值给minDate，当选中第二个后组件自动匹配，将大小日期分别赋值给maxDate、minDate
                if (maxDate.minDate && !maxDate.maxDate) {
                    thisVue.timeOptionRange = maxDate.minDate;
                }
                // 如果有maxDate属性，表示2个日期都选择了，则重置timeOptionRange
                if (maxDate.maxDate) {
                    thisVue.timeOptionRange = null;
                }
            }
        }
    },
```

