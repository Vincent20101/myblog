
# gerrit 2.12.x 版本 数据库表介绍 之 postgresql


下面是2.12.x版本的 数据库表
```bash

                     List of relations
 TABLE_SCHEM | TABLE_NAME                  | TABLE_TYPE
 ------------+-----------------------------+-----------
 public      | account_external_ids        | TABLE
 public      | account_group_by_id         | TABLE
 public      | account_group_by_id_aud     | TABLE
 public      | account_group_members       | TABLE
 public      | account_group_members_audit | TABLE
 public      | account_group_names         | TABLE
 public      | account_groups              | TABLE
 public      | account_patch_reviews       | TABLE
 public      | account_project_watches     | TABLE
 public      | account_ssh_keys            | TABLE
 public      | accounts                    | TABLE
 public      | change_messages             | TABLE
 public      | changes                     | TABLE
 public      | patch_comments              | TABLE
 public      | patch_set_approvals         | TABLE
 public      | patch_sets                  | TABLE
 public      | schema_version              | TABLE
 public      | starred_changes             | TABLE
 public      | submodule_subscriptions     | TABLE
 public      | system_config               | TABLE

```


# accounts表

这个表是保存账号相关信息的
```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='accounts';

 ordinal_position | column_name                   | data_type                | column_default
 -----------------+-------------------------------+--------------------------+---------------
 1                | registered_on                 | timestamp with time zone | NULL
 2                | full_name                     | character varying        | NULL
 3                | preferred_email               | character varying        | NULL
 5                | maximum_page_size             | smallint                 | 0
 6                | show_site_header              | character                | 'N'::bpchar
 7                | use_flash_clipboard           | character                | 'N'::bpchar
 8                | download_url                  | character varying        | NULL
 9                | download_command              | character varying        | NULL
 10               | copy_self_on_email            | character                | 'N'::bpchar
 11               | date_format                   | character varying        | NULL
 12               | time_format                   | character varying        | NULL
 13               | relative_date_in_change_table | character                | 'N'::bpchar
 14               | diff_view                     | character varying        | NULL
 15               | size_bar_in_change_table      | character                | 'N'::bpchar
 16               | legacycid_in_change_table     | character                | 'N'::bpchar
 17               | review_category_strategy      | character varying        | NULL
 18               | mute_common_path_prefixes     | character                | 'N'::bpchar
 19               | inactive                      | character                | 'N'::bpchar
 20               | account_id                    | integer                  | 0


select account_id, full_name, preferred_email from accounts where account_id=1000000;
 account_id | full_name  |     preferred_email      
------------+------------+--------------------------
    1000000 | xxxxxxx xx | xxxxxx.xx@xxxxxxxxx.com

account_id 是个数字，第一次登录gerrit的时候会自动分配一个，这个基本上是账号的唯一标识了。第一个登录的账号id是1000000，后续的依次累加。
full_name 是 全名，显示的名称，和登录时候填写的账号可以是不一样的，这里也可以是中文的，如果是用的ldap认证的，这里就是ldap里面的全名。
preferred_email 是邮箱地址，如果是用的ldap认证的，这里就是ldap里面的邮箱。

inactive 表示这个账号是否可用，取值 'N' 和 'Y', N表示不能使用，不能登录，不能访问ssh下代码等。表示账号被禁用。离职的员工可用把账号禁用了。



```

# account_external_ids 表

```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_external_ids';

 ordinal_position | column_name   | data_type         | column_default
 -----------------+---------------+-------------------+----------------------
 1                | account_id    | integer           | 0
 2                | email_address | character varying | NULL
 3                | password      | character varying | NULL
 4                | external_id   | character varying | ''::character varying
```


# account_groups 表
这个是群组相关的表。还有个account_group_names表和他对应的，这个表只存放了群组名称。
```sql

SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_groups';          

 ordinal_position | column_name      | data_type         | column_default
 -----------------+------------------+-------------------+----------------------
 1                | name             | character varying | ''::character varying
 2                | description      | text              | NULL
 3                | visible_to_all   | character         | 'N'::bpchar
 4                | group_uuid       | character varying | ''::character varying
 5                | owner_group_uuid | character varying | ''::character varying
 6                | group_id         | integer           | 0


```
# account_group_names 表

```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_group_names';

 ordinal_position | column_name | data_type         | column_default
 -----------------+-------------+-------------------+----------------------
 1                | group_id    | integer           | 0
 2                | name        | character varying | ''::character varying

```
# account_group_by_id 表

