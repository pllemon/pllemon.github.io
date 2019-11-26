---
title: Element Tree 组件的拓展
toc: true
date: 2019-11-24 13:33:30
tags: [element, vue]
categories: [JAVASCRIPT]
---

# 需求描述
> element-ui 的 Tree 组件在某些功能上有欠缺，在此对它进行一些业务上的补充和调整，最终效果如下图。  
测试环境：element-ui（2.12.0）、chrome。

![](/images/extent_tree.png)
<!-- more -->

# 基本布局
```html
<template>
  <div class="tree-content">
    <el-tree
      class="tree"
      default-expand-all
      :data="treeData" 
      :expand-on-click-node="false"
      :indent="16"
    >
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span :title="node.label" class="tree-label">{{ node.label }}</span>
        <span class="tree-action">
          <el-button type="text" size="mini" @click="editSingle(data)">编辑</el-button>
          <el-button type="text" size="mini" @click="deleteSingle(data)">删除</el-button>
        </span>
      </span>
    </el-tree>
  </div>
</template>
```

```scss
.tree-content{
  margin: 50px 100px;
  width: 310px;
  padding: 30px;
  border: 1px solid #ddd;
}
.el-tree{
  width: 100%;
}
.el-tree-node:focus>.el-tree-node__content{
  background: none;
}
.el-tree-node__content:hover{
  background: none;
}
```

# 功能拓展
## 2.1 添加层级指示线
> 实现原理：通过css的伪类给树的元素添加左边虚线
```scss
.tree-content {
  .el-tree > .el-tree-node > .el-tree-node__content:first-child {
    &:before,
    &:after {
      border: none;
    }
  }
  .el-tree-node .el-tree-node__content {
    &:before,
    &:after {
      content: "";
      position: absolute;
    }
    &:before {
      border-left: 1px dotted #aaa;
      height: 100%;
      top: 0;
      width: 1px;
      margin-left: -5px;
      margin-top: -10px;
    }
    &:after {
      border-top: 1px dotted #aaa;
      height: 20px;
      top: 14px;
      width: 10px;
      margin-left: -5px;
    }
  }
  .el-tree .el-tree-node {
    position: relative;
  }
}
```

参考链接
* https://blog.csdn.net/weixin_38641550/article/details/89360871
* https://blog.csdn.net/u011225228/article/details/93141026

疑问
* 好像并不需要如参考链接那样用到 /deep/ 深度选择器

优缺点
* 简单快捷，不涉及元素的操作

## 2.2 鼠标移入移出显示隐藏操作按钮
> 实现原理：通过css的 :hover 控制显示隐藏
```scss
.el-tree-node__content .tree-action{
  display: none;
}
.el-tree-node__content:hover .tree-action{
  display: inline-block;
}
```

优缺点
* 简单快捷，不涉及元素的操作
* 只是简单的控制显示隐藏，无法根据特有的限制条件去控制

## 2.3 文字超出隐藏
> 实现原理：利用 flex 布局，label 文字自适应，超出显示省略号
```scss
.custom-tree-node{
  width: 100%;
  display: flex;
  align-items: center;
  .tree-action{
    flex-shrink: 0;
  }
  .tree-label{
    width: 0;
    flex: 1;
    overflow: hidden;
    text-overflow : ellipsis;
  }
}
```


# 完整代码
```html
<template>
  <div class="tree-content">
    <el-tree
      class="tree"
      default-expand-all
      :data="treeData" 
      :expand-on-click-node="false"
      :indent="16"
    >
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span :title="node.label" class="tree-label">{{ node.label }}</span>
        <span class="tree-action">
          <el-button type="text" size="mini" @click="editSingle(data)">编辑</el-button>
          <el-button type="text" size="mini" @click="deleteSingle(data)">删除</el-button>
        </span>
      </span>
    </el-tree>
  </div>
</template>

<script>

export default {
  data() {
    return {
      treeData: [
        {
          label: '书单推荐',
          children: [
            {
              label: 'Html & Css',
              children: [
                {
                  label: 'Html5 高级程序设计'
                },
                {
                  label: 'Css 权威指南'
                }
              ]
            },
            {
              label: 'Javascript',
              children: [
                {
                  label: '基础',
                  children: [
                    {
                      label: 'Javascript 高级程序设计'
                    },
                    {
                      label: 'JavaScript DOM 编程艺术'
                    }
                  ]
                },
                {
                  label: '拓展',
                  children: [
                    {
                      label: 'JavaScript设计模式与开发实践'
                    },
                    {
                      label: '高性能JavaScript'
                    }
                  ]
                }
              ]
            }
          ]
        }
      ]
    }
  },
  mounted() {
    console.log(this.$refs.tree)
  },
  methods: {
    editSingle(data) {
      console.log(data)
    },
    deleteSingle(data) {
      console.log(data)
    }
  }
}
</script>

<style lang="scss">
.tree-content{
  margin: 50px 100px;
  width: 310px;
  padding: 30px;
  border: 1px solid #ddd;
}
.el-tree{
  width: 100%;
}
.el-tree-node:focus>.el-tree-node__content{
  background: none;
}
.el-tree-node__content:hover{
  background: none;
}

// 添加指示线
.tree-content {
  .el-tree > .el-tree-node > .el-tree-node__content:first-child {
    &:before,
    &:after {
      border: none;
    }
  }
  .el-tree-node .el-tree-node__content {
    &:before,
    &:after {
      content: "";
      position: absolute;
    }
    &:before {
      border-left: 1px dotted #aaa;
      height: 100%;
      top: 0;
      width: 1px;
      margin-left: -5px;
      margin-top: -10px;
    }
    &:after {
      border-top: 1px dotted #aaa;
      height: 20px;
      top: 14px;
      width: 10px;
      margin-left: -5px;
    }
  }
  .el-tree .el-tree-node {
    position: relative;
  }
}

// 鼠标移入、鼠标移出
.el-tree-node__content .tree-action{
  display: none;
}
.el-tree-node__content:hover .tree-action{
  display: inline-block;
}

// 文字过长
.custom-tree-node{
  width: 100%;
  display: flex;
  align-items: center;
  .tree-action{
    flex-shrink: 0;
  }
  .tree-label{
    width: 0;
    flex: 1;
    overflow: hidden;
    text-overflow : ellipsis;
  }
}
</style>

```