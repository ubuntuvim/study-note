```
--------------------------------------------------------------------------------
---查询表中的索引名称--唯一索引
select COUNT(*)  INTO INDEXNUM
  from user_indexes
 where table_name = '表名'
   AND INDEX_NAME = '索引名称';
  AND UNIQUENESS='UNIQUE/NOUNIQUE';

---删除一般索引   
drop index 索引名称;
--删除唯一索引
alter table 表名 drop constraint 索引名称
 
 
----------------------------------------------------------------------------------
---查询表中列
SELECT count(*) INTO COLUMNNUM FROM USER_TAB_COLUMNS where TABLE_NAME='表名' AND COLUMN_NAME='列名';
--增加一列：

   alter table 表名 add 列名 varchar2(10);

--修改一列：

   alter table 表名 modify 列表 varchar2(20);

--删除一列：

alter table 表名 drop column 列名;

-----------------------------------------------------------------------------------------------------
---查询表的约束
select count(*) INTO constraintnum from user_cons_columns cl where cl.constraint_name = '约束名称' AND TABLE_NAME='表名';

--添加一个主键约束  
alter table 表名 add constraint 约束名 primary key (列名);  

alter table 约束基于的表名 drop constraint 约束名;

--重名约束
alter table table_name rename constraint 旧的约束名称 to 新的约束名称;

--约束有效/无效
alter table table_name enable/disable constraint 约束名称;

----------------------------------------------------------------------------------
---查询该表是否存在
select  count(*) INTO TABLENUM from   user_tables WHERE TABLE_NAME='表名';
--删除表
drop table 表名;



----模板中运行-----
DECLARE 
 INDEXNUM NUMBER;
 begin
 INDEXNUM:=0;
 
 select COUNT(*)  INTO INDEXNUM
  from user_indexes
 where table_name = 'UEM_EMP'
   AND INDEX_NAME = 'UI_UEM_EMP'
   AND UNIQUENESS='UNIQUE';
   
 if INDEXNUM>0 THEN
 Execute immediate 'alter table 表名 drop constraint 索引名称';
 
  END IF;
  
 if INDEXNUM=0 then
  Execute immediate 'ALTER table 表名 add constraint UI_UEM_EMP unique (列名)
  using index 
  tablespace KDBASE_DATA
  pctfree 10
  initrans 2
  maxtrans 255
  storage
  (
    initial 64K
    next 1M
    minextents 1
    maxextents unlimited
  )';
  end if;
  end;
  
```  