```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_group_by_id';

 ordinal_position | column_name  | data_type         | column_default
 -----------------+--------------+-------------------+----------------------
 1                | group_id     | integer           | 0
 2                | include_uuid | character varying | ''::character varying

```

# account_group_members 表
这个表记录了哪些账号在哪个群组里面。
```sql

SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_group_members';

 ordinal_position | column_name | data_type | column_default
 -----------------+-------------+-----------+---------------
 1                | account_id  | integer   | 0
 2                | group_id    | integer   | 0


```

# account_patch_reviews 表
这个表格记录了某个账号review了哪些文件，file_name 就是review的文件名称，
```sql

SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_patch_reviews';

 ordinal_position | column_name  | data_type         | column_default
 -----------------+--------------+-------------------+----------------------
 1                | account_id   | integer           | 0
 2                | change_id    | integer           | 0
 3                | patch_set_id | integer           | 0
 4                | file_name    | character varying | ''::character varying


```
# account_project_watches 表

```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_project_watches';

 ordinal_position | column_name              | data_type         | column_default
 -----------------+--------------------------+-------------------+----------------------
 1                | notify_new_changes       | character         | 'N'::bpchar
 2                | notify_all_comments      | character         | 'N'::bpchar
 3                | notify_submitted_changes | character         | 'N'::bpchar
 4                | notify_new_patch_sets    | character         | 'N'::bpchar
 5                | notify_abandoned_changes | character         | 'N'::bpchar
 6                | account_id               | integer           | 0
 7                | project_name             | character varying | ''::character varying
 8                | filter                   | character varying | ''::character varying



```


# account_ssh_keys 表
```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='account_ssh_keys';       

 ordinal_position | column_name    | data_type | column_default
 -----------------+----------------+-----------+---------------
 1                | ssh_public_key | text      | ''::text
 2                | valid          | character | 'N'::bpchar
 3                | account_id     | integer   | 0
 4                | seq            | integer   | 0

```

# changes 表

```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='changes';         

 ordinal_position | column_name          | data_type                | column_default
 -----------------+----------------------+--------------------------+----------------------
 1                | change_key           | character varying        | ''::character varying
 2                | created_on           | timestamp with time zone | NULL
 3                | last_updated_on      | timestamp with time zone | NULL
 4                | owner_account_id     | integer                  | 0
 5                | dest_project_name    | character varying        | ''::character varying
 6                | dest_branch_name     | character varying        | ''::character varying
 7                | status               | character                | ' '::bpchar
 8                | current_patch_set_id | integer                  | 0
 9                | subject              | character varying        | ''::character varying
 10               | topic                | character varying        | NULL
 11               | original_subject     | character varying        | NULL
 12               | row_version          | integer                  | 0
 13               | change_id            | integer                  | 0
 14               | submission_id        | character varying        | NULL

```

# change_messages 表

```sql

SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='change_messages';

 ordinal_position | column_name           | data_type                | column_default
 -----------------+-----------------------+--------------------------+----------------------
 1                | author_id             | integer                  | NULL
 2                | written_on            | timestamp with time zone | NULL
 3                | message               | text                     | NULL
 4                | patchset_change_id    | integer                  | NULL
 5                | patchset_patch_set_id | integer                  | NULL
 6                | change_id             | integer                  | 0
 7                | uuid                  | character varying        | ''::character varying


```


# patch_sets 表
```sql

gerrit> SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='patch_sets';     
 ordinal_position | column_name         | data_type                | column_default
 -----------------+---------------------+--------------------------+---------------
 1                | revision            | character varying        | NULL
 2                | uploader_account_id | integer                  | 0
 3                | created_on          | timestamp with time zone | NULL
 4                | draft               | character                | 'N'::bpchar
 5                | change_id           | integer                  | 0
 6                | patch_set_id        | integer                  | 0
 7                | groups              | character varying        | NULL
 8                | push_certficate     | text                     | NULL

```

# patch_comments 表
```sql

SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='patch_comments';
 ordinal_position | column_name           | data_type                | column_default
 -----------------+-----------------------+--------------------------+----------------------
 1                | line_nbr              | integer                  | 0
 2                | author_id             | integer                  | 0
 3                | written_on            | timestamp with time zone | NULL
 4                | status                | character                | ' '::bpchar
 5                | side                  | smallint                 | 0
 6                | message               | text                     | NULL
 7                | parent_uuid           | character varying        | NULL
 8                | range_start_line      | integer                  | NULL
 9                | range_start_character | integer                  | NULL
 10               | range_end_line        | integer                  | NULL
 11               | range_end_character   | integer                  | NULL
 12               | change_id             | integer                  | 0
 13               | patch_set_id          | integer                  | 0
 14               | file_name             | character varying        | ''::character varying
 15               | uuid                  | character varying        | ''::character varying

```

