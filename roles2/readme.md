In the last exercise:
> Now, if we can say that commonrole will always have to be called before subrole1 is called, then we can add a dependency to 
subrole1. This will be achieved in the next section.

So, let's do that now.
First step is to add a dependency to subrole1. Then we'll remove the call to commonrole in the playbook, and finally test if 
commonrole is indeed loaded automatically before loading subrole1. 

Adding a dependency is simple enough. Open ./roles/subrole1/meta/main.yml, and all the way at the bottom, change:
```yml
dependencies: []
  # List your role dependencies here, one per line. Be sure>
  # if you add dependencies to this list.
```
to:
```yml
dependencies:
  # List your role dependencies here, one per line. Be sure>
  # if you add dependencies to this list.
  - role: commonrole
```
