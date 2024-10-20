# Ansible Dynamic Assignments and Community Roles

In this project, you will explore dynamic assignments using the `include` module and understand how it differs from static assignments that use the `import` module.

## Key Concepts

### Static Assignment (import)

- Uses the `import` module.
- All tasks are pre-processed when the playbook is parsed.
- Any changes made to the included files during execution will not be reflected.
- Reliable and recommended for most use cases because debugging is easier.

### Dynamic Assignment (include)

- Uses the `include` module.
- Tasks are processed during execution, not during parsing.
- Any changes made to the included files during execution will be considered.
- Useful for environment-specific variables but harder to debug due to its dynamic nature.

## Summary

- **`import` = Static**: Tasks are pre-processed when playbooks are parsed. Changes after parsing are not considered.
- **`include` = Dynamic**: Tasks are processed during execution, and changes during execution are considered.

## Best Practices

- Use static assignments for general playbooks (more reliable).
- Use dynamic assignments for specific cases like environment variables, where runtime changes might be needed.

## Setting Up Dynamic Assignments

### Steps to Set Up Dynamic Assignments

1. **Create a New Branch**

   First, create a new branch in your GitHub repository and call it `dynamic-assignments`:

   ```
   git checkout -b dynamic-assignments
   ```

2. **Set Up Folder Structure**

- Create the dynamic-assignments folder
```
mkdir dynamic-assignments
```

- Inside the dynamic-assignments folder, create the env-vars.yml file
```
touch dynamic-assignments/env-vars.yml
```

- Create a folder for environment variables called env-vars
```
mkdir env-vars
```

- Inside env-vars, create files for each environment
```
touch env-vars/dev.yml
touch env-vars/stage.yml
touch env-vars/uat.yml
touch env-vars/prod.yml
```

- Create the inventory structure with subfolders for each environment
```
mkdir -p inventory/dev
mkdir -p inventory/stage
mkdir -p inventory/uat
mkdir -p inventory/prod
```

- Create the playbooks and static-assignments folders if not already done
```
mkdir playbooks
mkdir static-assignments
```

3. **Add Content to env-vars.yml**

Next, add the following YAML content to env-vars.yml to dynamically include environment-specific variables:
yaml
```
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - alwayys
```


4. **Update the site.yml Playbook**

To include the env-vars.yml dynamically in your playbook, modify your site.yml file to reference it:
yaml
```
---
- hosts: all
  tasks:
    - include: ../dynamic-assignments/env-vars.yml
```
5. **Final Directory Structure**

Your repository should now have the following structure:

```
plaintext
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
│   ├── dev.yml
│   ├── stage.yml
│   ├── uat.yml
│   └── prod.yml
├── inventory
│   ├── dev
│   ├── stage
│   ├── uat
│   └── prod
├── playbooks
│   └── site.yml
└── static-assignments
    ├── common.yml
    └── webservers.yml
```
6. **Commit and Push Changes**

After setting up the structure and adding the files, commit and push your changes to the dynamic-assignments branch:
```
git add .
git commit -m "Set up dynamic assignments and environment variable files"
git push origin dynamic-assignments
```

### Conclusion

You have successfully set up dynamic assignments in your Ansible repository! With these environment-specific variable files, your playbooks can dynamically include the appropriate configuration for each environment.
plaintext


