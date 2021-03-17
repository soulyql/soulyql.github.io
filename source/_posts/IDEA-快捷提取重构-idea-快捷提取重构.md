---
title: IDEA-快捷提取重构
date: 2021-03-10 12:36:36.229
updated: 2021-03-16 19:36:22.085
url: http://39.106.177.179:8090/archives/idea-快捷提取重构
categories: 
tags: 快捷键
---

# 重构代码的快捷方式
总结：
```text
1.抽取代码块为新的方法
  快捷键：CTRL+ALT+M -> 定义方法名。
2.抽取常量 （将变量抽取为类常量）
  快捷键：CTRL+ALT+C -> 为常量命名
3.抽取字段 （将变量抽取为类字段）
  快捷键：CTRL+ALT+F -> 为字段命名
4.抽取方法内的局部变量
  快捷键：CTRL+ALT+V (将要抽取为局部变量选中)
5.抽取变量为方法参数
  快捷键：CTRL+ALT+P -> 为方法参数命名
6.给方法添加参数
  快捷键：CTRL+F6 输入添加参数的类型、名称和默认值
7.重构命名
  快捷键：SHIFT+F6 重新命名全局统一替换
```
## 1. Extract 抽取代码块为新的方法
**源代码：**
```java
     /**
     * 每解析一行回调此方法
     *
     * @param importTestDataEntity 行数据
     * @param analysisContext      .
     */
    @Override
    public void invoke(ImportTestDataEntity importTestDataEntity, AnalysisContext analysisContext) {
        // 必填参数校验
        String reasonForFailure = ValidatorFactoryUtils.validator(importTestDataEntity);
        importTestDataEntity.setLogStoragetime(importTime);
        if (Objects.nonNull(reasonForFailure)) {
            String reason = String.format("第%s行%s，请核实", analysisContext.readRowHolder().getRowIndex() + 1, reasonForFailure);
            ImportFailedReason importFailedReason = new ImportFailedReason(String.valueOf(UniqueID.generateId()), JSONObject.toJSONString(importTestDataEntity), reason, NOT_IGNORE, importTestDataEntity.getLogStoragetime());
            failureData.add(importFailedReason);
            return;
        }
        this.addSuccessData(importTestDataEntity);
    }
```
**1、将要抽取的代码选中 快捷键 Ctrl+ALT+M**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-27a39570566f48a6978f230236b4cbe5.png)
**2、命名抽取后方法的名称**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-92488dd76aa749f39a5770cd4e216ac6.png)
**3、方法提取后生成了一个新的方法（单一职责的体现）**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-a6b0609a06504c8fa1a070c7b147baad.png)
---
## 2.抽取常量 （将变量抽取为类常量） 
**1、从源代码中将（01）数值抽取为常量**
```java
  /**
     * 修改忽略状态sql
     *
     * @param logId 日志id
     * @return sql
     */
    private String ignoreSql(String logId) {
        return "UPDATE sys.oper_test_fail_log SET log_is_ignore = '01' WHERE log_id = '" + logId + "'";
    }
```
**2、快捷键CTRL+ALT+C**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-daf3cbe89fd549ef95b45c773ae0b9cb.png)
**3、为抽取的变量命名**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-0d17752255a34526bc7beebd52f7efdb.png)
**4、抽取重构后的效果**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-4cbb6375b4bb466c8f03b204cb5e2df8.png)
![image.png](http://39.106.177.179:8090/upload/2021/03/image-e6147a6b7961433ba297141973222c52.png)
---
## 3.抽取字段 （将变量抽取为类字段） 
**1、快捷键CTRL+ALT+F**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-5ac56a6f2cc24cd2bf6a8493a8002d7c.png)
**2、为抽取的字段命名**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-14d6f44080524bb3b069d14fdea49de9.png)
---
## 4.抽取方法内的局部变量 
**1、快捷键CTRL+ALT+V (将要抽取为局部变量选中)**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-2eb49b9e4d254b91873ff9a395ee208e.png)
**2、抽取后的效果 （方法内多次get同一属性时建议抽取为局部变量）**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-52f52143c97c45feae119896258c6197.png)
---
## 5.抽取变量为方法参数 
**1、选中变量 快捷键 CTRL+ALT+P**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-7a5a5020412b4ef980afdc0ee6539e18.png)
**2、为参数命名（所有调用此方法的方法都会修改）**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-4ff8af9c78834e2c9318fedd33d0bf62.png)
---
## 6.给方法添加参数 
**1、快捷键CTRL+F6 输入添加参数的类型、名称和默认值**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-0c3d5322ac7346768f3b5d9e53ad1c7c.png)
![image.png](http://39.106.177.179:8090/upload/2021/03/image-c1cc241cdcaf4851bd30a0308153e624.png)
**2、有多个方法调用此方法时一键修改**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-aea41b2400d744e6b3c4561077ee6f91.png)
---
## 7.重构命名 
**1、选中要重构的名字 快捷键SHIFT+F6**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-39c50f6b5135483b9516042e43b4bc43.png)
**2、修改名称 回车确认**
![image.png](http://39.106.177.179:8090/upload/2021/03/image-693b13db62884dc289b79a7f6fa3fc1e.png)