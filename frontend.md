# 婚纱店前端

## 技术栈
- nodejs  
- 框架：vue3  
  - typescript
  - 组合式API风格
- 组件库：element plus

## 组件
### table  
用于以表格的形式展示商品、订单等的情况。
### form  
用于填写提交post请求时的相关数据。

## script部分的解释
### 变量
1. Table  
用于展现和操作数据库对应的数据。
1. DialogVisible  
用于可见和不可见要填写的表单。
1. DialogDisable  
静止修改对应操作下表单的相关字段。
### 函数
1. handle  
实际进行相关请求的提交。
1. handleDialog  
展示对应表单。
1. fillForm  
在选择某行为操作对象时，自动填写相关数据以获得更好的可视化体验。

## 注意事项
- 后续可添加挂载前的预加载，以及每次操作之后的自动刷新，这里是为了稳定暂未添加。
