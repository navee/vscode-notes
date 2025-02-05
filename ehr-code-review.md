# Ehr-Code-Review

从本月开始，后端将进行严格的代码审核，出现如下情况的按规定进行扣分：
1、未按照需求删除代码中没有使用的入参、出参、接口的扣除0.5分/每接口
2、接口未正确进行数据格式、逻辑校验将内容新增、更新到数据库的扣除1分/每接口
3、未按照原型实现，导致不能满足需求完整逻辑的，按照原需求分数扣除25%-50%/每需求
4、在老需求上进行功能修改的，有责对历史代码校验逻辑进行检查和修复
     提出过整改但是未按要求修复且将代码发布到生产的，扣除整改开发人2分/每接口，并由整改人继续整改
     未提出过整改的，新需求开发人可以提出增加相应工时对历史代码进行整改
5、在没有特殊性能要求、多表联合查询结果的情况下Mapper直接查询并返回DTO的扣除1分/每接口


后端开发要正确的认识数据校验：无论调用方知不知道校验规则，后端必须进行了数据格式校验和数据逻辑校验才能写入数据库，出现如下情况，后端应该考虑如何进行数据校验：
1、某枚举字段有1、2、3，调用方传了4
2、某字段是用户id，调用方传了一个不存在的id
3、根据某项配置，某字段的值是另外一个配置项中选定的值，例如入职时岗位是根据前端选定的部门来决定的，调用方传了一个这个部门不存在的岗位
4、若某字段要求长度为10，格式为yyyy-MM-dd，调用方传了yyyyMMdd
5、某个已经关联的数据现在不存在了，例如user_info.dept_id，使用dept_id时应该判断这个id不能存在了要如何处理

# ehr oa解耦处理方案
## 前置处理
1. ehr中部门、岗位的oaid重复数据处理，增加唯一索引字段
2. ehrtooa从ehrid迁移到使用oaid
3. ehr数据迁移涉及的功能、表、sql准备
## 其他部门协调
1. erp：用到了岗位ehrid，迁移方案待定（韩若尘）
2. bi: 用到了岗位ehrid，设计数据较少，已同意先迁移到oaid（杨帆）
## 数据迁移
1. 关闭ehr部门、岗位编辑权限（对外同步理论上也是没有了）
2. ehr数据迁移
3. 其他部门数据迁移
3. 测试检查迁移后相关功能是否正常