# patch_set_approvals 表
```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='patch_set_approvals';
 ordinal_position | column_name  | data_type                | column_default
 -----------------+--------------+--------------------------+----------------------
 1                | value        | smallint                 | 0
 2                | granted      | timestamp with time zone | NULL
 3                | change_id    | integer                  | 0
 4                | patch_set_id | integer                  | 0
 5                | account_id   | integer                  | 0
 6                | category_id  | character varying        | ''::character varying

```
# starred_changes 表

```sql

SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='starred_changes';    
 ordinal_position | column_name | data_type | column_default
 -----------------+-------------+-----------+---------------
 1                | account_id  | integer   | 0
 2                | change_id   | integer   | 0

```


# submodule_subscriptions 表
```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='submodule_subscriptions';
 ordinal_position | column_name                | data_type         | column_default
 -----------------+----------------------------+-------------------+----------------------
 1                | submodule_project_name     | character varying | ''::character varying
 2                | submodule_branch_name      | character varying | ''::character varying
 3                | super_project_project_name | character varying | ''::character varying
 4                | super_project_branch_name  | character varying | ''::character varying
 5                | submodule_path             | character varying | ''::character varying
 
```

# schema_version 表
```sql

SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='schema_version'; 
 ordinal_position | column_name | data_type         | column_default
 -----------------+-------------+-------------------+----------------------
 1                | version_nbr | integer           | 0
 2                | singleton   | character varying | ''::character varying



```

# system_config 表
```sql
SELECT ordinal_position,column_name, data_type, column_default FROM information_schema.columns WHERE table_name ='system_config'; 
 ordinal_position | column_name                | data_type         | column_default
 -----------------+----------------------------+-------------------+----------------------
 1                | register_email_private_key | character varying | NULL
 2                | site_path                  | character varying | NULL
 3                | admin_group_id             | integer           | NULL
 4                | anonymous_group_id         | integer           | NULL
 5                | registered_group_id        | integer           | NULL
 6                | wild_project_name          | character varying | NULL
 7                | batch_users_group_id       | integer           | NULL
 8                | owner_group_id             | integer           | NULL
 9                | admin_group_uuid           | character varying | NULL
 10               | batch_users_group_uuid     | character varying | NULL
 11               | singleton                  | character varying | ''::character varying

```
















下面记录一下对表格数据进行清理的sql语句

```sql
account_group_by_id_aud 表 和 account_group_members_audit 表 可以直接清空，
里面记录的是添加账号到某个组之类的历史操作。没什么用的。

account_patch_reviews 表格记录的是 每个账号 是 review 某个文件的 操作记录，也没什么太大作用。
如果你打开review了某个文件，这个文件前面有个对号会勾上的。

account_project_watches 表是记录每个账号监听哪个项目有提交发邮件的 也可以清空。


changes，表是记录提交的一个主表。
change_messages，patch_comments，patch_set_approvals，patch_sets这个几个表都要个change_id列是引用了changes表的。

change_messages 表是 系统自动打上的注释类的信息的。
patch_comments 表是 员工自己写的注释类信息的。
patch_set_approvals 表是记录review、verify、submit历史信息的。

patch_sets 表是记录 revision，change_id, account_id 对应的历史信息的。


starred_changes 表记录是 加星的 提交，也没什么用，可以直接清空。

```

