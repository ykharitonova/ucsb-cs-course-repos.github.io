---
topic: "Basic Setup: W19 and later"
desc: "Basic Setup for a new course"
category_prefix: "Basic Setup: "
indent: true
---

# Designed for Github Pages

This format is designed to be used with Github Pages, a web publishing service integrated with Github.   

Some advantages of Github pages:
* Professionally-managed free hosting by Github
* Changes are automatically reflected in the website shortly after you do a push your changes to the master branch of the associated repo

# Do I have to use Github pages?

No.  You can use the format described here, and still host the pages anywhere.  For instance, you could still host the pages on the UCSB CS department webserver.   That's also an option you can use if/when there is a (rare) github.com outage.

To publish the site to, for example, `https://cs.ucsb.edu/~cs111`, you would
* Set up your system to be able run Jekyll Locally
* Change the site url in `_config.yml` from `https://github.com/ucsb-cs111.github.io` to `https://cs.ucsb.edu/`
* Change the `baseurl`
   * In the main site, from `/` to `/~cs111`
   * In the quarter specific site from `/w19` to `/~cs111/w19`
* Run the provided `./setup.sh` and `./jekyll.sh` scripts once in each of the two repos
* Copy the contents of `_site` from each of the two repos to the `public_html` directories

The catch is that you would then need to do this again each time you make changes to the Github repos.   That could get onerous.  It may be possible to do some automation of that step; but to be honest, that's what Github Pages has already done.  So unless there is a compelling reason to *NOT* use Github pages, that's the recommended approach.

# Organization, Main Repo, Instance Repo

For each course website (e.g. `ucsb-cs8`, `ucsb-cs24`, `ucsb-cs111`, etc.), we do the following:
* Set up a github organization for the course, e.g. `ucsb-cs111`
* Set up a repo with exactly the name `ucsb-cs111.github.io` within that organization
   * This is a naming convention for Github Pages; the website for a user or organization has the url `name.github.io`
   * A repo with the name `name.github.io` is automatically published to Github Pages
   * This repo will contain information about the course that DOES NOT VARY from quarter to quarter, e.g. explanations of course content, tutorials, useful links, etc.
   * We'll call this the "main repo" or the "parent repo" for the course.
* Set up a separate repo for each of the course offerings.  
   * For example, within the `ucsb-cs111` organization, you would set up a repo simply named `w19` for the Winter 2019 offering, and a repo named `s19` for the Spring 2019 offering (naming conventions for multiple instances of a course in the same quarter are explained below.)
   * These repos (e.g. `w19` and `s19`) are the "course offering" or "course instance" repos. 
   * Any information that is specific to a particular course offering (e.g. assignments, syllabus, exam info, course policies, lecture notes) goes here.
   
# Tying it together: `_data/navigation.yml`

