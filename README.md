![Build Status](https://gitlab.com/pages/gitbook/badges/master/build.svg)

---

Example [GitBook] website using GitLab Pages.

This project uses an orphan branch (`pages`) to host the book. 
This way, you can have your project's code in the `master` branch and use `pages` for accommodating only your website content.
We wouldn't be able to have a `README.md` in the `master` branch with this instructions if we weren't deploying the site from `pages`.

Learn more about GitLab Pages at https://pages.gitlab.io and the official
documentation http://doc.gitlab.com/ee/pages/README.html.

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [GitLab CI](#gitlab-ci)
- [Building locally](#building-locally)
- [GitLab User or Group Pages](#gitlab-user-or-group-pages)
- [Did you fork this project?](#did-you-fork-this-project)
- [Troubleshooting](#troubleshooting)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## GitLab CI

This project's static Pages are built by [GitLab CI][ci], following the steps
defined in [`.gitlab-ci.yml`](.gitlab-ci.yml):

```yaml
image: node:4.2.2

cache:
  paths: 
    - node_modules/ 

before_script:
  - npm install gitbook-cli -g
  - gitbook fetch latest
  
pages:
  stage: deploy
  script:
    - gitbook build
  artifacts:
    paths:
      - public 
  only:
    - pages # this job will affect only the 'pages' branch
```

## Building locally

To work locally with this project, you'll have to follow the steps below:

1. Fork, clone or download this project
1. Track `pages` branch: `git checkout --track origin/pages`
1. [Install][] GitBook `npm install gitbook-cli -g`
1. Fetch GitBook's latest stable version `gitbook fetch latest`
1. Preview your project: `gitbook serve`
1. Add content
1. Generate the website: `gitbook build` (optional)
1. Push your changes to the pages branch: `git push origin pages`

Read more at GitBook's [documentation][].

## GitLab User or Group Pages

To use this project as your user/group website, you will need one additional
step: just rename your project to `namespace.gitlab.io`, where `namespace` is
your `username` or `groupname`. This can be done by navigating to your
project's **Settings**.

Read more about [user/group Pages][userpages] and [project Pages][projpages].

## Did you fork this project?

If you forked this project for your own use, please go to your project's
**Settings** and remove the forking relationship, which won't be necessary
unless you want to contribute back to the upstream project.

## Troubleshooting

1. CSS is missing! That means two things:

    Either that you have wrongly set up the CSS URL in your templates, or
    your static generator has a configuration option that needs to be explicitly
    set in order to serve static assets under a relative URL.

[ci]: https://about.gitlab.com/gitlab-ci/
[GitBook]: https://www.gitbook.com/
[install]: http://toolchain.gitbook.com/setup.html
[documentation]: http://toolchain.gitbook.com
[userpages]: http://doc.gitlab.com/ee/pages/README.html#user-or-group-pages
[projpages]: http://doc.gitlab.com/ee/pages/README.html#project-pages