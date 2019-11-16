>This exercise does not list every step, consult the code in this repo to see exactly how to do that which is described here.

# Including global variables with roles
Any sizeable Ansible project will benefit from putting playbooks into separate folders. However, this will mess with automatically
loading in group_vars, as Ansible can't find the group_vars folder, even when you call your playbook from the root. 
The Ansible dev team has shown to have [no intention of solving this issue](https://github.com/ansible/ansible/issues/12862).

Working around this issue might involve manually [loading the variable files at the beginning of every playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#defining-variables-in-files).
Though this works, if we change the names of any of the files, or want to add extra files that always have to be loaded in, we'll have
to edit all our playbooks to accommodate this change.

Luckily, we can just make a role that will do this for us. We can make this role a dependency for each role that needs global
variables to work, and if we need global variables directly in a playbook, we can call it manually there.



## Creating ansible.cfg
When we start putting playbooks in subdirectories, ansible will not be able to find the roles/ directory anymore. As opposed to 
the group_vars problem, this is solvable. We do this by creating a configuration file called `ansible.cfg` in our ansible root 
directory:

```yml
[defaults]
roles_path = ./roles/
```
## Creating roles/load_config
Look at the previous exercises for how to create empty roles. Just like before, we add some data in roles/load_config/defaults/main.yml and then we use `set_facts` in roles/load_config/tasks/main.yml to load these into scope. We set roles/subrole1/meta/main.yml so that it has load_config as a dependency, and in roles/subrole1/tasks/main.yml we debug some of the config set by
load_config to test whether the variables are loaded.

So, basically the same steps as the last exercise. 

## Move test.yml, and test
We move test.yml to test/test.yml, edit it to debug the variable loaded in by load_config. Then we run it from the root:
```bash
ansible-playbook -i localhost test/test.yml -v
```


