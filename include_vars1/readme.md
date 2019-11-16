In the last exercise:
> Now, if we can say that commonrole will always have to be called before subrole1 is called, then we can add a dependency to 
subrole1. This will be achieved in the next section.

So, let's do that now. To change our current code to enable automatically loading commonrole before subrole1, we have to make the
following changes:
- add commonrole as a dependency to subrole1
- remove the call to commonrole in the playbook
- test if commonrole is indeed loaded automatically before loading subrole1. 

### Add commonrole as a dependency to role1
Open ./roles/subrole1/meta/main.yml, and all the way at the bottom, change:
```yml
dependencies: []
  # List your role dependencies here, one per line. Be sure to remove the '[]' above,
  # if you add dependencies to this list.
```
to:
```yml
dependencies:
  # List your role dependencies here, one per line. Be sure to remove the '[]' above,
  # if you add dependencies to this list.
  - role: commonrole
```

### Remove call to commonrole in the playbook
Edit ./test.yml to:
```yml
- hosts: localhost
  tasks:
#    - include_role:
#      name: commonrole
    - include_role:
        name: subrole1
```

### Test new code
```bash
ansible-playbook -i localhost test.yml -v
```
```
PLAY [localhost] ***********************************************************************************

TASK [Gathering Facts] *****************************************************************************
ok: [localhost]

TASK [include_role : subrole1] *********************************************************************

TASK [commonrole : Test 1 - Load default value] ****************************************************
ok: [localhost] => {
    "msg": "Mr. Potato"
}

TASK [commonrole : Test 2 - Export var] ************************************************************
ok: [localhost] => {"ansible_facts": {"rb_commonrole_name": "Mr. Potato"}, "changed": false}

TASK [subrole1 : Test 3 - Import var] **************************************************************
ok: [localhost] => {
    "msg": "Mr. Potato"
}

PLAY RECAP *****************************************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
``
