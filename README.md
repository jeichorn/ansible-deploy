Role Name
=========

Simple Ansible Deployment role for deploying from github, using the releases api + tarballs

Some concepts and code have been borrowed from https://github.com/ansistrano/deploy

Flow
------------
* Grab Release metadata from github
* If a local build is needed
  * Download newest release tarball
  * Extract tarball to local build
  * Run build hooks
* If release version doesn't exist at dfg_deploy_to
  * Setup deploy dir
  * Rsync code from local to remote temp (this speeds up rsync)
  * Copy from remote temp to release
  * Swap symlink
  * Prune old releases

Role Variables
--------------

```yaml
- vars:
  dfg_github_token: TOKEN # Oauth token generated at https://github.com/settings/tokens
  dfg_github_repo: username/repo
  dfg_build_root: "/tmp/dfg_build" # local build path
  dfg_deploy_to: "/var/www/my-app" # Base path to deploy to
  dfg_keep_releases: 0 # Releases to keep after a new deployment, 0 to not prune
  dfg_build_hook: "{{ playbook_dir }}/<your-deployment-config>/build-tasks.yml"
  dfg_version_dir: "releases" # Folder name for the releases
  dfg_current_dir: "current" # Softlink name for the current release
  dfg_shared_paths: [] # Shared paths to symlink to release dir
```

Example Playbook
----------------
```yaml
    - hosts: servers
      vars:
        dfg_github_token: TOKEN
        dfg_github_repo: username/repo
        dfg_deploy_to: "/var/www/my-app"
        dfg_keep_releases: 2
      roles:
         - { role: jeichorn.deploy-fromgithub }
```

License
-------

MIT

Author Information
------------------

Joshua Eichorn http://blog.joshuaeichorn.com
