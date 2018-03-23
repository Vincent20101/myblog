gerrit修改git路径，同时也要相应的修改数据库里面的路径。

```                     
 Schema |            Name             |   Type   |  Owner  
--------+-----------------------------+----------+---------
 public | account_diff_preferences    | table    | gerrit2
 public | account_external_ids        | table    | gerrit2
 public | account_group_by_id         | table    | gerrit2
 public | account_group_by_id_aud     | table    | gerrit2
 public | account_group_id            | sequence | gerrit2
 public | account_group_members       | table    | gerrit2
 public | account_group_members_audit | table    | gerrit2
 public | account_group_names         | table    | gerrit2
 public | account_groups              | table    | gerrit2
 public | account_id                  | sequence | gerrit2
 public | account_patch_reviews       | table    | gerrit2
 public | account_project_watches     | table    | gerrit2
 public | account_ssh_keys            | table    | gerrit2
 public | accounts                    | table    | gerrit2
 public | change_id                   | sequence | gerrit2
 public | change_message_id           | sequence | gerrit2
 public | change_messages             | table    | gerrit2
 public | changes                     | table    | gerrit2
 public | patch_comments              | table    | gerrit2
 public | patch_set_ancestors         | table    | gerrit2
 public | patch_set_approvals         | table    | gerrit2
 public | patch_sets                  | table    | gerrit2
 public | schema_version              | table    | gerrit2
 public | starred_changes             | table    | gerrit2
 public | submodule_subscriptions     | table    | gerrit2
 public | system_config               | table    | gerrit2
(26 rows)
```

sql语句

```
select dest_project_name from changes where dest_project_name like '%k2/%';
```

```
ssh gerrit.mage.com -p 29418 gerrit flush-caches --all
```

```
update changes set dest_project_name=replace(dest_project_name, "k2/", "git/android/")  where dest_project_name like '%k2/platform/manifest%';
```


```
update changes set dest_project_name=replace(dest_project_name, 'git/shared/tools/tools_scripts/tools/tools_scripts', 'git/shared/tools/tools_scripts') where dest_project_name like '%git/shared/tools/tools_scripts/tools/tools_scripts%';
```