```sql

select account_id
from accounts where 
preferred_email  like '%cxxxxxxxxx.com%' or  
preferred_email  like '%kxxxxxxxxx.com%' or 
preferred_email  like '%yxxxxxxxxx.com%' or 
preferred_email  like '%zxxxxxxxxx.com%' or 
preferred_email  is null 
order by account_id

select * from account_group_names where group_id not in (select group_id from account_groups)

#删除不需要账号下面的提交，先删除changes表格中的基类，其他的引用了changes表格的使用not in 来删除
select * from changes where owner_account_id in (
    select account_id 
    from accounts where 
    preferred_email  like '%cxxxxxxxxx.com%' or  
    preferred_email  like '%kxxxxxxxxx.com%' or 
    preferred_email  like '%yxxxxxxxxx.com%' or 
    preferred_email  like '%zxxxxxxxxx.com%' or 
    preferred_email  is null
)

#删除不需要账号下面的提交
select * from change_messages where author_id in (
    select account_id 
    from accounts where 
    preferred_email  like '%cxxxxxxxxx.com%' or  
    preferred_email  like '%kxxxxxxxxx.com%' or 
    preferred_email  like '%yxxxxxxxxx.com%' or 
    preferred_email  like '%zxxxxxxxxx.com%' or 
    preferred_email  is null
)
select * from change_messages where  change_id not in (select change_id from changes)

#删除不需要账号下面的提交
select * from patch_comments where author_id in (
    select account_id 
    from accounts where 
    preferred_email  like '%cxxxxxxxxx.com%' or  
    preferred_email  like '%kxxxxxxxxx.com%' or 
    preferred_email  like '%yxxxxxxxxx.com%' or 
    preferred_email  like '%zxxxxxxxxx.com%' or 
    preferred_email  is null
)
select * from patch_comments where  change_id not in (select change_id from changes)

#删除不需要账号下面的提交
select * from patch_set_approvals where account_id in (
    select account_id 
    from accounts where 
    preferred_email  like '%cxxxxxxxxx.com%' or  
    preferred_email  like '%kxxxxxxxxx.com%' or 
    preferred_email  like '%yxxxxxxxxx.com%' or 
    preferred_email  like '%zxxxxxxxxx.com%' or 
    preferred_email  is null
)
select * from patch_set_approvals where  change_id not in (select change_id from changes)

#删除不需要账号下面的提交
select * from patch_sets where account_id in (
    select account_id 
    from accounts where 
    preferred_email  like '%cxxxxxxxxx.com%' or  
    preferred_email  like '%kxxxxxxxxx.com%' or 
    preferred_email  like '%yxxxxxxxxx.com%' or 
    preferred_email  like '%zxxxxxxxxx.com%' or 
    preferred_email  is null
)
select * from patch_sets where  change_id not in (select change_id from changes)


#删除不需要账号下面的提交
select * from account_external_ids where account_id in (
    select account_id 
    from accounts where 
    preferred_email  like '%cxxxxxxxxx.com%' or  
    preferred_email  like '%kxxxxxxxxx.com%' or 
    preferred_email  like '%yxxxxxxxxx.com%' or 
    preferred_email  like '%zxxxxxxxxx.com%' or 
    preferred_email  is null
)

#删除不需要账号下面的提交
select * from account_ssh_keys where account_id in (
    select account_id 
    from accounts where 
    preferred_email  like '%cxxxxxxxxx.com%' or  
    preferred_email  like '%kxxxxxxxxx.com%' or 
    preferred_email  like '%yxxxxxxxxx.com%' or 
    preferred_email  like '%zxxxxxxxxx.com%' or 
    preferred_email  is null
)

# 先使用这个查看所有分支名称，然后挑出不需要的分支进行删除
select distinct dest_branch_name from changes order by dest_branch_name 

# 删除不需要的分支的提交
select  *  from changes where 
dest_branch_name like '%refs/heads/alchemy%' or
dest_branch_name like '%refs/heads/ares%' or
dest_branch_name like '%refs/heads/bakcup/%' or
dest_branch_name like '%refs/heads/bak/%' or
dest_branch_name like '%refs/heads/camry%' or
dest_branch_name like '%refs/heads/castor%' or
dest_branch_name like '%refs/heads/civic%' or
dest_branch_name like '%refs/heads/clover%' or
dest_branch_name like '%refs/heads/msm8909%' or
dest_branch_name like '%refs/heads/msm8953%' or
dest_branch_name like '%refs/heads/msm8976%' or
dest_branch_name like '%refs/heads/msm8996%' or
dest_branch_name like '%refs/heads/msm8998%' or
dest_branch_name like '%refs/heads/prague%' or
dest_branch_name like '%refs/heads/razer%' or
dest_branch_name like '%refs/heads/ruby%' or
dest_branch_name like '%refs/heads/victor%' or
dest_branch_name like '%refs/heads/zl%' or
dest_branch_name like '%refs/heads/zs%' or
dest_branch_name like '%refs/hy%' or
dest_branch_name like '%refs/meta/config%'



# 删除 多久之前的 处于abandon状态的提交
select * from changes  where status='A' and created_on < '2018-01-01' order by created_on









```