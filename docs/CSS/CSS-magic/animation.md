---

hideFooter: true

---
# 动画 #

::: tip 契子
- 出于《CSS揭秘》
- css
:::

## &01.过渡与动画 ##

- 给过渡和动画加上缓动效果（比如具有回弹效果的过渡过程）是一种流行的表现方式

cubic-bezier：三次赛贝尔，定义动画速度

1.使用浮动
```bash
.ball {
    background-color: aqua;
    width: 200px;
    height: 200px;
    border-radius: 50%;
    animation: bounce 3s cubic-bezier(.1,.25,1,.25);   #定义动画的时间
}
@keyframes bounce {
    60%, 80%, 100% {
        transform: translateY(350px);
        animation-timing-function: ease;
    }
    70% { transform: translateY(250px); }
    90% { transform: translateY(300px); }
}
```