To the end user (i.e. the student) the website appears as one seamless website.   This is accomplished by keeping the navigation consistent.   The file `_data/navigation.yml` allows the instructor to customize the global navigation for site and keep it consistent. (We'll describe those details later.)

# Summary
   
A course website in this format is 

| Github Repo | URL | Description |
|---------|-------------|----|
| <https://github.com/ucsb-cs56/ucsb-cs56.github.io> | <https://ucsb-cs56.github.io> |  Material that goes with the course permanently, e.g. lessons, tutorials, etc. |
| <https://github.com/ucsb-cs56/f18> | <https://ucsb-cs56.github.io/f18> | Material that is specific to a particular quarter and instructor, e.g. syllabus, homework assignments, labs, exams, seating charts, etc. |

When transitioning to a new quarter, a new repo is created for that quarter, e.g.

| Github Repo | URL | Description |
|---------|-------------|---|
| <https://github.com/ucsb-cs56/ucsb-cs56.github.io> | <https://ucsb-cs56.github.io> |  Material that goes with the course permanently, e.g. lessons, tutorials, etc. |
| <https://github.com/ucsb-cs56/f18> | <https://ucsb-cs56.github.io/f18> | Material that is specific to a particular quarter and instructor, e.g. syllabus, homework assignments, labs, exams, seating charts, etc. |

When setting up a brand new course for the first time, you'll create one new github organizations, e.g.

* ucsb-cs111

All of the repos for websites for that course will go into this organization.  For example:

* <https://github.com/ucsb-cs111/ucsb-cs111.github.io>
* <https://github.com/ucsb-cs111/w19>
* <https://github.com/ucsb-cs111/s19>
* etc.


If there are multiple instructors teaching multiple instances of a particular course in a particular quarter, the naming convention is to add the instructor's last name, e.g.

* <https://github.com/ucsb-cs16/ucsb-cs16.github.io>
* <https://github.com/ucsb-cs16/f18-mirza>
* <https://github.com/ucsb-cs16/f18-nichols>
* etc.


If there are multiple sections of a course with the same instructor in the same quarter, the lecture section day or time can be used to differentiate, if separate websites are desired.

* <https://github.com/ucsb-cs16/ucsb-cs16.github.io>
* <https://github.com/ucsb-cs16/w19-330> for the 3:30pm TR section
* <https://github.com/ucsb-cs16/w19-630> for the 6:30pm TR section
* etc.

You should add a `.gitignore` file customzied for Jekyll based on these instructions: [/topics/gitignore/](/topics/gitignore/)

# Setting up the main course website (e.g. `ucsb-cs111.github.io`)

You should now have a course organization (e.g. `ucsb-cs111`) and an empty course repo (e.g. `ucsb-cs111.github.io`)

You should use an exisiting course repo as a model, e.g. <https://github.com/ucsb-cs48/ucsb-cs48.github.io>

We'll go over the files you need, one at a time.

## Plumbing for Jekyll/Github

The following files are ones that you should copy over "as is".    They only need to be updated if/when the preferred Ruby version changes (e.g. from `2.5.1` to a later version).   While I've specified the purpose of the files below for documentation, you don't need to look into those details at this moment unless you really want to.

<style>
div.table-first-col-wide td:first-of-type { 
  width: 15em; 
}

</style>

<div class="table-first-col-wide">

| File to copy | Purpose |
|--------------|---------|
| `Gemfile`    | This is needed for any Ruby application; it specifies the version of Ruby you are using, and what Ruby gems your application depends on. |
| `.gitignore`    | This helps keep your repo tidy by telling github which files to ignore when committing to github |

The following additional file are needed only for running locally, and/or testing the site.   You can get by without them, but they are highly recommended.

| File to copy | Purpose |
|--------------|---------|
| `setup.sh`   | This is a shell script that does the `bundle install` step for setting up Ruby to run locally for testing purposes | 
| `jekyll.sh`   | This is a shell script that does the `bundle exec jekyll serve` step; if you run `setup.sh` once, then you can run this script to test updates to your website locally before pushing to Github. | 
| `.travis.yml` | Allows you to set up automated testing via <https://travis-ci.org>; this runs tests and help you find and debug configuration errors in your site as it's pushed to Github Pages. | 

</div>

## Configure the basic information for your course in `_config.yml`

Setting up the `_config.yml` is such a crucial topic, that we 
have factored it out into its own page here: [/topics/config_yml](/topics/config_yml/)

This is where you'll set most of the defaults for your course.

Example:

```yml
course: "CS111"
qtr: "F17"
quarter: "Fall 2017"
...
```

# Create `index.md` (home page)

A minimal `_index.md` looks like this for a main course website:

```
---
title: UCSB CS111
---

# {{raw}}{{site.course}}&mdash;{{site.course_title}}{{endraw}}

```

And like this for a course website:

```
---
title: UCSB CS111 W19
---

# {{site.course}} {{site.qtr}}&mdash;{{site.course_title}}

```
